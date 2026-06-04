# Outcome — Risk-Based Test Planning (Sprint 6, ToolShop)

**Sources:** `sprint6/sprint6-input/sprint6-content.md`, `sprint6/sprint6-input/sprint6-sprintGoal.md`, individual `US*.md` files in `sprint6/sprint6-input/`.

**Risk dimensions used:** business impact, technical complexity, integration risk, security relevance, production impact, user visibility, regression potential.

**MoSCoW slot targets:** MUST 3 · SHOULD 1 · COULD 1 · WON'T 4 (9 stories).

---

## Prompt A — structured risk-first (Chat-style general model)

```
You are a senior test strategist for the ToolShop e-commerce webshop.

Context — Sprint 6 goal:
1. Czech market entry: language, localized products, PayU payment, local delivery options.
2. Security foundation: password hashing, replace vulnerable auth library.

User stories in scope (summarised):
- US2300: Full Czech language support (switcher, UI, products, validation messages).
- US2350: Czech product titles/descriptions for selected articles.
- US2450: Delivery cost rules (free over €75, flat rate, oversized surcharge, express).
- US3100: PayU payment integration for Czech customers (region gating, webhooks, no card storage).
- US3150: New delivery option — Zásilkovna (Czech parcel pickup network).
- US3206: Hash passwords with MD5 (currently plaintext); all set/change paths + bulk migration.
- US3409: Replace xauth-X32 with xauth-X64 (ISEC-2499, CVSS 8.1, session manipulation risk).
- US4700: Login step during checkout for guest users (UC1/UC2 acceptance criteria).
- US5003: Replace ORMapper V1.2 → V2.0 (historical wrong-field bug, schema migration, perf AC).

Testing capacity is limited — not every story gets deep test design this sprint.

Task:
1. Score each story on: business impact, technical complexity, integration risk, security relevance, production impact, user visibility, regression potential (Low / Medium / High).
2. Identify the top 3 testing risks per story (concrete failure modes, not generic labels).
3. Assign each story to MoSCoW for *testing focus*: MUST 3, SHOULD 1, COULD 1, WON'T 4.
4. For each band, state suggested testing focus (layers, scenarios, NFR checks).

Output as a markdown table: Story ID | MoSCoW | Top risks | Suggested testing focus | One-line rationale.
Then a short paragraph on trade-offs you made filling the MoSCoW slots.
```

### Prompt A — model output (summary)


| Story ID | MoSCoW | Top risks                                                                | Suggested testing focus                                                         | Rationale                                               |
| -------- | ------ | ------------------------------------------------------------------------ | ------------------------------------------------------------------------------- | ------------------------------------------------------- |
| US3100   | MUST   | Callback replay; region mis-gating; order-state desync                   | Webhook signatures, region gating, failure/timeout paths, no local card storage | Payment gate for Czech market validation                |
| US3409   | MUST   | Session token reuse; concurrent login instability; refresh rotation bugs | Session lifecycle matrix, forced logout, multi-device regression, vuln scan     | ISEC-2499 / CVSS 8.1 affects all auth                   |
| US5003   | MUST   | Migration data loss; wrong-field bug persists; perf AC miss              | Migration reconciliation, schema/denorm checks, REF_ORG_Data perf (< 1.5 s)     | Platform-wide persistence; silent corruption            |
| US4700   | SHOULD | Guest blocked after login; cart loss on handoff; breaks PayU step        | UC1/UC2 AC, cart continuity, handoff to payment                                 | High revenue impact but narrower than platform layers   |
| US3206   | COULD  | Plaintext remnants; failed bulk migration; lockouts                      | All set/change paths + bulk conversion                                          | Security relevant but MD5 is interim; overlaps US3409   |
| US3150   | WON'T  | Pickup selection failure; label/API errors                               | Sample in CZ checkout journey only                                              | Important UX; lower blast radius than payment/auth/data |
| US2300   | WON'T  | Wrong/missing translations erode trust                                   | Locale smoke checklist                                                          | Content surface; spot-check vs deep matrix              |
| US2350   | WON'T  | Wrong or missing product copy                                            | Content QA on selected PDPs                                                     | Narrow content scope                                    |
| US2450   | WON'T  | Incorrect shipping totals                                                | Fold rule checks into checkout E2E samples                                      | Rules exercised via journey, not standalone campaign    |


