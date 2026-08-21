---
id: prd-czech-market-expansion
status: refined
created: 2026-08-20
updated: 2026-08-20
phase: start
author: "unknown"
---

Input for start-prd: 
Q: What inputs do you have for this PRD?
A: Verbal description / idea
Analyzing your answers...

Q: Describe the feature or initiative you want to build. What is the goal, and what problem does it solve?
A: I have a web-shop to sell tools. I want to expand to the Czech market and want to find out if this makes sense?

# PRD: Czech Market Expansion — Feasibility & Pilot

## Problem / Opportunity

ToolShop currently serves customers in a single market and has no localization, local payment methods, or logistics integrations for Central European markets. A new investor with Czech origin has identified the Czech Republic as a high-potential growth market. The planned expansion to China was cancelled, creating an opportunity to redirect that investment toward a closer, more accessible market.

Without this initiative, ToolShop cannot serve Czech customers in their language, accept local payment methods (PayU, Czech Koruna), or deliver goods through Czech logistics providers (e.g., Zásilkovna). The platform's market potential in the Czech Republic is unknown and untested.

## Value Propositions

### Business Value

- Validates commercial viability of the Czech market before committing to a full rollout, reducing investment risk.
- Aligns with investor interest and potentially unlocks additional funding tied to Czech market entry.
- Establishes a repeatable localization pattern for future market expansions (e.g., Slovakia, Austria).
- Creates a competitive foothold in a growing e-commerce market with established local players (Alza.cz, Mall.cz).

### User Value

Czech customers can discover ToolShop products in their language, pay via a familiar local payment method, and receive deliveries through a trusted domestic carrier — removing friction that would otherwise prevent purchase completion.

## Goals

1. Deliver a structured go/no-go recommendation for Czech market entry, supported by market research, competitor analysis, and gap assessment.
2. Identify all localization, payment, logistics, and legal gaps that must be closed for a Czech market pilot.
3. Define the minimum set of product changes required for a Czech pilot launch (language, payment, delivery, product content).
4. Validate market demand through a time-boxed pilot with measurable success criteria.
5. Produce a prioritized implementation roadmap covering Czech language support, PayU payment integration, Czech product content, and Zásilkovna delivery option.

## Constraints

- Investor expectation: Czech market entry should be visible within the current product increment cycle.
- Technical: The webshop does not yet support multi-language content management, region-specific payment routing, or CZK currency handling.
- Legal: Czech e-commerce regulations (GDPR compliance, distance selling rules, VAT on digital services) must be assessed before launch.
- No Czech-speaking internal team members confirmed — translation and localization support may require external resources.

## Anti-Goals

- This PRD does not cover the full global localization framework — only Czech Republic.
- This PRD does not include the US market expansion (separate initiative, separate sprint).

## Solution

**Selected approach:** Staged Pilot — Phase 1 now, Phase 2 triggered by pilot success.

### Phase 1 — Czech Market Pilot Launch

Deliver a credible Czech market presence in a single sprint:

- **Czech language support** — language switcher across all customer-facing pages; translations provided by investor's Czech contacts (native quality, no agency cost).
- **Dual-currency support (EUR + CZK)** — customers can pay in either currency; CZK requires PayU Czech Republic integration (moved to Phase 1 because CZK support is meaningless without a CZK-capable payment gateway).
- **Zásilkovna delivery option** — Czech domestic carrier added alongside existing standard shipping; delivery cost rules applied per the defined pricing table.
- **Czech product content** — pilot-scope product catalogue translated to Czech (titles + descriptions); full catalogue expansion deferred to Phase 2.
- **Best-effort legal checklist** — internal GDPR / distance-selling checklist completed and documented; no external legal counsel before launch (risk accepted and logged).

### Phase 2 — Full Rollout (triggered on pilot success)

Activated only if the pilot reaches **200 orders from Czech delivery addresses within 60 days**:

