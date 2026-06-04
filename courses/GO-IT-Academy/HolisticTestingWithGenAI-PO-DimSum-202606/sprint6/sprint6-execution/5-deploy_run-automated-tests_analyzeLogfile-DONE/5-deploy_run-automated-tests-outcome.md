# ToolShop automation log review — submission

## Log facts (manual skim + aggregation)

- **20** snapshot files: `ta-20260513*.log` (08:00–11:32 UTC on 2026-05-13).
- **Line format (fact):** `spec::case | STATUS | ISO timestamp | message` — statuses include `PASS`, `FAIL`, `FLAKY_PASS`, `SKIP`.
- **Volume (fact, all lines):** ~1,984 `FAIL`, ~984 `PASS`, ~770 `FLAKY_PASS`, ~250 `SKIP` (~3,988 lines total).
- **Inference:** The same failure text appears across unrelated specs (e2e, component, API, unit), and many cases flip `PASS` ↔ `FAIL` ↔ `FLAKY_PASS` within one run — strong signal of **test-layer instability and shared brittle patterns**, not twenty independent product bugs.

---

## Prompt A (structured test-quality audit)

*Provider/style: analytical, table-first; excerpts were top recurring `FAIL` lines plus one full run sample (`ta-20260513082400.log`, first 40 lines).*

```
You are a senior test engineer reviewing ToolShop Playwright/Jest pipeline logs.
Do NOT assume the application is broken. Focus on test-suite quality: locators, waits, data, isolation, diagnostics, and CI strategy.

Context: ToolShop e-commerce staging. Log line format:
  path::testName | STATUS | timestamp | message

Attached excerpts show recurring failures, e.g.:
- getByTestId('mini-cart-toggle') not found — attribute renamed to data-qa='mini-cart'
- waiting for locator('.product-grid .tile:nth-child(1) .price') failed: DOM order unstable
- Locator 'text=Proceed to checkout' strict mode: matched banner CTA and drawer CTA
- Intermittent: race — networkidle fired before hydration; element reflowed
- FLAKY_PASS: Passed after retry; first attempt: ...

Tasks:
1) Cluster failure messages into themes (locator contract, timing, test data, environment, API contract, pipeline).
2) For each theme, state what is FACT vs INFERENCE.
3) Output a markdown table with columns: Priority (P1–P4), Observation, Suspected test-layer cause, Concrete next step.
4) End with a 3-item sprint sequencing recommendation.

Rules: No product bug triage unless the log explicitly proves it. Prefer one shared fix over per-spec band-aids when the same message hits many files.
```

---

## Prompt B (alternate model/style — failure taxonomy + blast radius)

*Provider/style: concise checklist, rank by cross-file frequency; no long prompt reuse.*

```
ToolShop CI logs — classify ONLY test-harness problems.

Input: grep-style counts from 20 runs (FAIL lines):
- 154× page.goto timeout (staging catalog)
- 151× aria-label Search products → 0 nodes
- 145× spinner wait 45s
- 137× waitForResponse POST /api/cart/merge
- 125× btn-add-cart-legacy missing
- 118× nth-child(1) price DOM unstable
- 115× mini-cart-toggle → data-qa rename
- 770× FLAKY_PASS rows in same period

Return:
A) Top 5 "blast radius" issues (most files affected)
B) For each: one-line fix, owner hint (e2e / api / platform), effort S/M/L
C) False-positive risks if we misread these as app defects
D) One pipeline policy change (retries, shards, flags)

Format: numbered list, max 400 words.
```

---

## Consolidated improvement backlog (sprint-ready)