**Trade-offs (model):** Reserved three MUST slots for financial (PayU), security-critical (auth lib), and platform-wide (ORMapper) failures. Demoted US4700 to SHOULD because checkout login is high-impact but testable within PayU journey samples; US3206 kept COULD over US3150 because plaintext storage is an active breach risk today.

---

## Prompt B — business-journey weighted (different model / instruction style)

```
I'm preparing a risk-based test plan for ToolShop Sprint 6. We can only deeply test a subset of 9 user stories.

Stories: US2300 (Czech UI), US2350 (Czech product copy), US2450 (shipping price rules), US3100 (PayU), US3150 (Zásilkovna pickup), US3206 (MD5 password hashing), US3409 (auth library CVE fix), US4700 (login in checkout), US5003 (ORMapper migration).

Before you prioritise, ask yourself:
- Which failures would stop us validating the Czech market hypothesis?
- Which failures could lose money or leak credentials in production?
- Which changes touch the most code paths or data?

Constraints:
- MoSCoW quotas are fixed: MUST 3, SHOULD 1, COULD 1, WON'T 4.
- WON'T means no dedicated test campaign — only smoke/sampling in broader journeys.
- Call out where two stories compete for the same slot and why you picked one.

Return:
1. Ranked list (1–9) by testing urgency.
2. MoSCoW assignment matching the quotas.
3. For disagreements between rank and MoSCoW, explain.
4. One risk each story might hide if you only read the title.
```

### Prompt B — model output (summary)

**Ranked by testing urgency (1 = highest):**

1. US3100 · 2. US3409 · 3. US4700 · 4. US5003 · 5. US3206 · 6. US3150 · 7. US2450 · 8. US2300 · 9. US2350


| MoSCoW | Stories                        |
| ------ | ------------------------------ |
| MUST   | US3100, US3409, US4700         |
| SHOULD | US5003                         |
| COULD  | US3150                         |
| WON'T  | US2300, US2350, US2450, US3206 |


**Slot competition noted:** US5003 vs US4700 for third MUST — model picked US4700 because Czech golden path (guest → login → PayU) cannot be validated without checkout login; US5003 demoted to SHOULD. US3206 vs US3150 for COULD — model picked US3150 for Czech delivery hypothesis; US3206 WON'T with reliance on US3409 for auth hardening.

**Hidden risks if reading titles only:**

- US2450: Oversized surcharge stacking with express and free-shipping threshold edge cases.
- US3206: Bulk MD5 conversion may invalidate passwords for users mid-session.
- US5003: Mapping bug may have silently corrupted historical orders, not just future writes.

---

## Model comparison (Prompt A vs Prompt B)


| Dimension             | Prompt A (structured risk-first)                                                      | Prompt B (business-journey weighted)                                                     | My view                                                                                             |
| --------------------- | ------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| **Risk picture**      | Emphasises systemic layers: PayU webhooks, auth sessions, ORMapper data integrity     | Emphasises customer journey: checkout login + Czech market validation path               | Both valid; A catches data-layer risk earlier, B catches funnel risk                                |
| **Prioritisation**    | MUST: US3100, US3409, US5003 · SHOULD: US4700                                         | MUST: US3100, US3409, US4700 · SHOULD: US5003                                            | Agree with B on MUST trio; US4700 blocks Czech hypothesis more visibly than ORMapper in sprint demo |
| **Reasoning quality** | Explicit dimension scores; ties risks to layers (contract, session matrix, migration) | Explicit Czech-market hypothesis framing; names slot competition                         | B surfaces *why* checkout login competes with ORMapper for MUST; A is better for audit trail        |
| **Identified risks**  | Callback tampering, session token reuse, wrong DB fields post-migration, MD5 weakness | Checkout abandonment if login breaks, PayU region mis-gating, silent ORMapper corruption | A names more technical risks; B names more revenue-path risks                                       |
| **Testing focus**     | Layer-specific (webhook signatures, forced logout, reconciliation queries)            | Journey-specific (guest checkout → PayU → Zásilkovna handoff)                            | Consolidated plan uses both                                                                         |