- Full Czech product catalogue (all articles translated)
- Alza.cz marketplace onboarding and product listing
- Accounting / finance tooling aligned for CZK

### Scope

| | |
|---|---|
| **Reused** | Existing checkout flow, existing payment gateway (EUR path), existing delivery logic, existing product catalogue structure |
| **New work** | Language switcher + i18n framework, Czech translations (investor-supplied), PayU CZ API integration, CZK currency handling, Zásilkovna API integration, Czech product content for pilot catalogue |
| **Integration points** | PayU Czech Republic API, Zásilkovna API, existing checkout and order management |

### Rationale

- EUR + CZK from the start avoids a two-step payment migration and gives Czech customers their preferred payment experience immediately.
- Investor's Czech contacts eliminate translation cost and provide native quality — the only prerequisite is a defined handoff deadline.
- Alza.cz deferred to Phase 2 avoids marketplace onboarding complexity blocking Phase 1; direct webshop validation is faster and produces cleaner signal.
- Best-effort legal review is a deliberate, documented risk acceptance — faster than external counsel and appropriate for a time-boxed pilot.

**Estimated complexity:** M (Phase 1) + M (Phase 2 if triggered)

## Business Acceptance Criteria

**BAC-1 — Language Switcher Available on All Pages:**
Given a customer visits any page of the webshop,
When they look for a language selection control,
Then a visible language switcher is present and allows switching to Czech from any page without losing the current session context.

**BAC-2 — Full Czech UI After Language Switch:**
Given a customer has selected Czech as the display language,
When they navigate through the webshop (home, categories, product pages, cart, checkout, profile),
Then all navigation elements, buttons, labels, form fields, validation messages, and error messages are displayed in Czech.

**BAC-3 — Czech Product Content in Pilot Catalogue:**
Given a customer browses product pages in Czech language,
When a product is part of the pilot catalogue,
Then the product title and full product description are displayed in Czech.

**BAC-4 — EUR Payment Still Works for All Customers:**
Given any customer proceeds through checkout and selects EUR as currency,
When they complete payment with an existing payment method (e.g., credit card),
Then the order is processed and confirmed successfully in EUR without any change in existing behaviour.

**BAC-5 — CZK Currency Selection Available:**
Given a customer is on the checkout page,
When they select CZK as their payment currency,
Then the order total is displayed in Czech Koruna (CZK) using a correct EUR→CZK conversion, and the CZK amount is shown throughout the checkout and on the order confirmation.

**BAC-6 — PayU Available for CZK Checkout:**
Given a customer has selected CZK as currency during checkout,
When they proceed to payment method selection,
Then PayU (Czech Republic) is available as a payment method and successfully processes the CZK transaction.

**BAC-7 — PayU Payment Failure Handled Gracefully:**
Given a customer selects PayU and their payment fails or is cancelled,
When the payment gateway returns a failure or cancellation response,
Then the customer sees a clear error message in the webshop, the order is not created, and the customer can retry or choose a different payment method.

**BAC-8 — Zásilkovna Delivery Available for Czech Addresses:**
Given a customer enters a delivery address in the Czech Republic,
When they reach the shipping method selection step,
Then Zásilkovna appears as a delivery option alongside any existing options.

**BAC-9 — Correct Delivery Cost for Czech Orders:**
Given a customer with a Czech delivery address selects Zásilkovna or standard shipping,
When the order total and item weights are within defined thresholds,
Then the delivery cost is calculated and displayed correctly according to the defined pricing rules (free over €75, €5.95 under €75, €20 surcharge for oversized items, €12.95 for express).

**BAC-10 — Pilot Order Volume Tracked:**
Given the pilot has been live for any period,
When a team member checks order reporting,
Then orders with Czech delivery addresses are identifiable and counted separately so progress toward the 200-order threshold can be monitored at any time.

