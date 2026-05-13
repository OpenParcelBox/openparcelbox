# OpenParcelBox Editions

OpenParcelBox is designed as a local-first, self-hostable and open-ecosystem platform with optional hosted and enterprise services.

## Edition overview

| Edition | Operation | Main audience |
|---|---|---|
| OpenParcelBox Core | Local device / bridge | Everyone |
| OpenParcelBox Cloud Community Edition | Self-hosted | Makers, Home Assistant users, small installations |
| OpenParcelBox Cloud Hosted / Managed Cloud | Hosted by OpenParcelBox | Plug-and-play users, non-technical users |
| OpenParcelBox Cloud Enterprise | Hosted, on-prem or hybrid | Companies, locker systems, housing providers, professional deployments |

## OpenParcelBox Core

OpenParcelBox Core includes the local device and bridge foundation.

Typical scope:

- bridge firmware
- lock logic
- sensor handling
- local API
- MQTT
- Home Assistant support
- local policy checks
- local access decisions
- local event generation
- secure pairing concepts
- update mechanisms

## OpenParcelBox Cloud Community Edition

OpenParcelBox Cloud Community Edition is the self-hosted edition of the OpenParcelBox cloud platform.

It is intended to provide an open backend for managing bridges, locks, plugins, users, events and integrations on infrastructure controlled by the owner.

Typical scope:

- self-hosted backend
- open source code
- local or self-hosted plugins
- MQTT and Home Assistant integration
- local users
- local events
- local APIs
- simple access workflows
- manual updates
- manual backups
- community support

## OpenParcelBox Cloud Hosted / Managed Cloud

OpenParcelBox Cloud Hosted or Managed Cloud is the optional hosted service operated by the OpenParcelBox project.

It may provide:

- remote access
- push notifications
- managed backups
- managed updates
- hosted connectors
- optional carrier integrations
- account management
- device claim support
- trust services
- plugin distribution
- support

The hosted cloud should provide convenience, managed operation and integrations. It should not exist to artificially disable local or self-hosted functionality.

## OpenParcelBox Cloud Enterprise

OpenParcelBox Cloud Enterprise is intended for professional locker systems, companies, housing providers, public or semi-public locker networks and managed deployments.

It may provide:

- multi-tenant management
- locker fleets
- multiple locations
- advanced role models
- long-term audit logs
- SLA
- SSO
- custom plugins
- carrier integrations
- enterprise APIs
- white label
- managed rollout
- professional support
- compliance and security review support

## No artificial limitation principle

The Community Edition should not be deliberately crippled.

Paid services should exist where operation, integration, security, support, certification, liability or convenience create real costs.
