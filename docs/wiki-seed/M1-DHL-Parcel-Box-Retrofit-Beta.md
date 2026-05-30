# M1: DHL Parcel Box Retrofit Beta

Status: early beta target definition.

This milestone is the first concrete OpenParcelBox prototype direction: a retrofit kit for existing DHL parcel boxes.

## Why this is first

The discontinued electronic DHL parcel box is a strong first use case because:

- many boxes still physically exist
- the original closed electronic access path reached end of life
- the original DHL-only access model was one of the core limitations
- owners need a practical way to keep using the box
- the retrofit problem is concrete, testable and useful
- it demonstrates the OpenParcelBox principle: keep the box, replace the closed control layer

The useful lesson is not that a permanent shared combination code is a good platform. It is not.

The useful lesson is that a carrier does not need ownership of the whole parcel box. A carrier needs a limited right to open a receptacle for a concrete delivery.

## Official DHL retrofit reference

DHL published a public retrofit video:

https://www.youtube.com/watch?v=jXzvwI64qbM

Public metadata:

- title: `Umbau des DHL Paketkasten zu Ablageort`
- channel: `Deutsche Post und DHL`
- published: 2025-03-13

The video shows the practical baseline: keep the physical box, disable or bypass the old electronic path, mount a mechanical combination lock and store the access information as an alternative drop-off instruction.

OpenParcelBox starts from that insight, but aims to replace the permanent workaround code with local, open, auditable and carrier-neutral access control.

## M1 goal

Demonstrate a fully locally operated, provider-independent parcel-box system based on a converted DHL parcel box.

The beta should work as a realistic standalone retrofit, not as a final universal platform.

## Target behavior

- ESP-based controller
- standalone operation without mandatory cloud, bridge, Home Assistant or internet
- local AP/webserver setup path
- keypad/PIN access
- NFC access
- fingerprint prepared where feasible
- Wiegand access-module support
- door opener / lock driver control
- door open/closed feedback
- local logging
- MQTT and local API foundation
- OTA update path
- low-voltage 12 V supply design
- battery and solar treated as separate subsystems

## User and permission target

M1 should support a small multi-household model:

- up to 4 households
- up to 2 people per household
- PIN or NFC per person
- optional/prepared fingerprint identity
- up to 6 carrier/service-provider PINs
- master/admin code
- local event log with stable credential IDs, not raw secrets

Access types must be separated:

- owner/admin access
- user pickup access
- carrier delivery access
- remote command access
- maintenance/emergency path

## Current open decisions

- firmware base: ESP-IDF, Arduino Core, ESPHome or hybrid
- ESP target board: ESP32-C6, ESP32-S3, ESP32-C3 or classic ESP32
- lock strategy: reuse, gut, replace or redesign the old DHL lock path
- lock driver: MOSFET, relay, motor driver, solenoid or cabinet lock module
- Wiegand input protection and wiring
- local webserver security model
- audit-log storage and flash-wear strategy
- OTA signing and rollback assumptions
- battery/solar feasibility after current measurements

## Immediate next steps

1. Document available parts with photos, names, voltages and interfaces.
2. Photograph the DHL parcel box, door, lock area, inside space and cable paths.
3. Inspect the old lock unit and decide whether it is reusable.
4. Identify keypad/RFID/fingerprint module interfaces.
5. Sketch two or three lock mechanism options.
6. Estimate power consumption for controller, reader, lock actuation and sensors.
7. Decide the firmware base for the first prototype.
8. Convert the chosen direction into hardware and firmware issues.

## Source documents

- Full M1 spec: https://github.com/OpenParcelBox/specs/blob/main/docs/roadmap/m1-dhl-retrofit-beta.md
- Hardware module matrix: https://github.com/OpenParcelBox/specs/blob/main/docs/hardware/m1-module-matrix.md
- Wiegand ACCESS-Port profile: https://github.com/OpenParcelBox/specs/blob/main/docs/hardware/wiegand-access-port.md
- Opening rights: https://github.com/OpenParcelBox/specs/blob/main/docs/access-control/opening-rights.md
- Remote command security: https://github.com/OpenParcelBox/specs/blob/main/docs/security/remote-command-security.md
- M1 milestone: https://github.com/OpenParcelBox/specs/milestone/9

