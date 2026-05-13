# Milestone 1: DHL Parcel Box Retrofit Beta

Status: beta milestone target.

This milestone defines the first working beta target for OpenParcelBox: a fully local, provider-independent parcel-box system based on a retrofitted DHL parcel box.

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
