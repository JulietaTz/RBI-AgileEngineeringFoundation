---
id: prd-sprint7-delivery-overhaul
status: refined
created: 2026-08-20
updated: 2026-08-20
phase: refine
author: "unknown"
---

# PRD: Sprint 7 Delivery Overhaul — Value-Based Pricing and Zásilkovna Option

## Problem / Opportunity

The ToolShop checkout currently applies a weight-and-region-based delivery pricing model
introduced in Sprint 6 (US4200: flat €7.90 base → weight tiers, CZ/EU/US/Others regions).
This model does not reflect the desired business rules: an order-value threshold for free
shipping, a surcharge for oversized items, and a next-day express option.

Additionally, Zásilkovna was introduced in Sprint 6 as a delivery option (CZ + DACH regions
only), but its pricing and availability under the new Sprint 7 model has not been specified.

Customers currently cannot benefit from a free shipping incentive tied to order value, and
the express delivery option does not exist in the system.

## Value Propositions

### User Value
Customers see transparent, predictable delivery costs before confirming an order. A clear
free-shipping threshold (€75) incentivises larger baskets. Express next-day delivery is
available for eligible items.

### Business Value
A free-shipping threshold above €75 is a proven mechanism for increasing average order
value. Replacing a complex weight-and-region matrix with a simpler value-based model
reduces customer confusion and potential support requests.

### Technical Value
Delivery cost rules become configurable without code changes (per US2450 non-functional
requirement), improving operational maintainability.

## Goals

- Replace the Sprint 6 weight+region-based delivery pricing with a value-based model:
  - Free standard shipping for orders above €75 (oversized items excluded from threshold)
  - Flat rate €5.95 for orders below or equal to €75
  - +€20 surcharge for any single item over 25 kg or with large dimensions
  - Express next-day delivery at €12.95 (in-stock items only)
- Introduce Zásilkovna as a selectable delivery option alongside Standard delivery
- Delivery cost shown and recalculated dynamically during checkout before order confirmation
- Order summary displays product cost, delivery cost, and total separately
- Delivery cost calculation completes within 2 seconds
- Delivery cost rules are maintainable without code changes

## Constraints

- Must integrate with the existing checkout workflow (multi-step, Angular frontend)
- Currency display convention from Sprint 6 (CZK for CZ billing country, USD otherwise)
  must be honoured — or explicitly superseded (see Open Decisions)
- Express delivery must be gated on real-time stock status
- Unsupported delivery destinations must show a user-facing error

## Anti-Goals

- No changes to payment methods (PayU, Mock PayU service remain unchanged)
- No changes to Czech language localisation or Czech product content
- No rework of Sprint 6 security hardening stories

## Solution

### Selected approach: JSON config in DB (Option B)

A dedicated delivery rules JSON document is stored in the existing `settings` table (or
equivalent config store). A new Laravel `DeliveryCalculatorService` reads and Redis-caches
this document. Angular calls `POST /delivery/calculate` on the delivery step; the backend
applies the full rule pipeline and returns the list of eligible delivery options with costs.

**Scope**

| Scope | Detail |
|---|---|
| **Reused** | Checkout step 3 (Delivery) page and route; existing `settings`/config store; Redis cache; product `weight` field |
| **New work** | `DeliveryCalculatorService` (rule engine); `POST /delivery/calculate` endpoint; delivery JSON config schema + seeding; admin `PUT /admin/delivery-config` endpoint; Angular reactive binding of delivery cost to cart state changes |
| **Integration points** | Product catalogue (weight per item); stock service (in-stock gate for Express); checkout session (cart items, shipping address, billing country); order service (persist chosen method + cost); invoice rendering (itemised summary) |

**Rationale**

- Satisfies the "configurable without code deployment" NFR without the relational complexity of
  full delivery rule tables, keeping Sprint 7 scope contained.
- Redis caching means no performance penalty for the 2-second calculation constraint.
- JSON config is admin-editable via a single PUT endpoint; no DB migration required for price
  changes.

**Estimated complexity:** M

---

## Business Acceptance Criteria

**BAC-1 — Free shipping for Standard above €75 (non-oversized):**
Given a cart whose total, counting only non-oversized items, exceeds €75,
When the customer selects Standard delivery,
Then the delivery cost displayed is €0.00 and the order summary shows "Free Shipping".

**BAC-2 — Flat rate for Standard at or below €75:**
Given a cart whose non-oversized item total is €75 or less,
When the customer selects Standard delivery,
Then the delivery cost displayed is €5.95.

**BAC-3 — Oversized surcharge applied on top of base delivery cost:**
Given a cart containing at least one item weighing more than 25 kg,
When the customer views delivery options,
Then a €20 surcharge is shown as a separate line item in the order summary, added on top
of the applicable Standard or Zásilkovna base delivery cost.

