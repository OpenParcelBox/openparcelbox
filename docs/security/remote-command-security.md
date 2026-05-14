# Remote Command Security

Remote commands are security-sensitive. They must not directly drive a lock output.

Any remote open/close command must be transported over an authenticated secure channel and must be evaluated by the local access-control policy before lock actuation.

No external integration may directly control the relay, motor driver or lock output.

## Security Boundary

Local reader wiring and network commands are different trust zones.

Some local readers, especially Wiegand keypads and RFID readers, may present raw or easily replayable credentials to the internal controller. This is a hardware reality. The mitigation is physical and architectural:

- the reader is only a credential source
- the reader must not be final authority
- the controller and lock driver must be inside the protected area
- lock actuation remains a separate internal output path

Network communication has a higher expectation:

- no raw credentials in MQTT, API, bridge, cloud or smart-home payloads
- no raw long-term secrets in command payloads
- no unauthenticated remote unlock
- no plain MQTT unlock command as a recommended safe mode

## Command Authorization

Transport security alone is not enough for high-risk actions such as `unlock.request`.

Remote commands should require:

- authenticated transport
- scoped integration identity
- rotating command authorization value
- replay protection
- local policy validation
- audit logging

The command authorization value should be bound to:

- `parcel_lock_id`
- `integration_id`
- command/action
- scope
- timestamp or time window
- request id or nonce

## HMAC-Style Command Signing

A practical early model is HMAC-based command signing.

The integration receives a scoped integration key during pairing. For each sensitive command it calculates:

```text
signature = HMAC(secret, parcel_lock_id || integration_id || command || scope || timestamp || request_id)
```

The device checks:

- known `integration_id`
- allowed scope
- valid timestamp window
- unused request id or nonce
- valid signature
- local policy allows the action

The secret itself is never sent.

Example command payload:

```json
{
  "parcel_lock_id": "opb-lock-01",
  "integration_id": "ha-main",
  "command": "unlock.request",
  "scope": "remote_unlock",
  "timestamp": 1778751000,
  "request_id": "req-000123",
  "signature": "hmac-sha256-value"
}
```

## MQTT Profile

MQTT may be useful for local smart-home integration, but a plain MQTT boolean is not an acceptable secure unlock path.

Unsafe pattern:

```text
openparcelbox/lock/set = OPEN
```

Required direction for secure operation:

- MQTT over TLS where possible
- broker authentication
- topic ACLs
- no raw credentials in payloads
- signed or HMAC-protected sensitive commands
- remote unlock disabled by default
- explicit local policy enabling remote unlock
- every remote command attempt logged

M1 may allow an unsafe lab/debug profile if it is clearly marked as unsafe and not recommended for normal operation.

## Matter and Smart Home

Matter may provide authenticated encrypted transport in a future integration path.

Matter must still be treated as a request channel, not as authority to directly actuate the lock. For high-risk actions, OpenParcelBox may require an additional OpenParcelBox command authorization value on top of the Matter transport.

Key principle:

> Matter may provide the secure transport, but OpenParcelBox remains the local authorization authority.

## Backup Codes and Recovery

Backup codes are local recovery or emergency credentials. They are not long-term integration secrets.

They should be:

- generated locally
- displayed only during setup or regeneration
- stored only as hashes or verifier values
- single-use where possible
- revocable
- logged on use

## M1 Minimum

M1 should reserve the model even if it does not implement full command signing yet.

M1 should:

- keep remote unlock disabled by default
- represent `remote_command` as an access source in the event model
- never expose raw PINs, Wiegand codes or NFC secrets over MQTT/API
- document that production remote unlock requires authenticated transport plus rotating command authorization
- keep the local controller as the final authority before lock actuation
