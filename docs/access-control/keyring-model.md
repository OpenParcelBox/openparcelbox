# Keyring and Delegated Key Model

OpenParcelBox should behave more like a configurable digital key system than a keypad that simply compares numbers.

Credentials are not identities. A credential belongs to a local actor. The actor may be a user, household member, carrier, admin, neighbor, service role or integration. Audit events reference actor and credential IDs, never raw credential secrets.

## Core Concepts

| Concept | Meaning |
|---|---|
| `Organization` | Local owner context, housing association, operator or later civic/community deployment. |
| `Household` | Household, flat, recipient group or local user group. |
| `Actor` | Person, carrier, service role, admin, neighbor, system or integration. |
| `Keyring` | Collection of keys owned by an actor or organization. |
| `Key` | Local digital key with scopes and policy. |
| `Subkey` | Delegated or derived key with limited scope. |
| `Credential` | Concrete presentation form: PIN, Wiegand code, NFC, fingerprint, token, API signature. |
| `OpeningRight` | Permission to perform a specific action on a target. |
| `Scope` | Boundary such as delivery, pickup, admin, maintenance, box, compartment or integration. |

## Local Identity Hierarchy

M1 should already reserve a stable hierarchy:

```text
organization_id
  household_id
    actor_id / user_id
      keyring_id
        key_id
          credential_id
```

A small private installation may use a local organization such as `org_local`. Larger deployments can later map this to a building, housing association, neighborhood box or operator.

## Master Actor

The first setup should create a local master/admin actor.

The master actor may:

- create households
- create users
- assign credentials
- create carrier/service credentials
- create scoped integration keys
- revoke credentials and subkeys
- view local logs
- perform local recovery actions

Master credentials must not leave the device. They may authorize creation of scoped keys, but should not be shared with integrations.

## Credentials Belong to Keys

One user may have several credentials. They should all resolve to the same local actor through a key or keyring.

```json
{
  "actor_id": "usr_02",
  "actor_type": "household_user",
  "household_id": "hh_01",
  "keyring_id": "kr_usr_02",
  "credentials": [
    {
      "credential_id": "cred_usr_02_pin_1",
      "credential_type": "pin",
      "key_id": "key_usr_02_main"
    },
    {
      "credential_id": "cred_usr_02_nfc_1",
      "credential_type": "nfc",
      "key_id": "key_usr_02_main"
    }
  ]
}
```

Lookup flow:

```text
credential input
  -> normalized credential event
  -> credential lookup
  -> key lookup
  -> actor lookup
  -> policy check
  -> local decision
  -> audit event
```

## Subkeys

Subkeys are limited keys derived from or authorized by a parent key.

Examples:

- temporary pickup code
- neighbor pickup key
- carrier delivery key
- service maintenance key
- Home Assistant integration key
- Matter adapter key
- Bridge sync key
- emergency backup key

Subkeys may have:

- `parent_key_id`
- `scope`
- `valid_from`
- `valid_until`
- `max_uses`
- `used_count`
- `generation`
- `revocation_state`
- `offline_policy`

Subkeys should be revocable without deleting the master actor.

## Integration Keys

Integrations must not receive master credentials.

Instead, a master/admin actor authorizes creation of a scoped integration key.

Example:

```json
{
  "parcel_lock_id": "opb-lock-01",
  "integration_id": "ha-main",
  "key_id": "key_int_ha_main",
  "parent_key_id": "key_admin_master",
  "scopes": ["status.read", "event.read", "unlock.request"],
  "algorithm": "hmac-sha256",
  "valid_from": "2026-05-14T00:00:00Z",
  "expires_at": null
}
```

The integration stores only the scoped integration secret or public/private key material assigned to it. It then creates rotating command authorizations for concrete actions.

## Offline Revocation Pattern

Offline systems cannot magically revoke every credential everywhere instantly.

Hotel RFID systems often solve this with a combination of:

- short validity windows
- signed or MAC-protected credentials
- room/lock generation counters
- service cards or admin devices carrying updates
- occasional online synchronization
- local blacklists or revocation lists

OpenParcelBox should use the same family of ideas:

- keep static credentials local and revocable
- prefer short-lived remote command tokens
- bind tokens to `parcel_lock_id`, action, time and request id
- use key generations to invalidate older subkeys
- allow bridge/cloud synchronization to update revocation lists when available
- document that offline revocation is bounded by the last local sync or local generation update

## Event Model Impact

Audit events should reference:

- `actor_id`
- `actor_type`
- `key_id`
- `parent_key_id`, if relevant
- `credential_id`
- `credential_type`
- `scope`
- `opening_right`
- `decision_result`
- `decision_reason`

They must not log:

- raw PINs
- raw Wiegand codes
- raw NFC identifiers where avoidable
- fingerprint templates
- long-term integration secrets
- command signing secrets

## M1 Minimum

M1 does not need full delegated-key infrastructure, but it should not paint the data model into a corner.

M1 should reserve:

- stable local actor IDs
- stable credential IDs
- household assignment
- carrier/service actor types
- key or keyring IDs, even if simple
- audit events that reference IDs instead of raw secrets

This makes later rolling codes, integration keys, backup codes and delegated pickup rights possible without redesigning the entire model.
