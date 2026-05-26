# OpenParcelBox Development Hub (draft)

Status: planning draft — this file describes how to run the GitHub **Project** and related issue workflow so work does not scatter across chat tools.

## Purpose

- Give contributors a **single queue** for M1 and cross-cutting work.
- Separate **research** (private inbox, external AI) from **product decisions** (this repo, issues, PRs).
- Keep **local-first**, **no-230V-prototypes**, and **optional cloud/carrier** constraints visible on every card.

## GitHub Project setup (maintainers)

1. In the `OpenParcelBox` organization, open or create a Project named **OpenParcelBox Development Hub** (name from project bootstrap notes).
2. Link the project to repositories: **`OpenParcelBox/specs`**, **`OpenParcelBox/www`**, and optionally **`OpenParcelBox/openparcellock`**.
3. Recommended **custom fields** (table view):
   - **Priority:** P0 / P1 / P2 / P3
   - **Area:** firmware, hardware, access-control, security, website, governance, integrations, research
   - **Milestone:** M0, M1, later
   - **Risk:** low / medium / high (security or hardware safety)
4. Create two saved views:
   - **Board** — columns below, grouped by status.
   - **Roadmap table** — sort by Milestone, then Priority.

## Default board columns

Map GitHub **Status** (or Project column) to these meanings:

| Column | Meaning |
|--------|---------|
| **Intake / Triage** | New idea, needs scope, may be closed or split. |
| **Backlog** | Accepted, clear enough to schedule, not next. |
| **Ready** | Fully specified starter: acceptance criteria + links to docs. |
| **In progress** | Someone is actively working (issue assignee + draft PR if code). |
| **Needs decision** | Architecture, legal, safety, or vendor trade-off — short ADR or discussion outcome required. |
| **In review** | PR or doc PR open; request review from maintainers. |
| **Done** | Merged or explicitly wontfix with rationale. |

## Swimlanes (optional)

Use **Area** or a parent **Epic** issue instead of physical swimlanes:

- **M1 DHL retrofit** — hardware bring-up, lock strategy, power, enclosure.
- **Firmware platform** — toolchain, OTA, web admin, MQTT/API baseline.
- **Access model** — opening rights, keyring, carrier presentation formats.
- **Sustainability narrative** — neighborhood capacity framing (docs + website only until product exists).
- **Operations** — website build checks, DNS, Pages, discussion hygiene.

## Epics (parent issues to create)

Each epic should stay a **tracking issue** with checkboxes; child issues carry the real work.

1. **Epic: M1 beta — DHL retrofit controller**  
   Anchor: `docs/roadmap/m1-dhl-retrofit-beta.md` and GitHub issue **#24** (ready-made module research).  
   Exit: documented power tree, lock measurement protocol, reproducible BOM shortlist linked from docs.

2. **Epic: Firmware platform decision (ESP32-C6)**  
   Exit: ADR-style decision covering ESP-IDF vs Arduino vs ESPHome-hybrid, AP-mode onboarding, local webserver, OTA strategy, MQTT/local API, and **rolling-code / token** direction (even if first implementation is deferred).

3. **Epic: Keyring and multi-tenant model**  
   Exit: documented breakdown for org / household / user / carrier delegation / subkeys / temporary rights, aligned with `docs/access-control/keyring-model.md` and `opening-rights.md`.

4. **Epic: Neighborhood capacity & sustainability messaging**  
   Exit: concept doc + cautious website copy; **no** shipped feature implied. Research inputs may live in the private research inbox reviewed notes.

5. **Epic: Website & Pages quality gate**  
   Exit: checklist run on `openparcelbox.org` after each Pages deploy (links, DE/EN parity, mission statements).

## Ready-to-create child issues (copy into GitHub)

Maintainers: create these as separate issues, then attach them to the Project and set **Area** / **Priority**.

---

### Issue: Firmware — ADR: ESP32-C6 platform (IDF vs Arduino vs hybrid)

**Priority:** P0  
**Area:** firmware  
**Milestone:** M1

**Description**

Choose the baseline firmware stack for the M1 retrofit controller. The decision must cover:

- ESP32-C6 support and long-term vendor alignment
- **AP-mode** onboarding and captive portal or equivalent local-first UX
- Local **HTTP** management surface (minimal acceptable feature set)
- **OTA** update policy (signing, rollback, offline failure modes)
- MQTT and **local HTTP API** as optional compile-time or runtime modules
- Perspective for **rolling codes** / time-limited tokens (may defer implementation but must not paint us into a dead end)

**Acceptance criteria**

- ADR markdown under `docs/` (path agreed in PR) with options, decision, and consequences.
- Explicit statement on Home Assistant / ESPHome: optional only.
- Security notes for remote commands cross-link `docs/security/remote-command-security.md`.

**Non-goals**

- Full production hardening in the same issue.
- Final carrier integration.

---

### Issue: Docs — Neighborhood capacity sharing (concept only)

**Priority:** P2  
**Area:** governance / website  
**Milestone:** M1 (docs) / later (product)

**Description**

Add `docs/community/neighborhood-capacity-sharing.md` describing voluntary, privacy-preserving **shared local receiving capacity** as a **concept**. Include non-goals, consent sketch, audit sketch, and a visible banner that the feature is **not implemented**.

**Acceptance criteria**

- No unsourced CO₂ savings claims.
- Links to credible framing sources (EU transport study, peer-reviewed locker last-mile literature) only for general context; label vendor logistics benchmarks correctly if used.
- Cross-links to `docs/governance/PRINCIPLES.md` and access-control docs.

**Non-goals**

- Legal liability conclusions.
- Implementing routing or neighbor discovery in firmware.

---

### Issue: Website — Post-Pages deploy smoke checklist

**Priority:** P3  
**Area:** website  
**Milestone:** M0

**Description**

Maintain a short checklist (in this issue or linked doc) to run after each website deploy: home + `/de/` links, key messaging (DHL retrofit beta, open local capacity narrative, manufacturer responsibility), and asset URLs on `openparcelbox.org`.

**Acceptance criteria**

- Checklist executed once and findings filed as follow-up issues if needed.

---

### Issue: Access control — Keyring decomposition workshop

**Priority:** P1  
**Area:** access-control  
**Milestone:** M1

**Description**

Break down organization / household / user / carrier / subkeys / temporary rights into concrete state machines and examples. Output should extend `docs/access-control/keyring-model.md` without contradicting `opening-rights.md`.

**Acceptance criteria**

- At least three worked examples (delivery-only right, pickup, delegated cleaner).
- Explicit threat notes for coercion and social pressure where neighborhood features are mentioned.

---

## Research inbox (private)

Delegated research tasks and reviewed summaries may live in **`OpenParcelBox/openparcelbox-research-inbox`** (or a personal fork used for dispatcher experiments). They are **inputs** to issues above, not substitutes for GitHub tracking.

## Revision

Update this draft when the GitHub Project fields or column names are finalized so newcomers are not confused by drift between this document and the live board.
