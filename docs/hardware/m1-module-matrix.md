# M1 Ready-Made Hardware Module Matrix

Status: planning note for M1 DHL retrofit research.

This document lists ready-made module classes that can be used to prototype the first OpenParcelBox DHL parcel box retrofit before designing a custom PCB.

It is not a bill of materials, product endorsement, schematic, compliance statement or production design.

## Design Constraints

M1 should start with available, inspectable modules and use the lessons learned to shape a later OpenParcelBox Core/backplane.

Core constraints:

- low-voltage only
- no mains voltage on prototype boards
- certified external power supplies only if mains is used during tests
- 12 V system direction for locks, readers and distribution
- 5 V and 3.3 V derived locally for ESP and peripherals
- battery subsystem treated separately from the main logic board
- solar subsystem treated separately from the battery subsystem
- controller, credential store and lock driver stay inside the protected area
- external access readers present credentials only
- the internal controller decides whether to open

## Tags

| Tag | Meaning |
| --- | --- |
| `PROT` | Useful for bench or field prototype testing, not a production recommendation. |
| `REF` | Useful reference pattern for a later OpenParcelBox Core/backplane design. |
| `RISK` | Known safety, reliability, outdoor, documentation or lifecycle concern. |

## Candidate Module Matrix

### 12 V Input and Distribution

| Candidate class | Notes | Tag |
| --- | --- | --- |
| Automotive ATO/ATC fuse block | Useful for a fused 12 V distribution tree with separate branches for lock, reader, DC/DC and radio. | `REF` |
| DIN rail or pluggable terminal distribution | Useful for readable prototype wiring and star-ground discipline in a protected enclosure. | `REF` |

Requirements:

- fuse each outgoing branch
- label all branches
- do not use an unfused bus bar for lock and electronics loads

### Battery Subsystem

| Candidate class | Notes | Tag |
| --- | --- | --- |
| Complete 4S LiFePO4 12 V battery pack with integrated BMS | Safer prototype direction than loose cells or custom cell welding. Use vendor charge and discharge limits. | `REF` |
| Separate BMS board for 3S/4S Li-ion or 4S LiFePO4 | Only for bench evaluation until the specific board, balancing behavior and protection path are verified. | `PROT`, `RISK` |

Requirements:

- battery gets its own fuse
- battery connector must be keyed or clearly protected against polarity mistakes
- battery and solar must not be merged through an undocumented "charger module"
- battery chemistry must be documented before field testing

### Solar Subsystem

| Candidate class | Notes | Tag |
| --- | --- | --- |
| Documented MPPT solar charge controller for 12 V battery systems | Good reference for measuring real winter reserve and daily energy budget. | `REF` |
| Small low-cost PWM or "mini MPPT" solar controller | Useful only for comparison testing; many modules have vague claims and poor documentation. | `PROT`, `RISK` |

Requirements:

- treat `PV panel -> solar controller -> battery` as a separate documented chain
- do not power the ESP directly from a raw panel
- measure standby current before claiming solar suitability
- document winter reserve assumptions separately from sunny-day behavior

### 5 V and 3.3 V Regulators

| Candidate class | Notes | Tag |
| --- | --- | --- |
| Documented buck regulator module from known vendor | Preferred for ESP, reader and radio power during prototype testing. Size for Wi-Fi peaks, not idle current. | `REF` |
| Generic LM2596 adjustable module | Common and cheap, but clones and pulse-load behavior vary. Use only after bench validation. | `PROT`, `RISK` |

Requirements:

- measure output ripple under lock actuation and Wi-Fi/MQTT peaks
- keep lock current off the ESP regulator path
- document dropout, thermal behavior and idle current

### Lock Driver

| Candidate class | Notes | Tag |
| --- | --- | --- |
| 12 V automotive-style relay module or relay stage | Simple, inspectable and isolated, but coil current and flyback handling must be documented. | `REF` |
| Logic-level MOSFET low-side switch | Useful for solenoids if the MOSFET is actually suitable for 3.3 V gate drive and inductive loads. | `REF` if engineered, `RISK` if generic |
| Smart high-side switch / protected load switch | Good reference for a later board if solenoid inrush and fault detection matter. | `REF` |

Requirements:

- lock output must be fused
- inductive loads need flyback protection
- remote command paths must never directly bypass access-control logic
- fail-secure vs fail-safe behavior must be decided per lock strategy

### Lock Mechanism Candidates

