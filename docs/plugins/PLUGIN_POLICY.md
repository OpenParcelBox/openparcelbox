# OpenParcelBox Plugin Policy

OpenParcelBox is designed to support plugins while keeping security-critical decisions under the control of the Core.

## Plugin classes

OpenParcelBox may support different plugin classes.

### Community Plugins

Community plugins may be developed by the community and used in self-hosted installations.

They may be open source or private, depending on the author and use case.

### Verified Plugins

Verified plugins may be reviewed, signed or approved for higher trust use.

They may receive additional permissions after review.

### Cloud Plugins

Cloud plugins may run through OpenParcelBox Cloud services.

They may be used for push, carrier APIs, hosted connectors, routing, trust registry integration or managed services.

Cloud plugins may be free or commercial.

### Enterprise Plugins

Enterprise plugins may be used for locker fleets, housing providers, companies, carrier systems, public locker networks or white-label deployments.

They may be subject to contracts, SLA, audits or certification requirements.

## Plugin licensing

The OpenParcelBox Plugin SDK and API client libraries are intended to be licensed under Apache-2.0.

Plugins may be open source or proprietary, depending on the author, integration type, API terms and commercial agreements.

## Core authority

Plugins must not directly execute security-critical actions such as unlocking a physical lock.

Plugins may request actions.

The OpenParcelBox Core decides.

The Core should remain responsible for:

- unlock decisions
- access grants
- role checks
- policy checks
- event logging
- owner-controlled decisions
- device ownership checks
- audit behavior

## Permission model

Potential plugin permissions include:

- `read_status`
- `read_events`
- `request_unlock`
- `manage_users`
- `manage_devices`
- `carrier_integration`
- `enterprise_admin`

Higher-risk permissions should require explicit approval, configuration, signing, verification or certification.

## Carrier and business plugins

Carrier, business, enterprise or partner plugins may be proprietary when API contracts, credentials, security requirements or commercial agreements require it.

The existence of proprietary plugins should not prevent local or self-hosted operation of the open core.

## Self-hosted installations

Self-hosted installations should be able to use local plugins and documented connector interfaces.

Where central services are needed, self-hosted installations should be able to connect through documented connectors when technically and legally possible.

## Security-sensitive plugin behavior

Plugins should not:

- bypass Core access decisions
- disable logging without explicit policy
- store tokens insecurely
- expose unlock endpoints without authorization
- silently change ownership
- hide security-relevant events
- claim official certification without approval
