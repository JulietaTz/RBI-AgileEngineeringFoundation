---
id: prd-czech-market-expansion-sprint6
status: refined
created: 2026-08-20
updated: 2026-08-20
phase: refine
author: "unknown"
---

# PRD: Czech Market Expansion — Sprint 6

## Problem / Opportunity

After the cancellation of the planned China expansion, the Czech Republic has been identified as an alternative growth market for the ToolShop webshop. Currently the webshop does not support Czech language, Czech product content, local Czech payment methods, or Czech-specific logistics options. Czech customers cannot complete a purchase in their native language or use payment and delivery services they are familiar with. This limits market acceptance and prevents establishing a commercial presence in Czechia.

## Value Propositions

### User Value

Czech customers can browse the webshop in their native language, read product information in Czech, pay via PayU (the dominant local payment provider), and choose delivery options including Zásilkovna — a widely used Czech parcel service. This reduces friction and builds purchase confidence.

### Business Value

Entering the Czech market validates the growth strategy replacing the cancelled China expansion. Enabling local language, payment, and logistics lowers the barriers to purchase completion, supporting revenue generation and market penetration in the Czech Republic.

## Goals

- Czech customers can complete the full purchase journey (browse → product detail → cart → checkout → payment confirmation) in the Czech language.
- PayU is available as a payment method for customers with a Czech billing or shipping address.
- Zásilkovna and Standard delivery are selectable delivery options for Czech customers.
- Delivery costs are calculated correctly and displayed during checkout based on the defined pricing rules.
- Czech product content is displayed for supported articles when the Czech language is active.

## Constraints

- PayU Czech Republic integration requires a configured merchant account and API credentials from PayU.
- PayU is geographically restricted: only displayed for Czech billing or shipping addresses.
- Delivery cost rules are defined in euros (€); CZK conversion or dual-currency handling is not yet scoped.
- Free shipping threshold (€75) and surcharge rules must be configurable without code changes (per US2450 non-functional requirement).
- Express delivery is limited to in-stock items only.
- Czech product content is provided for a defined subset of articles; not all products require Czech content in this sprint.

## Anti-Goals

- Admin or back-office interface translation into Czech is out of scope.
- Expanding to additional Central/Eastern European markets is out of scope for this sprint.
- Full CZK currency display across the entire webshop is out of scope (only addressed in PayU integration where PayU supports CZK).

## Solution

**Approach:** Layered separation — Transloco for static UI, API locale parameter for dynamic content.

Angular's existing **Transloco** i18n library handles all static UI text (navigation, labels, buttons, headings, form validation messages, error messages). A new Czech translation file (`cs.json`) is added to the Angular app. **Dynamic product content** (titles, descriptions, specifications) is fetched from the Laravel API with a `lang` query parameter; Czech content is stored per article in the database. A shared `LanguageService` in Angular stores the selected locale, drives Transloco, and injects the `lang` parameter into all product API calls. Language preference is persisted via a browser cookie (no login required). Delivery costs are calculated **server-side** in Laravel using the configurable weight-tier × region × billing-currency pricing model (per US4200). PayU is integrated as `payment_method: "payu-cz"` through the existing mock PayU service infrastructure (US9100) already architected in the backend.

**Scope:**
- **Reused:** Transloco i18n setup, existing PaymentController + mock PayU service (US9100), existing checkout flow, existing product API endpoints (extended with `lang` param)
- **New work:** `cs.json` Czech translation file, `LanguageService` with cookie persistence, Czech product content in DB, locale-aware product API responses, delivery calculation endpoint (weight + region + billing country), Zásilkovna delivery option, PayU payment method registered in checkout, language switcher UI component
- **Integration points:** Transloco service (language switcher), Product API (locale param), Checkout/Delivery API (delivery options + costs), PaymentController (`payment_method: "payu-cz"`)

**External contracts:**
- Mock PayU API contract: `POST /mock/payu/orders` with `{ amount, currency, order_id }`, responses defined in US9100
- Checkout payment contract: `POST /payment/check` → `200 { "message" }` on success, `4xx { "error" }` on failure (defined in US9100)
- Delivery API: new endpoint returning `{ delivery_method, price, currency }` per option, based on shipping region + cart weight + billing country

**Rationale:**
- Transloco is already in the app — extending it for Czech avoids introducing a new i18n system
- Delivery rules and pricing remain configurable server-side without frontend deployments (satisfies US4200 / US2450 NFR)
- Mock PayU is already architected (US9100); using it avoids real payment credentials in training/test environments

**Estimated complexity:** M

