# Security Policy

OpenParcelBox is a security-sensitive project.

It may control physical locks, parcel access, delivery permissions, users, tokens, cloud connectors and potentially public or semi-public locker infrastructure.

OpenParcelBox is currently in an early planning and specification phase. No production hardware, firmware or security guarantees exist yet.

## Responsible disclosure

Please do not publish exploit details in public issues, discussions or pull requests.

If you believe you have found a security vulnerability, report it privately to:

GitHub private vulnerability reporting, once enabled for the repository.

Until that is available, contact the project maintainer privately through the GitHub organization instead of opening a public issue.

Please include:

- affected component
- version, commit or branch if known
- technical description
- potential impact
- steps to reproduce if safe to share
- suggested mitigation if available

## Security-sensitive areas

Security-sensitive areas include, but are not limited to:

- bridge firmware
- lock control logic
- pairing process
- authentication and authorization
- token handling
- certificate handling
- firmware update process
- plugin permission model
- cloud connectors
- carrier integrations
- device ownership transfer
- event logs
- access grants
- remote unlock workflows
- public locker routing
- trust registry and certificate revocation

## Core security principle

Plugins may request security-sensitive actions, but they must not directly execute them.

The OpenParcelBox Core should remain the final authority for:

- unlock decisions
- access grants
- role and policy checks
- event logging
- owner-controlled decisions
- device identity checks
- security boundaries

In short:

> Plugins request. The Core decides.

## Planned security orientation

OpenParcelBox aims to align its design with relevant consumer IoT security expectations, including principles commonly associated with standards and regulations such as ETSI EN 303 645 and the EU Cyber Resilience Act.

This does not mean that OpenParcelBox is currently certified, compliant or production-ready.

## No production guarantee

At this stage, OpenParcelBox is not suitable for unsupervised production deployment, security-critical use or commercial locker operation without independent review, testing and appropriate legal, operational and security assessment.

## Public issues

Public issues are welcome for general bugs, documentation problems and non-sensitive hardening ideas.

Do not post:

- working exploit chains
- private keys
- tokens
- credentials
- bypass techniques
- carrier API secrets
- confidential integration details
