# OpenParcelBox

OpenParcelBox is an open ecosystem for smart parcel delivery infrastructure, retrofit parcel boxes, open locks and self-hosted access control.

The project is in the early planning and specification phase. No production hardware, firmware or security guarantees exist yet.

## Goal

OpenParcelBox explores how existing parcel boxes, cabinets, lockers, garage boxes and other containers can be upgraded into smart parcel drop-off stations without locking users into one carrier, one cloud or one proprietary controller.

The ecosystem is intended to support:

- local-first device operation
- optional self-hosted bridge
- optional hosted cloud service
- optional integrations such as MQTT, Home Assistant, HomeMatic, Matter, webhooks and LoRa/LoRaWAN
- open lock modules and lock profiles
- retrofit paths for existing parcel boxes
- dynamic carrier access concepts inspired by CEN/TS 17457:2020

## Principles

- The device must keep its basic local function without internet, cloud or Home Assistant.
- Cloud services are optional convenience services, not platform requirements.
- Home Assistant and HomeMatic are optional integrations, not prerequisites.
- Carrier integrations are optional plugins, not platform dependencies.
- Delivery access and pickup access must be separate rights.
- Hardware should use documented ports, profiles and adapters instead of hidden proprietary assumptions.
- Prototype hardware must not include mains voltage on the board. Use certified external power supplies.

## Ecosystem

OpenParcelBox is not a single finished product. It is planned as a set of open building blocks:

- **OpenParcelBox Core**: hardware/backplane concept for power, ports, sensors, locks and expansion
- **OpenParcelBox Device**: local controller firmware and standalone behavior
- **OpenParcelLock**: open lock module and lock interface profile
- **OpenParcelBox Bridge**: optional self-hosted server for users, events, notifications and integrations
- **OpenParcelBox Cloud**: optional hosted bridge for people who do not want to self-host
- **OpenParcelBox Retrofit**: migration paths for existing parcel boxes and cabinets
- **OpenParcelBox Profiles**: documented profiles for locks, readers, sensors and controllers
- **OpenParcelBox Plugins**: integrations for tracking, carriers, smart home systems and notifications

## Standards

OpenParcelBox should use **CEN/TS 17457:2020** as an important reference for interoperable digital opening and closing systems for parcel receptacles.

The standard is not copied into this repository. Project work should refer to it legally and summarize requirements in original project language.

See:

- [Standards baseline](docs/standards/baseline.md)
- [Opening rights](docs/access-control/opening-rights.md)
- [Carrier adapter interface](docs/integrations/carrier-adapter.md)

## Current Focus

Early project work focuses on:

- project vision and terminology
- repository and GitHub Project structure
- CEN/TS 17457:2020 research
- access-control model and opening rights
- local-first architecture
- OpenParcelLock concept
- retrofit scenarios for existing parcel boxes
- security and privacy requirements
- carrier access and dynamic code concepts

## Repositories

- [openparcelbox](https://github.com/OpenParcelBox/openparcelbox): ecosystem, planning, documentation and roadmap
- [openparcellock](https://github.com/OpenParcelBox/openparcellock): open lock module and interface profile
- [.github](https://github.com/OpenParcelBox/.github): organization profile and community health files

## Domain

The canonical project domain is:

https://openparcelbox.org

The website is planned to be GitHub-based, using GitHub Pages.

## Project Status

This repository currently contains planning material. It is not yet an implementation repository for production-ready hardware or firmware.

Use issues and discussions for ideas, research, architecture decisions and early specification work.