**BAC-4 — Oversized item value excluded from free shipping threshold:**
Given a cart containing one oversized item (>25 kg) worth €80 and no other items,
When the customer reaches the delivery step,
Then the non-oversized total is €0 (below €75 threshold), Standard base delivery is €5.95,
and the €20 oversized surcharge is shown separately — total delivery cost: €25.95.

**BAC-5 — Mixed cart: free shipping on non-oversized portion with surcharge:**
Given a cart containing €80 of non-oversized items plus one oversized item (>25 kg),
When the customer reaches the delivery step,
Then Standard delivery for the non-oversized portion is €0.00 (free) and the €20 oversized
surcharge is shown separately — total delivery cost: €20.00.

**BAC-6 — Express available when all items in stock and none oversized:**
Given all cart items are in stock and no item weighs more than 25 kg,
When the customer views delivery options,
Then Express next-day delivery is shown at €12.95 as a selectable option.

**BAC-7 — Express not available when any item is out of stock:**
Given the cart contains at least one item that is out of stock,
When the customer views delivery options,
Then Express is not shown, or is shown as disabled with a message explaining the reason.

**BAC-8 — Express not available when any item is oversized:**
Given the cart contains at least one item weighing more than 25 kg,
When the customer views delivery options,
Then Express is not shown, or is shown as disabled — regardless of stock status.

**BAC-9 — Zásilkovna available for CZ and DACH shipping addresses:**
Given the customer enters a shipping address in Czech Republic, Germany, Austria, or Switzerland,
When the customer reaches the delivery step,
Then Zásilkovna appears as a selectable delivery option alongside Standard.

**BAC-10 — Zásilkovna not available outside CZ and DACH:**
Given the customer enters a shipping address in any country other than CZ, DE, AT, or CH,
When the customer reaches the delivery step,
Then Zásilkovna is not shown; only Standard delivery is offered.

**BAC-11 — Zásilkovna uses Standard pricing rules:**
Given the customer selects Zásilkovna with a qualifying CZ or DACH shipping address,
When the order summary is displayed,
Then the delivery cost follows the same value-based rules as Standard (free above €75
non-oversized total, €5.95 below, +€20 oversized surcharge). Express is not available
for Zásilkovna.

**BAC-12 — Order summary shows itemised delivery costs:**
Given the customer has selected a delivery method,
When the order summary is displayed at the delivery and confirmation steps,
Then product subtotal, base delivery cost, oversized surcharge (if applicable), and order
total are shown as separate line items.

**BAC-13 — Delivery cost updates when cart changes:**
Given a customer is on the delivery step with a delivery cost displayed,
When the customer navigates back and modifies the cart (adds or removes an item),
Then upon returning to the delivery step the displayed delivery cost reflects the updated
cart — without a full page reload.

**BAC-14 — Delivery config updatable without code deployment:**
Given the shop administrator updates the delivery pricing configuration via the admin API,
When a customer begins a new checkout session after the update,
Then the updated prices and rules apply — no code release or deployment is required.

**BAC-15 — Unsupported shipping destination handled gracefully:**
Given the customer enters a shipping address in a destination not served by any delivery method,
When the customer reaches the delivery step,
Then a clear explanatory message is displayed and the customer cannot advance to payment.

---

## Dependencies

### Upstream (must exist before implementation)

| Dependency | Owner | Status |
|---|---|---|
| Product `weight` field populated for all catalogue items | Product / Data team | Exists in DB; data completeness unknown |
| Stock status API (in-stock gate for Express) | Inventory service | Exists (used in checkout); real-time read required |
| Checkout session carries item weights, values, and stock flags | Checkout module | Must be verified / extended if not already passed |

### Downstream (affected by this change)

| Affected area | Change required |
|---|---|
| Order service | Must persist chosen delivery method identifier and final delivery cost with the order record |
| Invoice / order confirmation page | Must render itemised delivery breakdown (base + surcharge as separate lines) |
| Admin panel | New admin API endpoint `PUT /admin/delivery-config` — admin frontend story may be separate |

### Cross-repo / deployment dependencies

No external repository dependencies. Delivery config JSON seeding is a DB migration run at
deploy time and does not require changes outside the main ToolShop repository.

---

## Risk Analysis

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Weight data missing or incorrect for some products | H | M | Audit product catalogue weights before cutover; treat missing weight as "unknown — assume oversized" for delivery surcharge |
| Stock status stale at delivery step | M | M | Cache stock with short TTL (≤30 s); display a stale-data indicator if cache is older than threshold |
| JSON config corruption by admin error | L | H | Validate config JSON schema on `PUT /admin/delivery-config` before persisting; maintain a versioned config history (last N snapshots) |
| Edge-case combinations missed in testing | M | M | Explicitly test: in-stock + oversized (no Express), mixed stock (no Express), exactly €75 cart total (flat rate, not free), €75 threshold with mixed normal + oversized items |
| Currency UX regression for CZ customers | L | L | Sprint 6 displayed CZK; Sprint 7 uses EUR exclusively — communicate this change clearly in release notes and on the checkout UI |
| Express + oversized logic ordering | M | M | Ensure both gates (stock AND weight) are applied independently; neither gate alone must be sufficient to enable Express |