**Disagreements with model suggestions:**

- **US5003 (ORMapper):** Prompt A placed it MUST; I agree with Prompt B at **SHOULD** — migration is critical but checkout/payment/auth must pass first; deep ORMapper work follows once MUST paths are green.
- **US3206 (MD5):** Prompt B WON'T it; I place it **COULD** — plaintext storage is an active breach risk today; MD5 is weak but bulk migration and all auth paths need time-boxed coverage after MUST/SHOULD.
- **US3150 (Zásilkovna):** Prompt B COULD; I **WON'T** — important for CZ UX but validate pickup selection via checkout journey samples rather than a standalone campaign.

---

## Part 1 — Story risk analysis (consolidated)


| ID         | Business impact           | Technical complexity       | Integration risk            | Security relevance               | Production impact          | User visibility    | Regression potential |
| ---------- | ------------------------- | -------------------------- | --------------------------- | -------------------------------- | -------------------------- | ------------------ | -------------------- |
| **US2300** | High (trust, market)      | Medium (i18n surface)      | Low–medium                  | Low                              | Medium                     | Very high          | Medium               |
| **US2350** | High (conversion)         | Low–medium                 | Low                         | Low                              | Medium                     | High (PDP)         | Low–medium           |
| **US2450** | High (revenue, disputes)  | Medium (rules, edge cases) | Medium (checkout)           | Low                              | High (wrong totals)        | High (checkout)    | High                 |
| **US3100** | Very high                 | High (PSP, webhooks)       | Very high                   | Very high                        | Very high                  | High               | Very high            |
| **US3150** | High (CZ delivery UX)     | Medium (carrier API)       | Medium (checkout, labels)   | Low                              | Medium                     | High (CZ checkout) | Medium               |
| **US3206** | High (breach)             | Medium (all auth paths)    | Medium                      | High (plaintext today; MD5 weak) | High (lockouts, migration) | Medium             | Very high            |
| **US3409** | High (compliance)         | High (lib swap)            | High (all auth consumers)   | Very high (ISEC-2499)            | High                       | Low direct         | Very high            |
| **US4700** | High (abandoned checkout) | Medium (state machine)     | Medium (guest ↔ auth)       | Medium (session handoff)         | High                       | Very high          | High                 |
| **US5003** | High (wrong data)         | Very high (migration)      | Very high (all persistence) | Medium                           | Very high                  | Low direct         | Very high            |


---

## Part 2 — MoSCoW reasoning (my judgement)

**MUST (3)** — Reserve for failures that block **Czech market validation**, are **security-critical**, or **stop revenue**:

- **US3100 (PayU):** Czech market cannot be validated without payment; webhook/order-state bugs cause double charges or stuck orders; PCI-style NFRs (no card storage).
- **US3409 (auth library):** ISEC-2499 / session manipulation affects every authenticated flow; forced logout and regression matrix required before release.
- **US4700 (login in checkout):** Documented checkout abandonment when guests cannot finish; intersects PayU and session handling — failure blocks the Czech purchase funnel.

**SHOULD (1)** — Next highest blast radius, not fully covered by MUST:

- **US5003 (ORMapper):** Historical wrong-field persistence + full schema migration → silent data corruption and perf regression across all reads/writes; deep campaign once MUST paths pass.

**COULD (1)** — Important but narrower or deferrable with sampling:

- **US3206 (MD5 hashing):** Many touchpoints (register, profile, forgot password, bulk conversion) and plaintext today; auth library work (US3409) reduces session risk; MD5 is tech debt — time-box after MUST/SHOULD.

