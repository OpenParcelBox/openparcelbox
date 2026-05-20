# Milestone 1 / Version 1 Beta: DHL Parcel Box Retrofit

Status: beta milestone target.

This document is the single roadmap source for the first concrete OpenParcelBox prototype: a fully local, provider-independent parcel-box system based on a retrofitted DHL parcel box.

The goal is not yet a universal OpenParcelBox platform. The goal is a realistic first standalone retrofit that proves the core ideas on real hardware.

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

M1 Beta should build on the same physical insight while adding a local, open and auditable access-control layer.

## Milestone Goal

Demonstrate a fully locally operated, provider-independent parcel-box system based on a converted DHL parcel box.

The focus is stable operation in a small multi-household scenario with distinguishable carrier access.

## Hardware Baseline

### Controller

- ESP-based microcontroller as central controller.
- Fully standalone operation without cloud.

### Input Devices

- Touch keypad through Wiegand.
- NFC card reader.
- Fingerprint sensor optional in V1, but prepared in hardware and firmware architecture.

### Actuation

- Door opener / lock control through relay, MOSFET, driver or comparable actuator path.
- Door state feedback for open/closed state.

### Power

- 12 V supply system.
- Internal voltage conversion for ESP and peripherals.
- Low-voltage only for the prototype.
- Battery and solar are treated as separate subsystems.
- Solar-supported operation remains a power-design goal, but beta acceptance starts with a stable low-voltage supply design.

The first prototype should start with ready-made modules where practical. The module research should inform a later OpenParcelBox Core/backplane rather than pretending that the first prototype is already a production PCB.

See: [M1 ready-made hardware module matrix](../hardware/m1-module-matrix.md).

## User and Permission Model

### Household Structure

- Support 4 households.
- Up to 2 people per household.

### Authentication Per Person

Each person may authenticate through:

- PIN code, or
- NFC token, or
- fingerprint where supported/prepared.

### Carrier Access

- Up to 6 dedicated PINs for parcel service providers.
- No fixed assignment to a specific household required in V1.
- Goal: identify in the local log who opened the box and when.

### Access Semantics

M1 must separate the meaning of access codes and commands.

At minimum:

- owner/admin access: may open for pickup, setup or maintenance
- user access: may open according to local permissions
- carrier access: should be deposit-oriented and should not automatically imply pickup rights
- master code: local emergency/admin path
- remote command access: security-sensitive action for MQTT/API, must be logged

This should align with the existing OpenParcelBox opening-right model, even if the first firmware stores everything locally.

## Core Functions

### Access System

The box can be opened through:

- PIN
- NFC
- optional fingerprint

Inputs are processed through the Wiegand access path where applicable.

### Logging

All openings are logged locally.

Log entries should include:

- timestamp
- credential identifier or stable internal ID
- access type: PIN / NFC / fingerprint / admin / remote command
- assignment: household, person or carrier
- door state result where available

Raw secrets should not be stored in the log.

### Multi-Household Operation

The system must clearly separate households.

The beta should make it traceable which household, person or carrier access was used for an opening event.

## Local Management

### ESP Web Interface

The ESP should provide local configuration without cloud.

The local web interface should manage:

- households
- people
- PINs
- NFC tokens
- prepared fingerprint identities
- carrier PINs
- identity-to-household assignment

## Interfaces and Integration

### MQTT

MQTT should publish relevant events:

- opening event
- credential/input event
- door status
- controller status where useful

MQTT may receive control commands such as:

- open door
- request status

Remote open commands must be treated as security-sensitive actions and should be logged.

### Local API

A local API foundation should exist.

Goal:

- external status readout
- future external configuration
- future bridge integration

The API does not need to be complete in beta, but it must not be blocked by the internal architecture.

### Smart Home

Smart-home integration may happen indirectly through MQTT or local API.

There must be no vendor lock-in.

## Updates

### OTA

Over-the-air updates should be supported.

Goal:

- maintenance without physical controller access
- local or trusted-network update path

OTA design must consider rollback and failure safety later, but beta may start with a simple documented approach.

## Firmware Platform Decision

The ESP firmware base is an explicit M1 architecture decision.

The project must compare at least:

- Arduino Core for ESP32
- ESP-IDF
- ESPHome
- a mostly custom firmware stack
- hybrid approaches, such as ESP-IDF with selected Arduino libraries or an ESPHome-inspired configuration layer

Decision criteria:

