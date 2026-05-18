# Outcome — Risk-Based Test Planning (per `2-plan-identify-risks-transfer-task.md`)

**Sources:** `sprint6/sprint6-input/sprint6-content.md`, `sprint6/sprint6-input/sprint6-sprintGoal.md`, individual `US*.md` files in `sprint6/sprint6-input/`.  
**Note:** This execution was performed in a single analysis pass. Part 3 documents how results would be compared across models in a classroom or (RBI)(Chat)_GPT setting; no live second model was invoked in this workspace.

---

## Part 1 — Analyze the user stories

Sprint goal emphasizes **Czech market entry** (language, products, PayU) and **security foundation** (password handling, auth library). Assessment dimensions: business impact, technical complexity, integration risk, security relevance, production impact, user visibility, regression potential.

| ID | Business impact | Technical complexity | Integration risk | Security relevance | Production impact | User visibility | Regression potential |
|----|------------------|----------------------|------------------|---------------------|-------------------|-----------------|------------------------|
| **US2300** | High (trust, market) | Medium (i18n surface) | Low–medium (CMS/pipeline) | Low (mostly presentation) | Medium (wrong locale erodes trust) | Very high | Medium (all screens) |
| **US2350** | High (conversion) | Low–medium (content scope) | Low (catalog/content) | Low | Medium (wrong copy) | High on PDP | Low–medium |
| **US2450** | High (revenue, disputes) | Medium (rules, edge cases) | Medium (checkout, carriers) | Low | High (wrong totals) | High at checkout | High (pricing logic) |
| **US3100** | Very high (cannot monetize CZ without pay) | High (PSP, webhooks, regions) | Very high (PayU, order state) | Very high (PCI-style NFRs, no card storage) | Very high (double charge, stuck orders) | High | Very high (other PMs, regions) |
| **US3206** | High (breach/reputation) | Medium (hash everywhere) | Medium (all auth flows) | High (plaintext today; MD5 still weak) | High (lockouts, migration) | Medium | Very high (login, reset, register) |
| **US3409** | High (compliance narrative) | High (lib swap, sessions) | High (all auth/session consumers) | Very high (ISEC-2499, CVSS 8.1 class) | High (session bugs) | Low direct | Very high (login, refresh, multi-device) |
| **US4700** | High (abandoned checkout) | Medium (checkout state machine) | Medium (guest ↔ auth, cart) | Medium (session handoff) | High (funnel drop) | Very high | High (checkout paths) |
| **US5003** | High (wrong data downstream) | Very high (migration, schema) | Very high (all persistence) | Medium (vendor deprecation) | Very high (silent data corruption history) | Low direct | Very high (reads/writes, reports) |

**US2450** appears in the consolidated sprint list in `sprint6-content.md` but not in the shorter list at the bottom of `sprint6-sprintGoal.md`; it was still evaluated as in-scope content from `sprint6-input`.

---

## Part 2 — MoSCoW for **testing** focus (this sprint)

Target mix from task: **MUST 2**, **SHOULD 1**, **COULD 2**, **WON’T 3** (eight stories).

### Reasoning (summary)

- **MUST:** Reserve for flows where failure is **financial**, **security-critical**, or **systemic**. **US3100 (PayU)** and **US3409 (auth library / ISEC-2499)** dominate: money path plus authentication/session integrity across the whole shop.
- **SHOULD:** One slot for the next **widest blast radius** not fully covered by MUST: **US5003 (ORMapper)** — historical wrong-field persistence, migration, performance acceptance criterion → high integrity and regression risk.
- **COULD:** **US4700** (checkout funnel, visible but narrower than data layer) and **US3206** (many touchpoints but story specifies MD5; depth can be time-boxed after MUST auth/payment paths).
- **WON’T (for dedicated test cycles this sprint):** **US2300**, **US2350**, **US2450** — still smoke-tested in E2E and release checks, but **no** separate deep test campaigns vs. PayU/auth/migration in a capacity-constrained sprint. Czech copy and shipping rules are validated via **sampling** and **journey tests** rather than owning the same depth as MUST items.

---

## Part 3 — Compare different GenAI models (how you would use them)

This execution is one **risk-first** prioritization. In practice with **(RBI)(Chat)_GPT** and another model (e.g. Gemini, Claude, or a smaller local model), you would compare:

| Dimension | What to compare |
|-----------|-----------------|
| Risk assessment | Does the model overweight *security CVE text* vs *payment integration*? Does it flag MD5 as inadequate? |
| Prioritization | Do MUST/SHOULD slots differ (e.g. ORMapper vs PayU vs auth)? |
| Reasoning quality | Explicit trade-offs vs generic “test everything”; references to sprint goal. |
| Identified risks | Webhook replay, session fixation, migration rollback, locale gating — which are named? |
| Suggested testing focus | Concrete layers: contract tests for PayU, auth session matrix, data reconciliation for ORMapper. |

**Typical variance (illustrative, not from live second runs):**

- **Chat-style general model:** Often aligns PayU + auth as MUST; may rank **US4700** higher for “customer pain” wording in the story.
- **Smaller / faster models:** Tend to flatten risk (more stories in MUST) or omit **US5003** data-integrity nuance unless prompted with “wrong DB fields”.
- **Security-tuned or long-context model:** May elevate **US3206** (plaintext) toward SHOULD or argue **MD5** as unacceptable — useful for **quality of reasoning** review even if MoSCoW counts stay fixed.

**Process takeaway:** Fix the **MoSCoW quotas first**, then let models **argue which story fills each bucket**; disagreeements become your review agenda.

---

## Outcome — Prioritized user stories (MoSCoW table)

| Priority | User story | Testing focus (short) |
|----------|------------|------------------------|
| **MUST** | US3100 — PayU | Region gating, happy path, failure/timeout, callback/signature tampering, no local storage of sensitive data, sync with order state |
| **MUST** | US3409 — Replace xauth-X32 | Session lifecycle, concurrent login, refresh rotation, forced logout after deploy, regression on all auth entry points |
| **SHOULD** | US5003 — ORMapper 2.0 | Migration correctness vs V1.2 bug, schema/ACCESS denorm, perf AC on REF_ORG_Data, cross-module reads/writes |
| **COULD** | US4700 — Login in checkout | Guest vs logged-in UC1/UC2, cart continuity, handoff to payment/shipping |
| **COULD** | US3206 — MD5 hashed password | Register / profile / login change / forgot password + bulk conversion; note MD5 weakness for defect/tech-debt follow-up |
| **WON’T** | US2300 — Czech language | Checklist / spot checks; rely on E2E locale smoke rather than full UI matrix this sprint |
| **WON’T** | US2350 — Czech product content | Content QA sampling; not a dedicated test project vs MUST capacity |
| **WON’T** | US2450 — Delivery costs | Rule-based tests folded into checkout E2E samples; no standalone deep campaign this sprint |

**MoSCoW counts:** MUST 2, SHOULD 1, COULD 2, WON’T 3 — satisfied.
