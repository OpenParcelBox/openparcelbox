# Developer Certificate of Origin

OpenParcelBox uses the Developer Certificate of Origin (DCO) process for contributions.

The DCO is a lightweight way for contributors to certify that they have the right to submit their contribution under the applicable project license.

## What signing off means

By adding a `Signed-off-by` line to your commit, you certify that:

1. The contribution was created by you, or you have the right to submit it.
2. You are allowed to submit it under the applicable OpenParcelBox license.
3. The contribution may be used, modified and distributed as part of OpenParcelBox under that license.
4. You understand that the contribution is intended to remain part of the project and cannot later be used as a demand to remove properly licensed project history.

## How to sign off a commit

Use:

```bash
git commit -s -m "Your commit message"
```

This adds:

```text
Signed-off-by: Your Name <your.email@example.com>
```

Example:

```bash
git commit -s -m "Add MQTT topic for lock state"
```

Result:

```text
Add MQTT topic for lock state

Signed-off-by: Jane Developer <jane@example.com>
```

## Pull requests

Every commit in a pull request should contain a valid DCO sign-off.

Pull requests without valid sign-off may be rejected or returned for correction.

## Correcting a missing sign-off

For the most recent commit:

```bash
git commit --amend -s
git push --force-with-lease
```

For multiple commits, use an interactive rebase and add sign-offs to the affected commits.

## Project-specific DCO statement

By signing off a contribution to OpenParcelBox, you certify that the contribution is submitted under the applicable license of the relevant repository, directory or component, and that the OpenParcelBox project may use, modify and distribute the contribution under that license.
