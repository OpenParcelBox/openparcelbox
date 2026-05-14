# Opening Rights

OpenParcelBox models parcel-box access as rights management, not as keypad-only code handling.

The core abstraction is `OpeningRight`.

## Goals

- Support local-first operation.
- Separate delivery and pickup rights.
- Allow offline validation where possible.
- Support optional carrier integrations later.
- Avoid storing raw PINs, QR secrets or token secrets in logs.
- Keep Home Assistant, HomeMatic, cloud and carrier integrations optional.

## OpeningRight Model

Draft fields:

| Field | Purpose |
|---|---|
| `id` | Stable local identifier. |
| `issuer` | Actor or system that created the right. |
| `subject` | Human, carrier, service role, device or integration receiving the right. |
| `actor_type` | `owner`, `resident`, `neighbor`, `carrier`, `service`, `admin`, `system`. |
| `action` | `deposit`, `pickup`, `return_pickup`, `maintenance`, `reservation`, `admin_unlock`. |
| `scope` | Box, compartment, lock, time window or parcel-specific scope. |
| `target` | Receptacle, compartment or lock profile target. |
| `parcel_ref` | Optional parcel or tracking reference. Should be minimized or pseudonymized. |
| `valid_from` | Start of validity. |
| `valid_until` | End of validity. |
| `max_uses` | Number of allowed uses. |
| `used_count` | Number of consumed uses. |
| `revoked_at` | Revocation timestamp, if revoked. |
| `offline_policy` | Whether and how the right can be validated locally. |
| `presentation_methods` | Allowed encodings such as PIN, QR, barcode, signed token or API grant. |
| `audit_policy` | Minimum logging and retention rules. |

## Actions

Delivery and pickup are separate actions:

- `deposit`: allows placing a parcel into the box.
- `pickup`: allows removing a parcel from the box.
- `return_pickup`: allows a carrier to collect an outgoing parcel.
- `maintenance`: allows service actions without normal parcel access.
- `reservation`: reserves a compartment or time slot.
- `admin_unlock`: administrative unlock with stronger audit requirements.

Carrier delivery rights should be deposit-only by default.

## Lifecycle

```text
draft -> issued -> rendered -> active -> used
                         \-> expired
                         \-> revoked
                         \-> denied
```

Validation must define behavior for:

- not yet valid
- expired
- revoked
- already used
- malformed credential
- wrong action
- wrong target
- offline state unknown

## Presentation Formats

Opening rights can be presented in different formats:

- numeric PIN
- QR payload
- barcode-compatible payload
- signed token
- app deep link
- carrier API grant
- local admin approval

The presentation format is not the core permission. It is only how the permission is shown, transported or entered.

## PIN Requirements

PINs are easy for simple keypads but weak if treated casually.

Draft rules:

- No plaintext PIN storage.
- Rate limiting is mandatory.
- Failed-attempt lockout is mandatory.
- PIN length and entropy must be documented per profile.
- Static carrier PINs are allowed only as fallback, not the preferred long-term model.
- One-time or time-limited PINs are preferred.

## QR and Token Requirements

QR payload options:

- opaque local reference
- signed self-contained token
- URL or app deep link

Draft rules:

- Include expiry.
- Include intended action.
- Include target box or compartment.
- Support replay protection.
- Do not expose raw long-term secrets.
- Treat scanned payloads as untrusted input.

## Audit Events

Opening-right audit events should be privacy-preserving.

Initial event types:

- `right_issued`
- `right_rendered`
- `right_synced`
- `right_validated`
- `right_used`
- `right_denied`
- `right_expired`
- `right_revoked`
- `right_purged`

Minimum event fields:

- event id
- timestamp
- device id
- right id
- action
- target
- result
- reason code

Avoid logging:

- raw PINs
- QR secrets
- signed token secrets
- full tracking numbers unless explicitly required and justified

## Offline Behavior

The device must be able to validate locally available rights without cloud or bridge access.

When offline:

- local rights remain usable according to their offline policy
- denied attempts are logged locally
- events are buffered for later sync/export
- cloud-issued rights only work if already synced and locally valid

## Related Models

Opening rights should be implemented on top of a local keyring model rather than raw code matching.

See:

- [Privacy-preserving audit events](audit-events.md)
- [Keyring and delegated key model](keyring-model.md)
- [Remote command security](../security/remote-command-security.md)