- reliable Wiegand input handling
- NFC and optional fingerprint module support
- local web interface feasibility
- local storage for users, credentials and logs
- MQTT support
- OTA support
- offline-first behavior
- security and update model
- long-term maintainability for open-source contributors
- ability to support future OpenParcelBridge integration
- avoiding unnecessary dependency on Home Assistant or any vendor ecosystem

M1 Beta does not need to settle the forever architecture for all OpenParcelBox devices, but it must choose a documented firmware base for the first prototype.

## Retrofit Design Questions

### Hardware Inventory

The next practical work starts when the available parts are documented:

- existing DHL parcel box and lock unit
- old lock mechanism and whether it is removed, reused or gutted
- keypad/RFID/NFC module
- possible fingerprint module
- ESP target board
- relay/MOSFET/driver options
- battery and solar parts
- door state sensors
- cable glands, wiring paths and mounting options

### Lock Mechanism

The lock is the most important mechanical unknown.

Open design questions:

- remove the original DHL lock unit completely?
- gut the old lock housing and reuse its mechanical position?
- keep part of the original latch/bolt mechanism?
- use a solenoid lock, motor lock, cabinet lock or custom OpenParcelLock module?
- define a fail-safe or fail-secure behavior?
- provide a local mechanical emergency opening path?
- protect wiring and lock actuation from outside manipulation?

M1 should prefer a simple, inspectable and serviceable lock mechanism over a clever but fragile design.

### Access Module Placement

The external access module needs its own small design study.

Questions:

- where can a keypad/RFID/NFC reader be mounted without weakening the door?
- can the existing lock opening or front cut-out be reused?
- does the old lock area need a cover plate or adapter plate?
- how is the cable routed through the moving door without fatigue?
- can a fingerprint module be used outdoors reliably?
- is Wiegand the preferred first interface for keypad/RFID/fingerprint readers?
- should the access module be replaceable without redesigning the whole door?

### Wiring and Protected Areas

M1 should assume that security-critical components live inside the parcel box or another protected area.

Preferred boundary:

- outside: keypad/RFID/NFC/fingerprint reader and possibly status LED/buzzer
- inside: ESP controller, lock driver, battery, charging, sensor wiring and code store
- protected connection: reader cable, door cable routing and tamper-aware wiring where practical

The reader should present credentials. The internal controller decides whether to open.

### Power Questions

Open power questions:

- ESP deep sleep or always-on controller?
- keypad wake input or always-powered reader?
- battery chemistry and protection board?
- charge controller module?
- winter reserve target?
- solar sizing based on standby current and opening frequency?

## Architecture Principles

- 100% locally runnable.
- No cloud requirement.
- Provider-independent.
- Modular and extensible.
- Prepared for future OpenParcelBridge integration.
- Security-sensitive decisions stay in the local controller unless explicitly delegated later.

## Out of Scope for Beta

The beta does not include:

- complex PIN hierarchies, such as combined household/person PINs
- complete ecosystem integration
- advanced automation logic
- cloud services
- carrier APIs
- parcel tracking
- formal compliance or certification claims
- final certified hardware
- universal support for all parcel boxes

These belong to later milestones.

## Beta Acceptance Criteria

The beta milestone is reached when:

- the converted DHL parcel box can be opened locally through at least PIN and NFC
- Wiegand input is parsed reliably for the selected input hardware
- the lock or door opener is controlled by the ESP
- open/closed state is detected locally
- 4 households with up to 2 people each can be represented
- up to 6 carrier PINs can be represented
- access events are logged locally with timestamp and identity mapping
- local web management can create and edit households, people, PINs and NFC tokens
- MQTT publishes opening and status events
- a local API foundation exists
- OTA update path is documented and tested at least once
- no cloud, Home Assistant or bridge is required for basic operation

## Immediate Next Steps

1. Document the available components as they arrive.
2. Photograph the DHL parcel box, door, lock area, inside space and cable paths.
3. Open or inspect the old lock unit and decide whether it is reusable.
4. Identify keypad/RFID/NFC/fingerprint module interfaces, especially Wiegand, relay, UART or standalone behavior.
5. Sketch two or three lock mechanism options before selecting one.
6. Estimate power consumption for controller, reader, lock actuation and sensors.
7. Compare Arduino Core, ESP-IDF, ESPHome and custom firmware options.
8. Convert the chosen direction into firmware and hardware tasks.
