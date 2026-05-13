# Carrier Adapter Interface

Carrier integrations are optional plugins. OpenParcelBox must work without DHL, Hermes, DPD, GLS, UPS, Amazon or any other carrier partnership.

The carrier adapter interface maps external carrier workflows to internal OpenParcelBox opening-right lifecycle events.

## Goals

- Keep the core system carrier-neutral.
- Avoid hard-coding a single carrier workflow.
- Support future official integrations.
- Preserve local-first operation.
- Avoid requiring a remote "open door" command.
- Allow unsupported carriers to use local/manual fallback flows.

## Non-Goals

- Do not assume a universal public carrier API exists.
- Do not make any carrier integration mandatory.
- Do not give carriers pickup rights by default.
- Do not expose raw access secrets through logs or webhooks.

## Core Events

Carrier adapters may map external systems to these internal events:

- `parcel_pre_advised`
- `delivery_right_requested`
- `delivery_right_issued`
- `delivery_right_rendered`
- `delivery_attempt_started`
- `delivery_deposit_confirmed`
- `delivery_attempt_failed`
- `delivery_right_expired`
- `delivery_right_revoked`
- `return_pickup_requested`
- `return_pickup_completed`

## Adapter Boundary

An adapter may:

- receive carrier webhooks
- query carrier APIs
- map parcel references to local expected parcels
- request an `OpeningRight`
- receive delivery result events
- emit notification events

An adapter must not:

- bypass local access-control checks
- directly unlock hardware
- require cloud operation for local access
- store raw access secrets unnecessarily
- grant pickup rights unless explicitly configured

## Manual Fallback

Unsupported carriers can still work through:

- static local deposit code
- one-time deposit code
- time-windowed code
- QR code shown or printed by the recipient
- owner-approved delivery mode
- simple deposit-only mechanical flow, if hardware supports it

Fallback flows must be clearly labeled as less integrated than official carrier adapters.

## Example Mapping

An official or semi-official carrier integration might map:

```text
parcel pre-advice -> expected parcel
delivery time window -> OpeningRight validity window
carrier delivery role -> actor_type=carrier
deposit permission -> action=deposit
delivery confirmation -> audit event + notification
failed attempt -> denied/failed event
```

This is a generic model, not a compatibility claim for any specific carrier.

## Privacy Requirements

- Minimize parcel identifiers.
- Prefer pseudonymous local references.
- Do not log raw PINs or token secrets.
- Make retention configurable.
- Separate operational logs from user-facing history where useful.

## Research References

Useful public references to continue watching:

- CEN/TS 17457
- OPEN GIE
- GLS/NXT public API documentation
- Direct4.me integration descriptions
- provider-neutral station policy discussions