**Delivery cost specification:** US4200 (backlog-V6CZ) is the authoritative spec, superseding US2450 (sprint input). Weight-based pricing (Light < 10 kg / Heavy ≥ 10 kg), CZK for CZ billing, USD for all others. Express option and value-based threshold from US2450 are not implemented.

---

## Business Acceptance Criteria

### Language Support (US2300)

**BAC-1 — Language switcher present on all pages:**
Given a customer visits any customer-facing page (home, product listing, product detail, checkout, profile),
When the page renders,
Then a visible language switcher is displayed that includes Czech as a selectable option.

**BAC-2 — Switching to Czech activates Czech UI immediately:**
Given a customer is on any page of the webshop,
When they select Czech in the language switcher,
Then all navigation elements, headings, UI labels, buttons, and menu items switch to Czech immediately, without a full page reload.

**BAC-3 — Czech language persists across sessions:**
Given a customer has selected Czech in the language switcher,
When they close and reopen the webshop in the same browser on the same device,
Then Czech is still the active language without the customer needing to select it again.

**BAC-4 — Validation and error messages in Czech:**
Given a customer has set the webshop language to Czech,
When they submit a form with invalid or incomplete data (during registration, login, profile update, or checkout),
Then all validation error messages and system error messages are displayed in Czech.

**BAC-5 — Full checkout flow in Czech:**
Given a customer has set the webshop language to Czech,
When they proceed through each checkout step (cart, billing address, payment method, order confirmation),
Then all page labels, field names, button text, step indicators, and status messages across all checkout steps are displayed in Czech.

### Czech Product Content (US2350)

**BAC-6 — Czech product title and description on listing and detail pages:**
Given the webshop language is set to Czech and a customer views a product for which Czech content exists,
When the product listing page or product detail page is displayed,
Then the product title and product description are shown in Czech.

**BAC-7 — Czech product content in cart and checkout order summary:**
Given the webshop language is set to Czech and a customer has added a product with Czech content to the cart,
When they view the cart or the checkout order summary,
Then the product title is displayed in Czech.

**BAC-8 — English fallback for products without Czech content:**
Given the webshop language is set to Czech and a customer views a product for which no Czech translation exists,
When the product is displayed on any page (listing, detail, cart, checkout),
Then the product title and description are shown in English, with no empty or blank content fields.

**BAC-9 — Czech special characters render correctly:**
Given the webshop language is set to Czech,
When any page displays content containing Czech-specific characters (e.g., á, č, ě, í, ř, š, ů, ž),
Then all Czech characters are rendered correctly without encoding errors, replacement characters, or garbled text.

**BAC-10 — Search results display Czech content:**
Given the webshop language is set to Czech,
When a customer searches for a product and the results include articles with Czech content,
Then the search results list displays Czech product titles and descriptions for those articles.

### Delivery Options and Costs (US3150 / US4200 — supersedes US2450)

**BAC-11 — Standard delivery available for all shipping destinations:**
Given a customer with any shipping address is on the delivery selection step in checkout,
When delivery options are displayed,
Then Standard delivery is listed as a selectable option, regardless of shipping country.

**BAC-12 — Zásilkovna available for CZ and DACH shipping destinations:**
Given a customer with a shipping address in Czech Republic, Germany, Austria, or Switzerland is on the delivery selection step,
When delivery options are displayed,
Then Zásilkovna is listed as a selectable option alongside Standard delivery.

**BAC-13 — Zásilkovna not available outside CZ and DACH:**
Given a customer with a shipping address in any country other than Czech Republic, Germany, Austria, or Switzerland is on the delivery selection step,
When delivery options are displayed,
Then Zásilkovna is NOT shown; only Standard delivery is available.

**BAC-14 — Light weight tier price applied:**
Given a customer's cart has a total item weight below 10 kg and they select a delivery method,
When the delivery cost is calculated and the order summary is displayed,
Then the delivery cost matches the Light weight tier price in the pricing matrix for the selected delivery method and shipping region.

**BAC-15 — Heavy weight tier price applied:**
Given a customer's cart has a total item weight of 10 kg or more and they select a delivery method,
When the delivery cost is calculated and the order summary is displayed,
Then the delivery cost matches the Heavy weight tier price in the pricing matrix for the selected delivery method and shipping region.

**BAC-16 — Delivery prices displayed in CZK for Czech billing address:**
Given a customer has a Czech Republic billing address,
When they view delivery options and the order summary,
Then all delivery prices are displayed in Czech Koruna (Kč).

**BAC-17 — Delivery prices displayed in USD for non-Czech billing address:**
Given a customer has a billing address in any country other than Czech Republic,
When they view delivery options and the order summary,
Then all delivery prices are displayed in US Dollars ($).

