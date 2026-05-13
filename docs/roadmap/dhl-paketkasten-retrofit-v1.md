# DHL Paketkasten Retrofit V1 Roadmap

Status: early target definition.

This document defines the first concrete OpenParcelBox prototype direction: a retrofit kit for existing DHL parcel boxes.

The goal is not yet a universal OpenParcelBox platform. The goal is a realistic first standalone retrofit that proves the core ideas on real hardware.

Concrete beta acceptance target:

- [Milestone 1: DHL Parcel Box Retrofit Beta](m1-dhl-retrofit-beta.md)

## Why this is V1

The discontinued electronic DHL parcel box is the strongest first use case because:

- many boxes still physically exist
- the original closed electronic access path has reached end of life
- the original carrier-specific access model was one of the key limitations
- owners need a practical way to keep using the box
- the retrofit problem is concrete, testable and useful
- it demonstrates the OpenParcelBox principle: keep the box, replace the closed control layer

The useful lesson is not that a permanent shared combination code is a good platform. It is not.

The useful lesson is that a carrier does not need ownership of the whole parcel box. A carrier needs a limited right to open a receptacle for a specific delivery. OpenParcelBox should turn that workaround insight into local, auditable and carrier-neutral access control.

## Official DHL Retrofit Reference

DHL published a practical retrofit video for turning the older electronic DHL parcel box into a manual drop-off location:

https://www.youtube.com/watch?v=jXzvwI64qbM

Public video metadata:

- title: `Umbau des DHL Paketkasten zu Ablageort`
- channel: `Deutsche Post und DHL`
- published: 2025-03-13

The video demonstrates, in project-relevant terms:

- opening the box with the original electronic mechanism before removing the batteries
- removing the batteries so the original electronic latch remains permanently open
- mounting a mechanical combination lock to the parcel compartment
- using inside-secured screws / security screws for the new lock hardware
- entering the new code in the DHL customer account as part of the alternative drop-off location instructions
- adding a separate manual lock to the letter compartment where applicable

OpenParcelBox should treat this as a useful baseline and contrast point:

- DHL's approach is intentionally simple and manual.
- It keeps the physical box in use.
- It makes access understandable for delivery staff.
- It accidentally removes the original DHL-only access barrier.
- It does not solve dynamic codes, auditability, multi-user rights, local event logging, tamper handling or open electronic interoperability.

V1 should build on the same physical insight while adding a local, open and auditable access-control layer.

## V1 Goal

Build a standalone ESP-based retrofit controller for an existing DHL parcel box.

The first prototype should:

- work locally without cloud, bridge, Home Assistant or internet
- control a practical lock mechanism inside the parcel box
- support touch keypad, NFC and prepared fingerprint access where feasible
- process access hardware through Wiegand where applicable
- run from a 12 V low-voltage supply system with internal conversion for ESP and peripherals
- represent a small multi-household user model
- support carrier PINs separately from owner pickup/admin access
- provide local logging, local web management, MQTT events, a local API foundation and OTA updates for beta
- keep the design open enough that other users can reproduce, modify and improve it

## Target User Model

V1 should support a deliberately limited but useful standalone model:

- 4 households
- up to 2 people per household
- each person can authenticate through PIN or NFC
- fingerprint is optional in beta but should be prepared in the model
- 1 master/admin code
- up to 6 carrier/service-provider PINs
- household/group capability should be included in the data model from the beginning

The first firmware does not need full cloud account management, but it should avoid painting itself into a corner.

Example future groups:

- household
- owner/admin
- family member
- neighbor
- carrier
- service/maintenance

## Access Semantics

V1 must separate the meaning of access codes.

At minimum:

- owner/admin access: may open for pickup, setup or maintenance
- user access: may open according to local permissions
- carrier access: should be deposit-oriented and should not automatically imply pickup rights
- master code: local emergency/admin path
- remote command access: security-sensitive action for MQTT/API, must be logged

This should align with the existing OpenParcelBox opening-right model, even if the first firmware stores everything locally.

## Hardware Direction

The first prototype will be based on components available during the initial hardware exploration.

The next practical work starts when the available parts are documented:

