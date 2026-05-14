# Privacy-Preserving Audit Events

OpenParcelBox audit events must describe what happened without storing raw secrets.

The event model connects:

- local credentials from Wiegand, PIN, NFC, fingerprint or API input
- keyring and delegated-key decisions
- opening-right validation
- lock and door state
- remote command attempts
- later MQTT, bridge or cloud event projection

The audit log is not a raw debugging dump. It is a privacy-preserving security record.

## Principles

- Log decisions and state changes, not secrets.
- Credentials are not identities; credentials resolve to keys and actors.
- A remote command is only a request. The local controller remains the authorization authority.
- Lock actuation must be logged as a local decision, not as an external relay command.
- Offline logs must be bufferable and exportable later.
- Retention rules are draft until a concrete firmware/storage profile exists.

## ID Model

Events should reference stable local IDs.

| Field | Meaning |
|---|---|
| `event_id` | Monotonic or UUID-style local event identifier. |
| `device_id` | Stable OpenParcelBox controller/device ID. |
| `parcel_lock_id` | Stable lock/receptacle identity where separate from controller ID. |
| `organization_id` | Local organization/operator context, e.g. `org_local`. |
| `household_id` | Household, flat or recipient group. |
| `actor_id` | User, carrier, admin, service, neighbor or integration actor. |
| `actor_type` | `admin`, `household_user`, `carrier`, `service`, `neighbor`, `integration`, `system`, `unknown`. |
| `key_id` | Local key or subkey used for the decision. |
| `parent_key_id` | Parent/master key ID for delegated keys, if relevant. |
| `credential_id` | Local credential identifier. |
| `credential_type` | `pin`, `wiegand_pin`, `wiegand_card`, `nfc`, `fingerprint`, `api_signature`, `backup_code`, `unknown`. |
| `opening_right_id` | Opening-right ID if one is involved. |
| `request_id` | Remote command request ID or nonce, if relevant. |

For M1, `organization_id` may be `org_local`.

## Minimum Event Envelope

Every event should include:

```json
{
  "event_id": "evt_00001234",
  "schema_version": 1,
  "event_type": "access.granted",
  "timestamp": "2026-05-14T10:44:00Z",
  "device_id": "opb-dev-001",
  "parcel_lock_id": "opb-lock-001",
  "source": "local_reader",
  "result": "success"
}
```

If the device has no trusted wall-clock time, it should still record:

- monotonic sequence number
- boot/session id
- uptime milliseconds
- time status, e.g. `time_unsynced`

Example:

```json
{
  "event_id": "evt_00000042",
  "schema_version": 1,
  "event_type": "door.opened",
  "timestamp": null,
  "time_status": "time_unsynced",
  "boot_id": "boot_0003",
  "uptime_ms": 91833,
  "device_id": "opb-dev-001",
  "parcel_lock_id": "opb-lock-001",
  "source": "door_sensor",
  "result": "observed"
}
```

## Event Types

### Credential Events

| Event | Purpose |
|---|---|
| `credential.received` | A credential-like input was received from Wiegand, NFC, fingerprint, API or admin UI. |
| `credential.unknown` | No local credential match exists. |
| `credential.locked_out` | Credential or source is temporarily blocked due to failed attempts. |
| `credential.created` | A credential was created/enrolled. |
| `credential.disabled` | A credential was disabled. |
| `credential.revoked` | A credential or key was revoked. |

`credential.received` should be rate-limited or omitted in very small M1 logs if it would reveal too much behavior. It must never include raw values.

### Access Decision Events

| Event | Purpose |
|---|---|
| `access.granted` | Local policy allowed the requested action. |
| `access.denied` | Local policy denied the requested action. |
| `access.lockout_started` | Failed attempts triggered lockout. |
| `access.lockout_cleared` | Lockout was cleared by time or admin action. |

Decision reason examples:

- `credential_valid`
- `credential_unknown`
- `wrong_scope`
- `expired`
- `not_yet_valid`
- `revoked`
- `max_uses_reached`
- `lockout_active`
- `remote_unlock_disabled`
- `invalid_command_signature`
- `replay_detected`

### Opening-Right Events

| Event | Purpose |
|---|---|
| `right.issued` | Opening right was created. |
| `right.rendered` | Opening right was rendered as PIN, QR, token or similar. |
| `right.validated` | Opening right was checked. |
| `right.used` | Opening right was consumed. |
| `right.expired` | Opening right expired. |
| `right.revoked` | Opening right was revoked. |
| `right.purged` | Opening right was deleted according to retention policy. |

### Lock and Door Events

| Event | Purpose |
|---|---|
| `lock.unlock_requested` | Local controller decided to request unlock. |
| `lock.unlocked` | Lock reports or assumes unlocked state. |
| `lock.locked` | Lock reports or assumes locked state. |
| `lock.error` | Lock operation failed or ended in unknown state. |
| `door.opened` | Door sensor reports open. |
| `door.closed` | Door sensor reports closed. |
| `door.left_open` | Door remained open beyond expected time. |
| `door.opened_without_grant` | Door opened without preceding valid grant. |

`lock.unlock_requested` must be produced by the local controller, not directly by MQTT, Matter or any external integration.

### Remote Command Events

| Event | Purpose |
|---|---|
| `remote_command.received` | Remote command request was received. |
| `remote_command.accepted` | Remote command passed transport/auth/signature pre-checks. |
| `remote_command.denied` | Remote command failed authentication, authorization or local policy. |
| `remote_command.replay_detected` | Request id/nonce/timestamp was already used or outside policy. |
| `remote_command.executed` | Local controller executed the command after policy decision. |

Remote command events should include `integration_id`, `request_id`, `scope` and `command`, but never long-term secrets or raw signatures where not needed for diagnostics.