---

## Open Decisions

- [x] **Currency denomination for Sprint 7 pricing** — Decision: EUR as the single display
  currency. Sprint 6 CZK/USD convention is superseded for delivery pricing in Sprint 7.

- [x] **Zásilkovna pricing under the new model** — Decision: Same pricing rules as Standard
  (free above €75 non-oversized total, €5.95 flat rate, +€20 oversized surcharge). Express
  is not available for Zásilkovna.

- [x] **Zásilkovna regional availability** — Decision: Carry forward Sprint 6 restriction:
  CZ and DACH (DE, AT, CH) only.

- [x] **Oversized item definition and data source** — Decision: Oversized is derived purely
  from product weight: any item >25 kg is oversized. The "large dimensions" clause in US2450
  is dropped; no new dimension fields are required.

- [x] **Express delivery and oversized items** — Decision: Express is blocked for oversized
  items (>25 kg). Both gates (stock AND weight) must pass for Express to be available.

- [x] **Delivery cost configurability mechanism** — Decision: JSON config document in the
  existing DB settings store, Redis-cached, updatable via admin API without code deployment.

## Open Decisions

- [ ] **Currency denomination for Sprint 7 pricing** — US2450 prices are in EUR (€75, €5.95,
  €20, €12.95). Sprint 6 established CZK display for CZ billing addresses and USD otherwise
  (US4200). Sprint 7 does not specify multi-currency behaviour. Should Sprint 7 prices be
  EUR-denominated exclusively, or should they be converted to CZK/USD following the Sprint 6
  convention? This impacts display logic and pricing tables.

- [ ] **Zásilkovna pricing under the new model** — US4200 (Sprint 6) defined separate
  weight-tier pricing for Zásilkovna (e.g. CZ Light: $1.99 / 49 Kč). US2450 does not mention
  Zásilkovna pricing. Does Zásilkovna use the same value-based pricing rules as Standard, or
  does it have its own rate card? If different, what are the rates?

- [ ] **Zásilkovna regional availability** — Sprint 6 restricted Zásilkovna to CZ and DACH
  (DE, AT, CH). Does this restriction carry forward into Sprint 7, or does availability
  change? US3150 does not specify.

- [ ] **Oversized item definition and data source** — US2450 defines oversized as ">25 kg or
  large dimensions". Where does this attribute live in the product data model? Is "large
  dimensions" already a structured field, or does it need to be introduced?

- [ ] **Express delivery and oversized items** — US2450 specifies express is available for
  in-stock items only. It does not address whether express is available for oversized items.
  Should express be blocked for oversized, or is the only gate stock status?

- [ ] **Delivery cost configurability mechanism** — US2450 requires delivery cost rules to be
  maintainable without code changes. What is the configuration surface: database table, config
  file, admin UI, or environment variable? This impacts implementation scope.

## References

- [US2450 — Delivery Costs Calculation](courses/GO-IT-Academy/HolisticTestingWithGenAI-ScrumMasterCommunity-20261012/sprint7-delivery-feature/US2450-delivery-cost.md)
- [US3150 — New Delivery Option (Zásilkovna)](courses/GO-IT-Academy/HolisticTestingWithGenAI-ScrumMasterCommunity-20261012/sprint7-delivery-feature/US3150-delivery-option.md)
- [US4200 — Sprint 6 Delivery Options (superseded)](systemsUnderTest/toolShop/backlog-V6CZ/US4200-delivery-costs.md)
- [Sprint 6 Agile Vision Statement](systemsUnderTest/toolShop/backlog-V6CZ/sprint6-agile-vision-statement.md)
- [ToolShop Practice App](https://practicesoftwaretesting.com/)
- [ToolShop GitHub Repository](https://github.com/testsmith-io/practice-software-testing)
- [ToolShop API Documentation](https://github.com/testsmith-io/practice-software-testing/tree/main/docs)

## Consistency Notes

**Conflict — pricing model supersession:** US4200 (Sprint 6) defined weight-and-region-based
delivery pricing with Zásilkovna for CZ+DACH. US2450 (Sprint 7) introduces a fundamentally
different value-based model. The user confirmed US2450 supersedes US4200. This means the
Sprint 6 delivery pricing implementation will be replaced, not extended.

**Currency gap:** US2450 prices are stated in EUR. The Sprint 6 model displayed prices in
CZK (CZ billing) or USD (all others). Whether Sprint 7 adds EUR as a third currency or
converts the new thresholds into existing currency denominations is unresolved — captured as
an open decision.

**US3150 underspecified:** The story contains only three words ("New delivery option /
Standards / Zasilkovna"). It appears to define the available delivery method names but
provides no pricing, regional availability, or functional rules for Zásilkovna under the
Sprint 7 model. Multiple open decisions stem from this gap.