| Priority | Observation (from logs) | Suspected root cause (test layer) | Concrete next step |
|----------|---------------------------|-----------------------------------|---------------------|
| P1 | **115×** `getByTestId('mini-cart-toggle')` → `data-qa='mini-cart'` across checkout, cart, shipping, API-adjacent specs | Hard-coded `data-testid` contract; no shared locator map | Add `locators/miniCart.ts` using `data-qa`; replace direct `getByTestId` calls; add 1-line smoke asserting required `data-qa` on staging |
| P1 | **118×** `.product-grid .tile:nth-child(1) .price` — DOM order unstable | Assertion tied to **sort order**, not product identity | Select tile by **slug/SKU** or `data-qa` on card; seed fixture product `TS-HAMMER-001` and assert its price |
| P1 | **123×** strict mode on `text=Proceed to checkout` + **41×** duplicate `Add to cart` role | Ambiguous text/role locators across banner vs drawer | Scope with region (`getByTestId('mini-cart-drawer')`) or unique `data-qa` per CTA; use `.filter({ hasNot: … })` where needed |
| P2 | **125×** `btn-add-cart-legacy` + **124×** `#old-checkout-submit` after US-Checkout refresh | Tests on **deprecated checkout** branch | Gate with `checkout_v2` flag helper; delete legacy selectors or maintain versioned locator factory |
| P2 | **137×** `waitForResponse` POST `/api/cart/merge` timeout | Brittle predicate or merge not on path under test | Match **method + pathname** only; or assert cart state via API/UI after known user action |
| P2 | **47×** `networkidle` before hydration + **41×** `Target closed (page navigated)` + **770** `FLAKY_PASS` rows | Racey sync; retries mask instability | Drop `networkidle`; wait for skeleton gone / landmark visible; **fail build on retry-only pass** |
| P2 | **56×** detached DOM after category chip click | Stale locator after list re-render | Re-query after filter; `await expect(chip).toBeHidden()` then click new locator |
| P3 | **151×** `[aria-label="Search products"]` → 0 nodes | Icon-only search; label removed | Standardize `data-qa="search-open"`; tests use QA hook first, role second |
| P3 | **144×** lazy image not in viewport | Visibility assert on decorative lazy load | Assert product title/stock badge, or `scrollIntoViewIfNeeded()` only if AC requires in-view image |
| P3 | **145×** spinner 45s + **154×** `page.goto` 60s on staging catalog | Infra slowness folded into functional suite | Tag `@slow` / separate job; pre-flight staging health; don’t raise all timeouts globally |
| P3 | Promo/SKU/region messages (`SPRING2024`, `EU-CENTRAL`, EUR vs CZK, ZIP surcharge) | **Fixture drift** vs staging rules | Idempotent seed step in pipeline; central `testData/shipping.ts` aligned with tariff tables |
| P3 | **38×** `toolshop_ci_expired@example.com` 401 | Expired CI credential in fixtures | Rotate secret; add auth preflight job that fails with actionable message |
| P4 | **49×** POST `/v1/order` vs `/v2/orders` | API client not migrated in tests | Update clients; single OpenAPI contract test in CI |
| P4 | **250×** `SKIP` — `checkout_v2 disabled on this shard` + **40×** retry-only pass on shard 3/4 | Uneven coverage; shards hide flakiness | Flag-matrix job: run checkout_v2 suite only where flag enabled; treat shard retry pass as failure in merge queue |

**Suggested sequencing:** P1 locator contract and nth-child (largest cross-cutting volume) → P2 timing/checkout/API waits → P3 data and environment → P4 pipeline policy.

---

## Reflection (Prompt A vs Prompt B)

| Dimension | Prompt A (structured audit) | Prompt B (taxonomy + blast radius) |
|-----------|----------------------------|-------------------------------------|
| **Depth** | Deeper on *why* (themes, fact vs inference, per-row remediation) | Faster on *what hurts most* (frequency-ranked) |
| **False positives** | Lower risk of blaming the app; explicitly scoped to test layer | Counts alone could over-prioritize infra timeouts unless reader remembers staging context |
| **Actionability** | Best for backlog rows developers can pick up | Best for sprint planning and owner assignment (e2e vs platform) |
| **Format** | Table ready for Jira/Linear import | Short narrative; needs manual merge into backlog |

**Next time:** Run **B first** for priority order, then **A** on the top three themes only (with 30-line excerpts, not whole files). Ask both prompts to cite **message substring + approximate count** to keep outputs tied to log evidence. I would also add an explicit rule: “treat `FLAKY_PASS` as failed for reliability metrics.”