**BAC-11 — Phase 2 Go Decision:**
Given 60 days have elapsed since pilot launch,
When the order report is reviewed,
Then if 200 or more orders with Czech delivery addresses have been placed, a Phase 2 go-decision is formally recorded; if not, a no-go decision is recorded with documented reasoning.

## Dependencies

### Upstream

| Dependency | Owner | Blocker? |
|---|---|---|
| PayU Czech Republic merchant account | PayU CZ (external) | Yes — onboarding takes 2–4 weeks; must start immediately |
| Zásilkovna API access and credentials | Zásilkovna (external) | Yes — needed for Phase 1 delivery integration |
| Czech translations for UI strings and pilot catalogue | Investor's Czech contacts | Yes — hard deadline must be agreed before sprint start |
| EUR→CZK exchange rate source | TBD (internal or external FX API) | Yes — needed for CZK display |

### Downstream

| Impact | System | Action Required |
|---|---|---|
| Checkout flow must support currency selection | Webshop frontend | New CZK option and PayU routing logic |
| Order storage must record currency per order | Order management / database | Schema or field extension for currency field |
| Finance / accounting tooling | Finance team | Confirm CZK orders are handled correctly before launch |

## Risk Analysis

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| PayU merchant onboarding delays Phase 1 launch | High | High | Start PayU application in parallel with development; track as critical path item |
| Investor's contacts miss translation deadline | Medium | Medium | Define a hard handoff date at sprint start; fallback to machine translation (DeepL) for non-critical UI strings |
| No formal legal review exposes GDPR / distance-selling liability | Medium | High | Complete and document internal checklist; formally accept and log remaining risk with investor |
| CZK orders not handled by existing finance tooling | Medium | Medium | Confirm with finance team before launch; add to Definition of Done |
| 200-order threshold not reached — Phase 2 never triggered | Medium | Low | Pre-define the no-go path explicitly so stranded Phase 1 investment is avoided |
| Zásilkovna API integration proves more complex than estimated | Low | Medium | Time-box integration to 3 days; fallback to standard shipping only for Phase 1 if needed |

## Open Decisions

- [x] **Go/no-go threshold** — 200 orders from Czech delivery addresses within 60 days of pilot launch. Decision reviewed by product owner and investor.
- [x] **Currency strategy** — Both EUR and CZK in Phase 1. CZK requires PayU integration (moved to Phase 1).
- [x] **Marketplace partnership** — Alza.cz deferred to Phase 2; not in scope for the pilot.
- [x] **Translation ownership** — Investor's Czech contacts own all Czech translations; hard deadline to be agreed at sprint start.
- [x] **Legal review scope** — Best-effort internal checklist; no external legal counsel before pilot launch. Risk accepted and logged.

## References

- [Sprint 6 Content & User Stories](../../../sprint6-input/sprint6-content.md)
- [Sprint 6 Goal](../../../sprint6-input/sprint6-sprintGoal.md)
- [US2300 — Czech Language Support](../../../sprint6-input/US2300-support-czech-language.md)
- [US2350 — Czech Product Content](../../../sprint6-input/US2350-czech-products.md)
- [US3100 — PayU Payment Integration](../../../sprint6-input/US3100-support-payment-type-PayU.md)
- [US3150 — Delivery Options (Zásilkovna)](../../../sprint6-input/US3150-delivery-option.md)
- [US2450 — Delivery Costs](../../../sprint6-input/US2450-delivery-cost.md)
- [Practice Software Testing — ToolShop](https://practicesoftwaretesting.com/)

## Consistency Notes

Sprint 6 already contains concrete implementation user stories for Czech market features (US2300, US2350, US3100, US3150, US2450). This PRD serves as the **discovery and justification layer** that precedes that implementation work. The BACs defined here map directly to the acceptance criteria in those stories and should remain consistent as implementation progresses.

The Sprint 6 goal explicitly names Alza.cz marketplace integration — this is surfaced as an Open Decision because no user story covers it yet. If the marketplace partnership is confirmed, a separate story or PRD may be needed.