**BAC-18 — Delivery cost shown separately before order confirmation:**
Given a customer has selected a delivery method and is reviewing the order,
When the final order summary is displayed before the customer confirms the purchase,
Then the product subtotal, the delivery cost, and the total order amount are shown as separate line items.

### PayU Payment Integration (US3100)

**BAC-19 — PayU listed for customers with Czech billing or shipping address:**
Given a customer whose billing address OR shipping address is in Czech Republic has reached the payment selection step,
When the list of available payment methods is displayed,
Then PayU is listed as a selectable payment method.

**BAC-20 — PayU not listed for fully non-Czech customers:**
Given a customer whose billing address AND shipping address are both outside Czech Republic has reached the payment selection step,
When the list of available payment methods is displayed,
Then PayU is NOT listed among the available payment methods.

**BAC-21 — Successful PayU payment creates the order:**
Given a Czech customer has selected PayU and confirmed the payment,
When the PayU payment is processed successfully,
Then a payment success message is displayed to the customer, the order is created in the webshop, and the customer can proceed to finalise the purchase.

**BAC-22 — Failed or declined PayU payment allows retry:**
Given a customer has selected PayU and confirmed the payment,
When the payment is declined or fails (e.g., insufficient funds, service unavailable),
Then a clear error message is displayed, the cart contents and all previously entered checkout data are preserved, and the customer can attempt the payment again without restarting the checkout flow.

---

## Dependencies

### Upstream (must exist before this feature can be completed)

- **US9100 — Mock PayU Service** (hard prerequisite): The embedded mock PayU service in the Laravel backend must be implemented before PayU checkout flow can be developed or tested. US9100 defines the full API contract (`POST /mock/payu/orders`, scenario-based responses). US3100 has a direct implementation dependency on US9100.
- **Czech product content** (data prerequisite): Czech translations for product titles, descriptions, and specifications must be authored and loaded into the database before Czech product content features can be verified.
- **US4200 delivery pricing matrix** (configuration prerequisite): The complete pricing matrix (Standard + Zásilkovna, all regions, both weight tiers, CZK and USD) must be finalized and available as configuration before the delivery cost calculation endpoint can be implemented.

### Downstream (affected by this change)

- **Existing checkout E2E tests**: New delivery options (Zásilkovna) and the PayU payment method will require updates to any automated tests that verify the list of delivery methods or payment methods available during checkout.
- **Order processing**: Delivery method and pricing data will now be part of each order record; existing order management logic must handle the new delivery option codes (`standard`, `zasilkova`).

### Cross-repo / Deployment

- **Environment variable**: A `PAYU_API_URL` (or equivalent) environment variable must be configured in all environments, pointing to the embedded mock service for non-production environments.
- **Czech translation file deployment**: `cs.json` must be included in the Angular build artifact; deployment pipeline must not strip translation assets.
- **Delivery pricing configuration**: The pricing matrix must be deployable as a configuration file or database seed, separately from application code, to support updates without redeployment.
- **Language cookie**: Legal review required to confirm whether a language preference cookie qualifies as a functional cookie (typically exempt from GDPR consent) or requires user consent.

---

## Risk Analysis

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| US9100 (Mock PayU) not completed before US3100 sprint integration | H | H | Sequence US9100 before US3100 in sprint planning; do not start US3100 implementation until US9100 is merged |
| Incomplete Czech product content at launch | M | M | Define minimum viable content scope (e.g., top Czech market products); English fallback ensures no blank UI fields |
| Language state desync between Transloco and API locale parameter | M | M | Implement a single shared `LanguageService`; mandate its use for both Transloco switching and API `lang` param injection |
| Zásilkovna DACH edge cases missed during testing | M | M | Create explicit test cases for DE, AT, CH with both Light and Heavy weight tiers before release |
| Language preference cookie triggers GDPR consent requirement | M | H | Classify as functional cookie during legal review; if consent is required, add to cookie consent banner before launch |
| Czech diacritics corrupted by database encoding | L | H | Verify database character set is `utf8mb4` before importing Czech content; include Czech diacritics in smoke tests |
| Delivery spec discrepancy (US2450 vs US4200) causes scope confusion in team | M | M | Formally close US2450 in favor of US4200; communicate to whole development and test team |
| Alza.cz integration (deferred) creates false market readiness expectation | L | M | Document clearly in release notes that Alza.cz integration is not part of Sprint 6 scope |

---

## Open Decisions

