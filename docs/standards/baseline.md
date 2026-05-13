# Standards Baseline

OpenParcelBox should be standards-oriented without copying protected standards text.

This document captures the public standards baseline used for early project planning. It is not a compliance claim.

## Standards Tracks

OpenParcelBox separates two concerns:

- Physical receptacle requirements: parcel-box size, weather resistance, ergonomics, mechanical security and delivery security.
- Digital access requirements: opening rights, authentication, authorization, revocation, auditability and interoperability.

## CEN/TS 17457:2020

CEN/TS 17457:2020 is the key public reference for digital, optionally online-connected opening and closing systems for home-use parcel receptacles.

OpenParcelBox uses it as a requirements compass for:

- secure electronic authentication of delivery actors
- interoperability between market participants
- digital opening and closing systems
- optional online connectivity
- access by delivery operators, collection operators and consumers

OpenParcelBox documentation must not copy protected standard text. Public project docs should use original project language and cite public catalogue pages or legally accessible summaries.

## CEN/TS 16819

CEN/TS 16819 is relevant for physical parcel-box assumptions. Public summaries describe topics such as parcel dimensions, ergonomics, safety, corrosion resistance, water penetration resistance and delivery security.

OpenParcelBox is retrofit-oriented, so not every physical receptacle can be treated as compliant by default. Existing boxes, cabinets and garage boxes need explicit assumptions and limitations.

## Initial Physical Receptacle Checklist

This checklist translates public descriptions of CEN/TS 16819 into OpenParcelBox planning criteria. It is conservative and conceptual. It does not claim conformance.

Legend:

- Covered: clearly part of current OpenParcelBox assumptions or design goals.
- Partially covered: acknowledged or intended, but not yet specified in CEN/TS 16819-level detail.
- Out of scope: intentionally outside the current OpenParcelBox physical scope.

| Area | Physical criterion | Status | Notes |
|---|---|---|---|
| Classification | Box type classification by receiver count and single/multiple delivery capability | Partially covered | OpenParcelBox targets home and small shared use, but does not yet map form factors to formal parcel-box type classes. |
| Dimensions | Standard parcel size classes | Partially covered | OpenParcelBox refers to realistic parcel sizes, but has not yet adopted CEN/TS 16819 size classes as reference vocabulary. |
| Dimensions | Gauge-parcel verification | Partially covered | Clearance and removability matter, but no formal gauge-parcel test procedure exists yet. |
| Dimensions | Documented capacity | Partially covered | Future profiles should state supported parcel dimensions and, where possible, corresponding standard size classes. |
| Ergonomics | Installation height guidance | Covered | OpenParcelBox should document usable heights for owners and delivery staff; retrofit deviations must be recorded. |
| Accessibility | Grouped box accessibility requirements | Out of scope | The current project focus is single or small numbers of receptacles, not large banks of boxes. Revisit for multi-unit deployments. |
| Safety | Sharp-edge and injury prevention | Covered | User-accessible components must be mechanically safe independent of formal certification status. |
| Safety | Child entrapment and ventilation for large receptacles | Partially covered | Small boxes may be physically unable to contain a child; garage/storage-room scenarios require explicit emergency opening and ventilation analysis. |
| Safety | Warning labels and safety instructions | Partially covered | Documentation is in scope, but physical labels and wording are not yet specified. |
| Fire/building | National fire and escape-route requirements | Partially covered | OpenParcelBox cannot define building-level compliance. Installers/operators must follow local building rules. |
| Corrosion | Corrosion resistance grades | Partially covered | Outdoor-capable hardware is intended, but target grades and test evidence are not selected. |
| Weather | Water penetration / rain protection | Partially covered | Weather resistance is a design goal; profiles must distinguish sheltered and exposed installations and select a test target. |
| Security | Graded mechanical resistance of doors and locks | Partially covered | Digital access control is not a substitute for mechanical resistance. Target security grades are not defined yet. |
| Security | Fixings and casing strength | Partially covered | Retrofit mounting substrates vary heavily; substrate-specific fixing guidance is needed. |
| Security | Non-standard door systems | Partially covered | Gate and garage scenarios need separate mechanical assumptions. |
| Testing | Dimension, ergonomics and safety tests | Partially covered | A test plan is required before any prototype claims can be made. |
| Testing | Corrosion and water tests | Partially covered | Test method and target grade remain open. |
| Testing | Mechanical security tests | Partially covered | Force levels and pass/fail criteria are not specified yet. |
| Marking | Classification labels and installation instructions | Partially covered | Installation documentation is planned; standard-style marking is not yet adopted. |

## Retrofit Constraints

OpenParcelBox is retrofit-oriented. That creates practical limits that purpose-built parcel-box standards do not automatically solve.

### Existing Geometry

Existing doors, walls, cabinets, letterboxes and garage structures may not provide an ideal parcel aperture or internal volume. Each retrofit profile should state which parcel size classes or maximum parcel dimensions are realistically supported.

### Installation Height

Existing cut-outs or building details may force a keypad, reader, handle or door opening outside ideal ergonomic ranges. OpenParcelBox should document target heights and clearly mark retrofit deviations.

### Mounting Substrates

Thin sheet metal, insulated facade panels, hollow masonry, wooden cabinets and lightweight garage doors have different mechanical capacities. A retrofit profile should either provide reinforcement/fixing guidance or explicitly cap the expected mechanical security level.

### Weather Exposure

Sheltered entrances, recessed walls and indoor garage scenarios differ from fully exposed outdoor boxes. OpenParcelBox should classify installations as at least:

- indoor
- sheltered outdoor
- exposed outdoor

Each class needs different sealing, drainage and corrosion assumptions.

### Large-Volume Receptacles

Garage, storage-room or large cabinet scenarios can create child-entrapment and ventilation risks. These scenarios require a separate safety model from small parcel boxes.

### Non-Box Receptacles

CEN/TS 17457 public summaries mention that digital opening systems can apply beyond classic boxes when physical requirements are met where applicable. OpenParcelBox should document, per receptacle type, which physical criteria are:

- fully applicable
- partially achievable
- not applicable but compensated by operational controls

## Legitimate Access to Standards

The project should use legal access paths for standards review.

Known legal options:

- Buy DIN CEN/TS 16819 and DIN CEN/TS 17457 through DIN Media or another national standards body shop.
- Use a DIN standards info point at participating universities and libraries for on-site reading.
- Use institutional access through databases such as Nautos where a university or library provides access.
- Use short paid online viewing options where available, such as DIN Media reading/time-contingent access.

Public repositories must not include copied protected standards text. Private compliance notes may summarize reviewed requirements in original language and should record who had legitimate access and when.

Open issue: the project has researched legal access paths, but has not yet obtained or documented actual maintainer access to licensed copies.

## Policy Watch

The project should maintain a lightweight watch list for:

- CEN/TS 17457
- CEN/TS 16819
- OPEN GIE
- German Postgesetz 2024
- Bundesnetzagentur postal market monitoring
- WIK research on provider-neutral parcel stations
- ERGP postal network access discussions

## Rules

- Mark legal and regulatory conclusions as interpretation unless based on official wording.
- Distinguish private home receptacles from provider-operated automated stations.
- Keep public documentation free of protected standards text.
- Track which statements come from public summaries and which require licensed standard review.
