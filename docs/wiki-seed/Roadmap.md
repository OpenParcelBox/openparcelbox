# Roadmap

This page is a readable overview. The working roadmap remains in the specs repository and GitHub milestones.

## Source of truth

- M1 specification: https://github.com/OpenParcelBox/specs/blob/main/docs/roadmap/m1-dhl-retrofit-beta.md
- M1 milestone: https://github.com/OpenParcelBox/specs/milestone/9
- Open issues: https://github.com/OpenParcelBox/specs/issues
- Development hub draft: https://github.com/OpenParcelBox/specs/blob/main/docs/project/development-hub.md

## Current phase

OpenParcelBox is in early planning and prototype definition.

The project is currently defining the first realistic prototype: a local, ESP-based retrofit controller for existing DHL parcel boxes.

## M1: DHL Parcel Box Retrofit Beta

Goal: prove the OpenParcelBox idea on real hardware.

M1 should demonstrate:

- standalone local operation without mandatory cloud, bridge, Home Assistant or internet
- keypad and NFC access, with fingerprint prepared where feasible
- Wiegand access-module support
- local web management on the ESP
- local event logging
- a small multi-household permission model
- carrier/service-provider PINs separated from owner pickup/admin access
- MQTT and local API foundation
- OTA update path
- stable low-voltage power design
- module-first hardware exploration before a later reference board

More: [M1: DHL Parcel Box Retrofit Beta](M1-DHL-Parcel-Box-Retrofit-Beta)

## Near-term work areas

### Hardware

- document the available DHL parcel box and lock unit
- evaluate ready-made modules for controller, lock driver, Wiegand reader, NFC, fingerprint, sensors, power and solar support
- select the first lock strategy
- sketch cable routing and protected wiring zones
- measure lock impulse current and voltage drop

### Firmware

- decide the M1 firmware base: ESP-IDF, Arduino Core, ESPHome or hybrid
- define AP mode and local webserver requirements
- define local API and MQTT event surface
- implement Wiegand event handling
- define audit-log storage and wear strategy
- prepare OTA and secure update assumptions

### Access control

- refine organization / household / user / carrier model
- define base events: Wiegand credential, door opened, door closed, remote command, failed attempt
- separate delivery access from pickup/admin access
- prepare future dynamic, rolling or signed opening rights
- keep raw secrets out of logs

### Standards and responsibility

- keep CEN/TS 17457:2020 as an important European interoperability reference
- avoid copying protected standards text
- separate community baseline from manufacturer conformity claims
- document that manufacturers must prove legal, technical and normative conformity for their target markets

### Community and ecosystem

- keep documentation bilingual where useful: English for technical source, German for the first strong community base
- invite DIY builders, hardware people, coders, standards people and manufacturers
- keep commercial paths possible without allowing a new closed dead end

## Later roadmap themes

- OpenParcelLock reference modules
- OpenParcelBox Core/backplane
- bridge and self-hosted services
- optional cloud convenience service
- dynamic carrier integrations
- Matter / Thread / Zigbee / Home Assistant / HomeMatic paths
- neighborhood capacity sharing
- civic parcel boxes where parcel stations are missing
- manufacturer compatibility and "Proudly made for OpenParcelBox" certification rules

