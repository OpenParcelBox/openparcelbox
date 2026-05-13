# Contributing to OpenParcelBox

Thank you for your interest in contributing to OpenParcelBox.

OpenParcelBox is a community-friendly open ecosystem. Contributions are welcome, but the project must remain legally clean, secure and trustworthy because it may control physical locks, parcel access and security-sensitive integrations.

## Contribution license

By contributing to OpenParcelBox, you agree that your contribution is submitted under the license of the repository, directory or component to which you contribute.

Different parts of OpenParcelBox may use different licenses. See `LICENSES.md`.

## Developer Certificate of Origin required

All contributions must include a Developer Certificate of Origin (DCO) sign-off.

This means you certify that:

- you wrote the contribution yourself, or
- you have the right to submit it under the applicable project license, and
- the contribution may be used, modified and distributed as part of OpenParcelBox under the applicable license.

To sign off a commit, use:

```bash
git commit -s -m "Your commit message"
```

This adds a line like:

```text
Signed-off-by: Your Name <your.email@example.com>
```

Pull requests without a valid DCO sign-off may not be merged.

## Do not submit material you do not have rights to

Do not submit:

- code copied from another project without compatible licensing
- confidential information
- leaked API documentation
- carrier API credentials
- proprietary SDKs without permission
- third-party trademarks or logos without permission
- security exploit details in public issues
- content you are not authorized to contribute

## Security-sensitive project

OpenParcelBox may control physical locks and parcel access. Security-sensitive changes require extra care.

Security-sensitive areas include:

- bridge firmware
- lock control logic
- authentication and authorization
- pairing
- update mechanisms
- token handling
- certificate handling
- plugin permissions
- cloud connectors
- carrier integrations
- event logging
- access decisions

## Plugin security model

Plugins may request access-related actions, but they must not directly force security-critical actions.

The OpenParcelBox Core should remain responsible for:

- unlock decisions
- access grants
- role checks
- policy checks
- event logging
- audit behavior
- device ownership checks

In short:

> Plugins request. The Core decides.

## Coding and documentation expectations

Contributions should be:

- understandable
- documented where needed
- testable where practical
- aligned with local-first and self-hostable principles
- designed without artificial cloud lock-in
- careful with user data and security assumptions

## Community and commercial work

OpenParcelBox welcomes community contributions and may also support commercial integrations, consulting, certification and enterprise work.

Commercial partners do not automatically receive control over the community project, roadmap or official releases.