**WON'T (4)** — Smoke/sampling only this sprint:

- **US2300, US2350:** Content and locale — spot checks and journey smoke, not full UI matrix vs payment/auth/checkout capacity.
- **US2450:** Pricing rules exercised via checkout E2E samples; no standalone rule-engine campaign.
- **US3150:** Zásilkovna pickup — validate in CZ checkout journey samples alongside US2450/US3100, not a standalone deep campaign.

---

## Outcome — Prioritized user stories (consolidated table)


| MoSCoW     | User story                         | Main testing risks                                                                                                | Suggested testing focus                                                                                                        |
| ---------- | ---------------------------------- | ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| **MUST**   | **US3100** — PayU                  | Callback replay/tampering; region mis-gating; order state desync; sensitive data stored locally                   | Region gating, happy/failure/timeout paths, webhook signature validation, order-state sync, security NFR (no card storage)     |
| **MUST**   | **US3409** — Replace xauth-X32     | Session token reuse; concurrent login instability; refresh rotation bugs; privilege/session hijack on macOS Tahoe | Session lifecycle matrix, forced logout post-deploy, login/logout/reset/multi-device regression, vulnerability scan (no X2499) |
| **MUST**   | **US4700** — Login in checkout     | Guest blocked after login prompt; cart loss on handoff; breaks PayU/shipping steps                                | UC1/UC2 acceptance criteria, cart continuity, handoff to payment and delivery selection                                        |
| **SHOULD** | **US5003** — ORMapper 2.0          | Migration data loss; wrong-field bug persists; perf AC miss on REF_ORG_Data; denormalisation errors               | Migration reconciliation, schema/ACCESS denorm checks, perf test on REF_ORG_Data (< 1.5 s), cross-module read/write smoke      |
| **COULD**  | **US3206** — MD5 hashed password   | Plaintext remnants; failed bulk migration; lockouts on login/reset                                                | All set/change paths + bulk conversion; flag MD5 weakness as follow-up defect                                                  |
| **WON'T**  | **US2300** — Czech language        | Wrong/missing translations erode trust                                                                            | Locale smoke checklist; no full UI translation matrix                                                                          |
| **WON'T**  | **US2350** — Czech product content | Wrong or missing product copy                                                                                     | Content QA sampling on selected PDPs                                                                                           |
| **WON'T**  | **US2450** — Delivery costs        | Incorrect shipping totals                                                                                         | Fold rule checks into checkout E2E samples only                                                                                |
| **WON'T**  | **US3150** — Zásilkovna delivery   | Pickup point selection failure; label/API integration errors                                                      | Sample pickup selection in CZ checkout journey                                                                                 |


**MoSCoW counts:** MUST 3 · SHOULD 1 · COULD 1 · WON'T 4 — satisfied.

---

## Reflection — prompting and model differences

**What worked:** Fixing MoSCoW quotas *before* asking the model to assign stories prevented "everything is MUST" flattening. Prompt A's dimension table produced auditable reasoning; Prompt B's journey framing surfaced Czech-market *hypothesis* links that pure risk scoring underweighted (e.g. US4700 competing with US5003 for the third MUST slot).

**Model differences observed:** Structured prompts yielded layer-specific test focus (contracts, session matrix, migration reconciliation); journey-weighted prompts ranked checkout stories higher and sometimes elevated US3150 or demoted US3206 because of title-level security wording. Neither prompt spontaneously combined PayU + login + Zásilkovna + Czech locale into one end-to-end Czech checkout scenario — that required human follow-up.

**What I would change next time:**

1. Add an explicit **"Czech golden path"** constraint: one E2E scenario must cover language → product → checkout → login → PayU → Zásilkovna, and ask which stories that path *forces* into MUST/SHOULD.
2. Ask models to **argue against** their own top pick for each MoSCoW slot (red-team step) before consolidating.
3. Include **US2450 delivery rules** as dependency context in Prompt B so models don't WON'T it without noting interaction with US3150/US3100 checkout totals.