| Candidate class | Notes | Tag |
| --- | --- | --- |
| 9-12 V cabinet solenoid lock | Likely useful for mechanical experiments, but outdoor durability and duty cycle are uncertain. | `PROT`, `RISK` |
| Electric strike / latch module | Useful reference for controlled release, but DHL box geometry may not fit. | `PROT`, `RISK` |
| Reused or gutted original DHL lock position | Potentially best mechanical fit, but requires inspection, measurement and reliability testing. | `REF`, `RISK` |

Requirements:

- inspect the actual DHL latch before choosing a mechanism
- measure force, travel, actuation current and duty cycle
- include local mechanical emergency-opening assumptions
- protect wiring from outside manipulation

### Wiegand Keypad / RFID Readers

| Candidate class | Notes | Tag |
| --- | --- | --- |
| 12 V Wiegand keypad/RFID reader | Strong M1 candidate because many access modules already expose Wiegand D0/D1. | `REF` |
| Commercial multi-technology access reader with Wiegand or OSDP | Better lifecycle and documentation, but more expensive and sometimes vendor-bound. | `REF` |

Requirements:

- reader stays outside; controller stays inside
- Wiegand is credential presentation, not a secure channel
- do not store raw PINs in logs
- document LED/buzzer control separately from credential input

### NFC Modules

| Candidate class | Notes | Tag |
| --- | --- | --- |
| PN532-class NFC module | Useful for protected/internal NFC experiments or enclosed reader modules. | `REF` |
| Low-cost PN532 clone module | Useful only after bus stability and power behavior are tested. | `PROT`, `RISK` |

Requirements:

- distinguish NFC card identity from secure authentication
- do not oversell MIFARE Classic-style IDs as high security
- decide whether NFC is a user credential, admin enrollment path or both

### Fingerprint Modules

| Candidate class | Notes | Tag |
| --- | --- | --- |
| UART fingerprint module such as R503-class hardware | Useful for experiments; outdoor claims and template handling must be verified. | `PROT`, `RISK` |
| Commercial biometric access module | Better documentation may help, but legal/privacy burden remains high. | `REF`, `RISK` |

Requirements:

- biometric use is optional and should be prepared carefully
- template storage, consent and deletion need explicit privacy handling
- do not make fingerprint mandatory for M1

### Door and Tamper Sensors

| Candidate class | Notes | Tag |
| --- | --- | --- |
| Reed contact / magnetic door sensor | Simple door open/closed feedback path. | `REF` |
| Hall-effect sensor | More robust in some mechanical setups, but needs active power. | `REF` |
| NC tamper loop with microswitches | Useful for lid, controller enclosure and cable routing tamper detection. | `REF` |

Requirements:

- door events must be debounced in firmware
- tamper events should be logged separately from normal openings
- sensor wiring should be routed inside the protected area where practical

### Protection and Wiring

| Candidate class | Notes | Tag |
| --- | --- | --- |
| Branch fuses / blade fuses / polyfuses | Required for fault isolation in the 12 V tree. | `REF` |
| TVS diode on 12 V ingress | Useful transient protection; does not replace fusing. | `REF` |
| Reverse polarity protection | Strong candidate for later backplane input design. | `REF` |
| IP-rated cable glands | Required for outdoor or semi-outdoor wiring. | `REF` |
| M12 circular connectors | Useful reference for robust sensor/reader connections, but cost may be high. | `REF` |

Requirements:

- every external cable path needs strain relief
- outdoor penetrations need sealing and drip-loop thinking
- signal cables should not expose lock actuation directly
- connector choice should support serviceability without making bypass attacks easy

## Immediate Prototype Checklist

Before selecting a final M1 hardware direction:

- photograph and measure the actual DHL parcel box lock area
- document available components with part numbers and voltage/current ratings
- measure reader idle current
- measure lock actuation current and pulse time
- test DC/DC behavior under lock actuation
- sketch cable routing through the moving door
- classify every module as `PROT` or `REF`
- list outdoor, tamper and standby-current risks

## Open Questions

- Can the original DHL lock housing be reused as a mechanical mounting point?
- Is the first M1 prototype always-on, deep-sleep, or reader-wake based?
- Can solar realistically support a permanently powered outdoor Wiegand reader in winter?
- Should M1 prioritize relay simplicity or MOSFET/smart-switch efficiency?
- Which parts should be considered reference-design inputs for OpenParcelBox Core v1?

## Related Work

- [M1 DHL parcel box retrofit beta](../roadmap/m1-dhl-retrofit-beta.md)
- [Wiegand ACCESS-Port profile](wiegand-access-port.md)
- [Remote command security](../security/remote-command-security.md)
- GitHub issue: [#24 M1 Beta: Research ready-made hardware modules for retrofit controller](https://github.com/OpenParcelBox/specs/issues/24)