- existing DHL parcel box and lock unit
- old lock mechanism and whether it is removed, reused or gutted
- keypad/RFID module
- possible fingerprint module
- ESP target board
- relay/MOSFET/driver options
- battery and solar parts
- door state sensors
- cable glands, wiring paths and mounting options

## Lock Mechanism Exploration

The lock is the most important mechanical unknown.

Open design questions:

- remove the original DHL lock unit completely?
- gut the old lock housing and reuse its mechanical position?
- keep part of the original latch/bolt mechanism?
- use a solenoid lock, motor lock, cabinet lock or custom OpenParcelLock module?
- define a fail-safe or fail-secure behavior?
- provide a local mechanical emergency opening path?
- protect wiring and lock actuation from outside manipulation?

V1 should prefer a simple, inspectable and serviceable lock mechanism over a clever but fragile design.

## Access Module Placement

The external access module needs its own small design study.

Questions:

- where can a keypad/RFID reader be mounted without weakening the door?
- can the existing lock opening or front cut-out be reused?
- does the old lock area need a cover plate or adapter plate?
- how is the cable routed through the moving door without fatigue?
- can a fingerprint module be used outdoors reliably?
- is Wiegand the preferred first interface for keypad/RFID/fingerprint readers?
- should the access module be replaceable without redesigning the whole door?

## Wiring and Protected Areas

V1 should assume that security-critical components live inside the parcel box or another protected area.

Preferred boundary:

- outside: keypad/RFID/fingerprint reader and possibly status LED/buzzer
- inside: ESP controller, lock driver, battery, charging, sensor wiring and code store
- protected connection: reader cable, door cable routing and tamper-aware wiring where practical

The reader should present credentials. The internal controller decides whether to open.

## Power Direction

V1 should explore solar-supported standalone operation.

The beta baseline uses a 12 V supply system with internal voltage conversion. Solar-supported operation remains a power-design goal, but the beta acceptance target is a stable low-voltage supply design.

Planning assumptions:

- low-voltage only
- no mains voltage on prototype boards
- certified external power supplies only if mains is used during tests
- battery-backed operation
- power budget for ESP sleep/wake behavior
- lock actuation peak current must be measured
- reader/keypad idle current must be measured
- solar sizing depends on standby current and opening frequency

Open questions:

- ESP deep sleep or always-on controller?
- keypad wake input or always-powered reader?
- battery chemistry and protection board?
- charge controller module?
- winter reserve target?

## Connectivity Direction

V1 is standalone first, but the beta includes MQTT, local API foundation and OTA as local/network features.

Matter, Zigbee, Home Assistant or bridge integration may be explored later, but they are not required for V1 to work.

The firmware architecture should still avoid blocking these future paths.

Possible staged approach:

1. Standalone local keypad/RFID operation.
2. Local configuration interface.
3. Local web management on the ESP.
4. MQTT events and local API foundation.
5. OTA update path.
6. Optional Home Assistant, Matter, Zigbee or bridge path.

## V1 Non-Goals

V1 does not need:

- hosted cloud
- carrier API integration
- package tracking
- dynamic carrier-issued codes
- multi-building management
- official compliance claims
- final certified hardware
- universal support for all parcel boxes

These remain platform goals, not first-retrofit requirements.

## Acceptance Criteria for V1 Concept

The V1 concept is ready for prototype implementation when:

- component inventory is documented with photos, names, voltages and interfaces
- lock strategy is selected: reuse, gut, replace or redesign
- access module mounting concept is sketched
- cable routing concept is sketched
- power budget is estimated
- local user/code model is written down
- local web management scope is written down
- MQTT event and local API scope is written down
- OTA update path is written down
- firmware state model is sketched
- safety and emergency opening assumptions are documented
- first bill of materials is drafted
- obvious weather and tamper risks are listed

## Immediate Next Steps

1. Document the available components as they arrive.
2. Photograph the DHL parcel box, door, lock area, inside space and cable paths.
3. Open or inspect the old lock unit and decide whether it is reusable.
4. Identify keypad/RFID/fingerprint module interfaces, especially Wiegand, relay, UART or standalone behavior.
5. Sketch two or three lock mechanism options before selecting one.
6. Estimate power consumption for controller, reader, lock actuation and sensors.
7. Convert the chosen direction into firmware and hardware issues.