### System and Safety Events

| Event | Purpose |
|---|---|
| `system.boot` | Controller booted. |
| `system.config_changed` | Local configuration changed. |
| `system.time_synced` | Device time was synchronized. |
| `system.time_unsynced` | Device time became unavailable or untrusted. |
| `power.low` | Battery or supply voltage is low. |
| `tamper.detected` | Tamper/sabotage input triggered. |
| `ota.started` | OTA update started. |
| `ota.completed` | OTA update completed. |
| `ota.failed` | OTA update failed. |

## M1 Minimum Event Set

M1 should implement or at least reserve these event names:

- `credential.received`
- `credential.unknown`
- `access.granted`
- `access.denied`
- `lock.unlock_requested`
- `lock.unlocked`
- `lock.locked`
- `lock.error`
- `door.opened`
- `door.closed`
- `door.left_open`
- `door.opened_without_grant`
- `remote_command.received`
- `remote_command.denied`
- `system.boot`
- `system.config_changed`
- `power.low`
- `tamper.detected`

OTA events may be added when the OTA path is implemented.

## Examples

### Wiegand PIN Granted

```json
{
  "event_id": "evt_00001234",
  "schema_version": 1,
  "event_type": "access.granted",
  "timestamp": "2026-05-14T10:44:00Z",
  "device_id": "opb-dev-001",
  "parcel_lock_id": "opb-lock-001",
  "organization_id": "org_local",
  "household_id": "hh_01",
  "actor_id": "usr_02",
  "actor_type": "household_user",
  "key_id": "key_usr_02_main",
  "credential_id": "cred_usr_02_pin_1",
  "credential_type": "wiegand_pin",
  "access_purpose": "pickup",
  "source": "local_reader",
  "result": "success",
  "reason_code": "credential_valid"
}
```

### Carrier PIN Granted

```json
{
  "event_id": "evt_00001235",
  "schema_version": 1,
  "event_type": "access.granted",
  "timestamp": "2026-05-14T10:51:00Z",
  "device_id": "opb-dev-001",
  "parcel_lock_id": "opb-lock-001",
  "organization_id": "org_local",
  "actor_id": "carrier_dhl",
  "actor_type": "carrier",
  "key_id": "key_carrier_dhl_delivery",
  "credential_id": "cred_carrier_dhl_pin_1",
  "credential_type": "wiegand_pin",
  "access_purpose": "delivery",
  "source": "local_reader",
  "result": "success",
  "reason_code": "credential_valid"
}
```

### Door Opened and Closed

```json
{
  "event_id": "evt_00001236",
  "schema_version": 1,
  "event_type": "door.opened",
  "timestamp": "2026-05-14T10:51:04Z",
  "device_id": "opb-dev-001",
  "parcel_lock_id": "opb-lock-001",
  "source": "door_sensor",
  "result": "observed",
  "related_event_id": "evt_00001235"
}
```

```json
{
  "event_id": "evt_00001237",
  "schema_version": 1,
  "event_type": "door.closed",
  "timestamp": "2026-05-14T10:51:22Z",
  "device_id": "opb-dev-001",
  "parcel_lock_id": "opb-lock-001",
  "source": "door_sensor",
  "result": "observed",
  "related_event_id": "evt_00001235"
}
```

### Remote Unlock Denied

```json
{
  "event_id": "evt_00001238",
  "schema_version": 1,
  "event_type": "remote_command.denied",
  "timestamp": "2026-05-14T11:02:00Z",
  "device_id": "opb-dev-001",
  "parcel_lock_id": "opb-lock-001",
  "actor_id": "int_ha_main",
  "actor_type": "integration",
  "integration_id": "ha-main",
  "command": "unlock.request",
  "scope": "remote_unlock",
  "request_id": "req-000123",
  "source": "mqtt",
  "result": "denied",
  "reason_code": "invalid_command_signature"
}
```

## Data Minimization

Never store in audit logs:

- raw PINs
- raw Wiegand codes
- raw QR secrets
- raw token payloads where they contain secrets
- fingerprint templates
- long-term integration secrets
- HMAC keys
- backup codes
- full tracking numbers unless explicitly required and justified
- recipient names unless explicitly required and justified

Prefer:

- stable pseudonymous local IDs
- truncated or hashed external references
- role and scope fields instead of names
- reason codes instead of raw validation details

Carrier IDs may be stored as coarse identifiers such as `carrier_dhl` or `carrier_unknown`. Parcel IDs should be optional and pseudonymized where possible.

## Offline Buffering and Export

The device must be able to buffer audit events locally when offline.

Draft behavior:

- events are appended to a local ring buffer or log store
- each event receives a monotonic sequence number
- if trusted time is unavailable, record boot id and uptime
- sync/export should preserve event order
- sync/export must not add raw secrets
- local log overflow should be reported as `audit.overflow` once such an event type exists

For M1, local storage may be limited. Retention and buffer size are draft and depend on the selected firmware platform and storage strategy.

## Retention Assumptions

Retention is draft.

Initial assumptions:

- keep recent local events long enough for owner troubleshooting and security review
- avoid indefinite retention by default
- allow local purge/reset by admin
- do not sync/export more data than needed for the target integration
- privacy-sensitive deployments such as shared buildings or civic parcel boxes need a separate retention policy

## MQTT/API Projection

MQTT and local API outputs should be projections of audit events, not raw internal dumps.

Allowed:

- event type
- result
- coarse actor type
- pseudonymous IDs
- door/lock status
- power/tamper status

Not allowed:

- raw PINs
- raw Wiegand codes
- raw NFC IDs where avoidable
- long-term secrets
- backup codes
- fingerprint templates

Remote unlock over MQTT/API is security-sensitive and must be logged. See [Remote command security](../security/remote-command-security.md).
