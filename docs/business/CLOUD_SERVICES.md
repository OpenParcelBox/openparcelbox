# OpenParcelBox Cloud Services

OpenParcelBox is local-first and self-hostable.

Some services are still useful or necessary as shared cloud services because they require common identity, routing, trust, public discovery, carrier coordination, push delivery, certificates or managed security.

## Central services that may make sense

OpenParcelBox may provide central services such as:

- OpenParcelBox ID / unified number
- OpenParcelBox account
- public locker directory
- trust registry
- plugin registry
- certificate and signature service
- carrier routing
- push relay
- firmware update channels
- device claim / ownership proof
- public compatibility registry
- certified hardware lookup
- connector directory

## Free ecosystem services

Some central services may be provided for free because they strengthen the ecosystem.

Examples:

- basic OpenParcelBox ID
- public project account
- community plugin registry
- certified device lookup
- public locker listing for community lockers
- device ownership proof mechanisms
- basic connector discovery

## Hosted and commercial services

Other services may require commercial terms because they create operating costs, support obligations, liability, integration work or contractual obligations.

Examples:

- managed cloud
- push at scale
- hosted backups
- carrier integrations
- enterprise routing
- SLA
- white label
- professional support
- managed updates
- custom connectors

## Self-hosted connector model

Self-hosted systems should be able to connect to central services through documented connectors where technically and legally possible.

A self-hosted instance may use central services for:

- identity
- trust registry
- public locker discovery
- plugin verification
- carrier routing
- push relay
- update checks

The central service should not unnecessarily replace local functions.

## Owner-controlled access decisions

The OpenParcelBox Cloud may provide identity, discovery, routing and trust services.

The actual access decision should remain with the owner-controlled Core whenever technically possible.

In short:

> Cloud may route and verify. The owner-controlled Core decides.

## No artificial cloud lock-in

OpenParcelBox should avoid moving features to central cloud services merely to force subscription.

Central services should be used where shared infrastructure creates real value.
