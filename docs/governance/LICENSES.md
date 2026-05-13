# OpenParcelBox Licensing

OpenParcelBox uses different licenses for different parts of the project. This is intentional.

The goal is to keep the core technology open and auditable, while allowing commercial hardware, cloud services, professional integrations, certification, consulting and enterprise deployments.

## License overview

| Component | Scope | License / Terms |
|---|---|---|
| Bridge firmware / Device Core | ESP32/controller firmware, lock logic, sensors, MQTT, local API, Home Assistant integration | GPL-3.0-or-later |
| Self-hosted server / OpenParcelBox Cloud Community Edition | Self-hosted backend for device management, users, plugins, events and integrations | AGPL-3.0-or-later |
| Plugin SDK / API client libraries / integration templates | SDKs, API clients, plugin templates, developer tooling | Apache-2.0 |
| Hardware designs | KiCad files, schematics, PCB layouts, reference boards, adapter boards, sensor boards | CERN-OHL-S-2.0 |
| Technical documentation and specifications | Protocol specifications, API documentation, security model, installation documentation, architecture | CC-BY-SA-4.0 |
| Website marketing texts and commercial presentation | Product pages, commercial descriptions, marketing copy | All rights reserved unless explicitly stated otherwise |
| Brand assets, logos, icons, certification marks and domain identity | OpenParcelBox, OpenParcelBox Cloud, OpenParcelBox Certified, OpenParcelBox Compatible, Powered by OpenParcelBox, logo/icon, openparcelbox.org | All rights reserved; use governed by trademark policy |
| Hosted Cloud / Managed Cloud / Enterprise services | Hosted services, push, backups, carrier integrations, trust registry, public locker directory, SLA, support, white label | Separate commercial terms |

## Separate licenses by directory

Some repositories or subdirectories may contain their own `LICENSE`, `COPYING`, `NOTICE` or equivalent files. If a subdirectory contains a more specific license file, that file governs the contents of that subdirectory.

When in doubt, check the license file closest to the file you want to use.

## Brand and trademark clarification

Open source licenses for source code, documentation or hardware designs do **not** grant permission to use the OpenParcelBox name, logo, certification marks, domain identity or official compatibility claims.

The OpenParcelBox name, logo and related marks are reserved project identifiers and trademarks of the OpenParcelBox project. Their use is governed by `TRADEMARK.md` and `BRAND_USAGE.md`.

## Commercial use

Commercial use of open components is allowed under the applicable license terms. However, official use of the OpenParcelBox name, logo, certification marks, hosted service names or partnership claims requires separate written permission.

## Early project stage

OpenParcelBox is in an early planning and specification phase. No production hardware, firmware or security guarantees are provided at this stage.
