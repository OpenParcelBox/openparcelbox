# Wiegand ACCESS-Port profile

Status: early architecture note.

OpenParcelBox should treat Wiegand as an important ACCESS-Port reference profile for retrofit-friendly access hardware.

## Why Wiegand matters

Wiegand is widely used in access-control systems and is supported by a very large number of practical modules:

- PIN keypads
- RFID and NFC readers
- fingerprint readers with controller output
- standalone access-control boards
- smart-home bridge modules and relay controllers

For OpenParcelBox, this makes Wiegand useful as a compatibility-first interface. It allows many existing access modules to be tested without designing a custom reader ecosystem first.

## Scope

The Wiegand profile should describe how an external access module presents a credential to the OpenParcelBox device.

The reader or keypad provides an input event. The OpenParcelBox firmware decides whether that event maps to a valid opening right.

The Wiegand profile is not the lock-control interface itself. Lock actuation remains a separate internal output path, for example through a LOCK-Port, relay output or lock-driver profile.

## Physical assumption

Typical parcel-box retrofits place the lock, controller and critical wiring inside the receptacle or inside another protected area.

The project should document Wiegand with this physical assumption:

- ACCESS-Port wiring should be routed inside the box or another protected area where possible.
- The reader may be outside, but the lock actuator and access decision should remain internal.
- The firmware should not treat a Wiegand reader as the final authority; it is a credential source.
- Tamper/contact inputs can be used where the mechanical setup supports them.

## Firmware model

Firmware should normalize Wiegand input into a protocol-independent event such as `AccessCredentialEvent`.

Suggested fields:

- `source`: `wiegand`
- `reader_id`
- `format`: for example `wiegand-26`, `wiegand-34`, `custom`
- `facility_code`, if present
- `credential_id`, if present
- `pin`, if the configured keypad mode emits PINs
- `raw_bits`, optional and only where useful for diagnostics
- `received_at`

The access-control layer should then map the event to an opening right, for example personal PIN, parcel-service PIN, one-time code, neighbor right or admin/service right.

## Configuration topics

The profile should later specify:

- supported bit lengths, starting with 26-bit and 34-bit
- keypad modes and PIN termination behavior
- parity validation behavior
- timing tolerances for D0/D1 pulses
- pull-up assumptions
- voltage-level assumptions
- maximum practical cable lengths for reference builds
- reader power output expectations
- multi-reader behavior, if needed

## Hardware topics

The ACCESS-Port concept should consider:

- Wiegand D0/D1 input
- GND reference
- reader supply, likely 12 V or 5 V depending on profile
- optional reader LED/buzzer control lines
- optional tamper/contact input
- protected input handling on the OpenParcelBox side

The project should keep the port generic enough that Wiegand is a profile, not the only possible access interface.

## Related alternatives

Future profiles may include:

- RS485/OSDP readers
- UART reader modules
- I2C modules for internal accessories
- NFC modules with local secure handling
- BLE or app-based signed tokens

These alternatives can be added without making them prerequisites for the first retrofit prototypes.

## Open questions

- Which Wiegand formats should be first-class in firmware?
- Should raw bit capture be exposed in diagnostics?
- How should keypad PIN mode be normalized across common readers?
- Should the Core Backplane provide selectable reader voltage?
- Which reader modules should be documented as early test candidates?