- [x] **US3150 delivery options specification** — Resolved: US4200 (backlog-V6CZ) is the authoritative spec. Weight-based pricing (Light < 10 kg / Heavy ≥ 10 kg), Standard + Zásilkovna options, Zásilkovna for CZ and DACH only, CZK for CZ billing / USD otherwise.
- [x] **Language persistence scope** — Resolved: persistent via browser cookie (no login required).
- [x] **PayU region restriction criteria** — Resolved: PayU is displayed when billing address OR shipping address is Czech Republic (aligns with US3100 ACC-02).
- [x] **Delivery cost currency** — Resolved: CZK when billing country = CZ, USD otherwise (per US4200).
- [x] **Alza.cz marketplace integration** — Resolved: deferred to a future sprint. Not in scope for Sprint 6.
- [ ] **Czech product content fallback language** — Both English and German were raised as candidates. English is used as the primary fallback in the BACs. If German fallback is required for DACH customers, this decision must be finalized before the US2350 story is implemented.

## References

- [US2300 — Czech Language Support (sprint input)](courses/GO-IT-Academy/HolisticTestingWithGenAI-ScrumMasterCommunity-20261012/sprint6/sprint6-input/US2300-support-czech-language.md)
- [US2350 — Czech Product Content (sprint input)](courses/GO-IT-Academy/HolisticTestingWithGenAI-ScrumMasterCommunity-20261012/sprint6/sprint6-input/US2350-czech-products.md)
- [US2450 — Delivery Costs (sprint input — superseded by US4200)](courses/GO-IT-Academy/HolisticTestingWithGenAI-ScrumMasterCommunity-20261012/sprint6/sprint6-input/US2450-delivery-cost.md)
- [US3100 — PayU Payment Integration (sprint input)](courses/GO-IT-Academy/HolisticTestingWithGenAI-ScrumMasterCommunity-20261012/sprint6/sprint6-input/US3100-support-payment-type-PayU.md)
- [US3150 — New Delivery Options (sprint input — superseded by US4200)](courses/GO-IT-Academy/HolisticTestingWithGenAI-ScrumMasterCommunity-20261012/sprint6/sprint6-input/US3150-delivery-option.md)
- [Sprint 6 Goal](courses/GO-IT-Academy/HolisticTestingWithGenAI-ScrumMasterCommunity-20261012/sprint6/sprint6-input/sprint6-sprintGoal.md)
- [Test Automation Strategy](courses/GO-IT-Academy/HolisticTestingWithGenAI-ScrumMasterCommunity-20261012/sprint6/sprint6-input/testAutomationStrategy.md)
- [US2300 — Czech Language Support (backlog-V6CZ)](systemsUnderTest/toolShop/backlog-V6CZ/US2300-support-czech-language.md)
- [US2350 — Czech Product Content (backlog-V6CZ)](systemsUnderTest/toolShop/backlog-V6CZ/US2350-czech-products.md)
- [US4200 — Delivery Options and Costs (backlog-V6CZ — authoritative spec)](systemsUnderTest/toolShop/backlog-V6CZ/US4200-delivery-costs.md)
- [US3100 — PayU Payment Integration (backlog-V6CZ)](systemsUnderTest/toolShop/backlog-V6CZ/US3100-support-payment-type-PayU.md)
- [US9100 — Mock PayU Payment Service (backlog-V6CZ — prerequisite)](systemsUnderTest/toolShop/backlog-V6CZ/US9100-Mock-PayU-Payment-Service.md)
- [US9200 — PayU TIP Testing UI (backlog-V6CZ)](systemsUnderTest/toolShop/backlog-V6CZ/US9200-PayU-TIP-Testing-UI.md)
- [ToolShop Practice App](https://practicesoftwaretesting.com/)

## Consistency Notes

- No `docs/INDEX.md` found at repo root. Consistency check performed against sprint input documents and `systemsUnderTest/toolShop/backlog-V6CZ/` documents.
- **US2450 superseded by US4200**: The delivery cost model in the sprint input (value-based, EUR, express option) conflicts with the more detailed backlog spec US4200 (weight-based, CZK/USD, no express). US4200 is authoritative. US2450 is closed.
- US2300 (language support) and US2350 (Czech product content) overlap on product titles/descriptions. US2300 establishes the language switching mechanism; US2350 scopes the content to specific articles with a fallback language. Both are in scope and complementary.
- US9100 (Mock PayU Service) is a prerequisite story not included in the Czech Market Expansion epic. It must be tracked and sequenced before US3100 in sprint planning.
- The test automation strategy references `T5_Payment_InvalidPaymentCallback_RequestRejected` and `T5_Checkout_EURHighAmountPolicy_EnforcedOrDocumentedConsistently` — these map to BAC-22 and the delivery pricing rules respectively, and should be included in the merge-gate automation spine.
- Angular uses Transloco for i18n (confirmed by US9200 which explicitly skips Transloco for the PayU TIP page, implying all other pages use it).
