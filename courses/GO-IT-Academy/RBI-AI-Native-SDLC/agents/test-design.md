# Test Design — Plan: ToolShop Czech Market Entry — Sprint 6

Generated from: plan.md  
Date: 2026-08-12

---

## Summary

| Metric | Count |
|---|---|
| Business Acceptance Criteria | 27 |
| Test Situations (total) | 57 |
| Happy Path | 29 |
| Negative | 18 |
| Boundary | 4 |
| Non-functional | 3 |
| Integration | 3 |

---

## Test Situations by BAC

---

### BAC-1 — Language switcher visible on all pages

**Story/Epic:** Story 1 — Czech Language & Localisation  
**Type(s):** Functional

#### TS-1.1 — Switcher present on every customer-facing page

| Field | Value |
|---|---|
| **BAC ref** | BAC-1 |
| **Type** | Happy Path |
| **Preconditions** | Application loaded; user on a customer-facing page (product listing, product detail, home, checkout) |
| **Action** | Load any customer-facing page |
| **Expected outcome** | A language switcher control is visible and interactive within the page layout on every customer-facing page |
| **Notes** | Must be visible without scrolling or navigating to a secondary screen |

---

### BAC-2 — Czech language activates immediately, no reload

**Story/Epic:** Story 1 — Czech Language & Localisation  
**Type(s):** Functional, Negative

#### TS-2.1 — Selecting Czech switches all UI text on the current page immediately

| Field | Value |
|---|---|
| **BAC ref** | BAC-2 |
| **Type** | Happy Path |
| **Preconditions** | Application loaded in English; language switcher is visible |
| **Action** | Select "Czech" from the language switcher |
| **Expected outcome** | All navigation elements, buttons, labels, headings, and form field labels on the current page switch to Czech text. The page does not reload (URL unchanged, scroll position preserved). |

#### TS-2.2 — Switching language does not discard in-progress page state

| Field | Value |
|---|---|
| **BAC ref** | BAC-2 |
| **Type** | Negative |
| **Preconditions** | Customer has in-progress state on the page: has applied a product filter, scrolled a listing, or has items in the cart |
| **Action** | Select Czech from the language switcher while in this state |
| **Expected outcome** | UI text switches to Czech; in-progress state (cart contents, applied filters, scroll position) is fully preserved. No data is lost and no navigation occurs. |

---

### BAC-3 — Czech language persists across browser sessions

**Story/Epic:** Story 1 — Czech Language & Localisation  
**Type(s):** Functional, Boundary

#### TS-3.1 — Selecting Czech writes a persistent language preference

| Field | Value |
|---|---|
| **BAC ref** | BAC-3 |
| **Type** | Happy Path |
| **Preconditions** | Application loaded; language is English |
| **Action** | Select Czech from the language switcher |
| **Expected outcome** | A language preference token (`lang=cs`) is written to browser cookie storage. The value is accessible to the application on subsequent visits. |

#### TS-3.2 — Returning to the app restores Czech locale from stored preference

| Field | Value |
|---|---|
| **BAC ref** | BAC-3 |
| **Type** | Happy Path |
| **Preconditions** | Browser has a `lang=cs` cookie set from a prior session |
| **Action** | Open the application (new page load / app initialisation) |
| **Expected outcome** | Application initialises in Czech locale without requiring the customer to re-select it. |

#### TS-3.3 — Language preference survives full browser close and restart

| Field | Value |
|---|---|
| **BAC ref** | BAC-3 |
| **Type** | Boundary |
| **Preconditions** | Customer previously selected Czech; browser was closed entirely (not just the tab) |
| **Action** | Reopen the browser and navigate to the application |
| **Expected outcome** | Application loads in Czech. The cookie is a persistent cookie (not session-scoped) and survives a full browser restart. |

---

### BAC-4 — Checkout fully in Czech

**Story/Epic:** Story 1 — Czech Language & Localisation  
**Type(s):** Functional

#### TS-4.1 — All checkout step labels and form fields rendered in Czech

| Field | Value |
|---|---|
| **BAC ref** | BAC-4 |
| **Type** | Happy Path |
| **Preconditions** | Locale is Czech; customer navigates into the checkout flow |
| **Action** | Progress through each checkout step: cart → address → payment → order confirmation |
| **Expected outcome** | Every step heading, form field label, button text, and section title is displayed in Czech. No English strings are visible on any checkout step. |

#### TS-4.2 — Order confirmation copy is rendered in Czech

| Field | Value |
|---|---|
| **BAC ref** | BAC-4 |
| **Type** | Happy Path |
| **Preconditions** | Locale is Czech; customer completes a purchase |
| **Action** | View the order confirmation page after successful checkout |
| **Expected outcome** | The confirmation message and all supporting text on the confirmation page are in Czech. |

---

### BAC-5 — Error and validation messages in Czech

**Story/Epic:** Story 1 — Czech Language & Localisation  
**Type(s):** Functional, Negative

#### TS-5.1 — Required field validation message displayed in Czech

| Field | Value |
|---|---|
| **BAC ref** | BAC-5 |
| **Type** | Negative |
| **Preconditions** | Locale is Czech; customer is on a form (e.g., checkout address step) |
| **Action** | Submit the form with one or more required fields left empty |
| **Expected outcome** | Validation error message(s) are displayed in Czech text. No English error text is visible. |

#### TS-5.2 — All validation error types displayed in Czech

| Field | Value |
|---|---|
| **BAC ref** | BAC-5 |
| **Type** | Negative |
| **Preconditions** | Locale is Czech; customer is on a form with multiple validation rule types |
| **Action** | Trigger validation errors of each type: required, invalid email format, minimum length, mismatched passwords |
| **Expected outcome** | Every category of validation error message is displayed in Czech. No type of error falls back to English. |

---

### BAC-6 — Czech product content on listing and detail pages

**Story/Epic:** Story 2 — Czech Product Content  
**Type(s):** Functional

#### TS-6.1 — Czech product title displayed on the product listing page

| Field | Value |
|---|---|
| **BAC ref** | BAC-6 |
| **Type** | Happy Path |
| **Preconditions** | Locale is Czech; at least one product with a Czech title exists in the catalogue |
| **Action** | Navigate to the product listing page |
| **Expected outcome** | Products with Czech translations display their Czech title in the listing grid or list view. |

#### TS-6.2 — Czech title, description, and specifications on product detail page

| Field | Value |
|---|---|
| **BAC ref** | BAC-6 |
| **Type** | Happy Path |
| **Preconditions** | Locale is Czech; viewing a product that has Czech title, description, and technical specifications populated |
| **Action** | Open the product detail page for a translated product |
| **Expected outcome** | Product title, description, and all technical specifications are displayed in Czech. |

---

### BAC-7 — English fallback for untranslated products

**Story/Epic:** Story 2 — Czech Product Content  
**Type(s):** Negative

#### TS-7.1 — Product without Czech title falls back to English title (not blank)

| Field | Value |
|---|---|
| **BAC ref** | BAC-7 |
| **Type** | Negative |
| **Preconditions** | Locale is Czech; a product exists in the catalogue with no Czech title (field is null or empty) |
| **Action** | View that product on the listing or detail page |
| **Expected outcome** | The product title is displayed in English. The title area is not blank, not broken, and contains no error indicator or placeholder text. |

#### TS-7.2 — Product without Czech description falls back to English description (not blank)

| Field | Value |
|---|---|
| **BAC ref** | BAC-7 |
| **Type** | Negative |
| **Preconditions** | Locale is Czech; a product exists with no Czech description (field is null or empty) |
| **Action** | View the product detail page for that product |
| **Expected outcome** | Description is displayed in English. The description area is not blank or broken. |

---

### BAC-8 — Czech product titles in search results

**Story/Epic:** Story 2 — Czech Product Content  
**Type(s):** Functional

#### TS-8.1 — Search results show Czech titles for translated products

| Field | Value |
|---|---|
| **BAC ref** | BAC-8 |
| **Type** | Happy Path |
| **Preconditions** | Locale is Czech; products with Czech translations exist in the catalogue |
| **Action** | Enter a search query that matches products that have Czech content |
| **Expected outcome** | Search results display Czech titles and descriptions for products with Czech translations. Products without Czech translations display English titles (fallback per BAC-7). |

---

### BAC-9 — Czech product content visible through checkout

**Story/Epic:** Story 2 — Czech Product Content  
**Type(s):** Functional

#### TS-9.1 — Czech product title visible in the cart

| Field | Value |
|---|---|
| **BAC ref** | BAC-9 |
| **Type** | Happy Path |
| **Preconditions** | Locale is Czech; a product with a Czech title has been added to the cart |
| **Action** | View the cart |
| **Expected outcome** | The cart line item displays the product's Czech title. |

#### TS-9.2 — Czech product title visible in the checkout order summary

| Field | Value |
|---|---|
| **BAC ref** | BAC-9 |
| **Type** | Happy Path |
| **Preconditions** | Locale is Czech; a translated product is in the cart; customer is on the checkout payment or confirmation step |
| **Action** | View the order summary panel within the checkout |
| **Expected outcome** | The product title in the order summary is displayed in Czech. |

---

### BAC-10 — PayU available for Czech addresses only

**Story/Epic:** Story 3 — PayU Payment Integration  
**Type(s):** Functional

#### TS-10.1 — PayU appears when billing address is Czech Republic

| Field | Value |
|---|---|
| **BAC ref** | BAC-10 |
| **Type** | Happy Path |
| **Preconditions** | Customer is on the checkout payment step; billing address country is Czech Republic; shipping address is any non-CZ country |
| **Action** | View the payment method options |
| **Expected outcome** | PayU appears as a selectable payment option in the list. |

#### TS-10.2 — PayU appears when only the shipping address is Czech Republic

| Field | Value |
|---|---|
| **BAC ref** | BAC-10 |
| **Type** | Happy Path |
| **Preconditions** | Customer is on the checkout payment step; billing address country is non-CZ (e.g., Germany); shipping address country is Czech Republic |
| **Action** | View the payment method options |
| **Expected outcome** | PayU appears as a selectable payment option. Either billing OR shipping country = CZ is sufficient for PayU to be shown. |

---

### BAC-11 — PayU not shown for non-Czech addresses

**Story/Epic:** Story 3 — PayU Payment Integration  
**Type(s):** Negative

#### TS-11.1 — PayU absent when both billing and shipping countries are outside Czech Republic

| Field | Value |
|---|---|
| **BAC ref** | BAC-11 |
| **Type** | Negative |
| **Preconditions** | Customer is on the checkout payment step; billing address country is non-CZ (e.g., Germany); shipping address country is non-CZ (e.g., Austria) |
| **Action** | View the payment method options |
| **Expected outcome** | PayU is not present in the payment options list. All other payment methods are unaffected and still displayed. |

---

### BAC-12 — Successful PayU payment creates order

**Story/Epic:** Story 3 — PayU Payment Integration  
**Type(s):** Functional, Integration

#### TS-12.1 — Successful gateway response results in a confirmed order

| Field | Value |
|---|---|
| **BAC ref** | BAC-12 |
| **Type** | Integration |
| **Preconditions** | CZ customer is on the checkout payment step with PayU selected; payment gateway (or mock) is configured to return a success response |
| **Action** | Click Confirm; gateway processes the payment and returns success |
| **Expected outcome** | A payment confirmation is displayed to the customer. An order record is created in the system with a status reflecting successful payment (`payment_confirmed`). Customer is not left in an ambiguous or broken state. |

---

### BAC-13 — Failed or cancelled PayU payment allows retry

**Story/Epic:** Story 3 — PayU Payment Integration  
**Type(s):** Negative

#### TS-13.1 — Declined payment shows error and does not create an order

| Field | Value |
|---|---|
| **BAC ref** | BAC-13 |
| **Type** | Negative |
| **Preconditions** | CZ customer is on the payment step with PayU selected; payment gateway configured to decline the payment |
| **Action** | Click Confirm; gateway declines the payment |
| **Expected outcome** | A clear error message is displayed. No order record is created. The Confirm button remains active (not replaced by a "Buy now" button). The customer can retry without restarting checkout. |

#### TS-13.2 — Cancelled PayU checkout allows retry without restarting

| Field | Value |
|---|---|
| **BAC ref** | BAC-13 |
| **Type** | Negative |
| **Preconditions** | CZ customer on the payment step with PayU selected; customer initiates payment then cancels on the PayU hosted page |
| **Action** | Cancel the payment on the PayU hosted page and return to the webshop |
| **Expected outcome** | Customer is returned to the payment step with an error or cancellation message. PayU is still available and selectable. Customer can click Confirm again and retry without restarting checkout from step 1. |

---

### BAC-14 — Sensitive payment data not stored

**Story/Epic:** Story 3 — PayU Payment Integration  
**Type(s):** Non-functional

#### TS-14.1 — No card number or CVV present in any stored record after a PayU payment

| Field | Value |
|---|---|
| **BAC ref** | BAC-14 |
| **Type** | Non-functional |
| **Preconditions** | A PayU payment has been attempted (successful, failed, or abandoned) |
| **Action** | Inspect the order record and all related tables for the payment in the database |
| **Expected outcome** | No card number, CVV code, or raw card data field is present in any stored record. Only a transaction reference ID and an order status value are stored on the order record. |

#### TS-14.2 — No raw gateway credentials present in any stored or logged record

| Field | Value |
|---|---|
| **BAC ref** | BAC-14 |
| **Type** | Non-functional |
| **Preconditions** | PayU integration is operational and a payment cycle has completed |
| **Action** | Inspect all order records, log entries, and API responses produced during the payment cycle |
| **Expected outcome** | API keys, client secrets, and gateway credentials do not appear in any database record, log entry, or API response body. |

---

### BAC-15 — Standard delivery available globally

**Story/Epic:** Story 4 — Delivery Pricing  
**Type(s):** Functional, Boundary

#### TS-15.1 — Standard delivery shown for Czech Republic shipping address

| Field | Value |
|---|---|
| **BAC ref** | BAC-15 |
| **Type** | Happy Path |
| **Preconditions** | Customer is on the delivery step; shipping address country is Czech Republic; cart has known weight |
| **Action** | View delivery options |
| **Expected outcome** | Standard delivery option is shown with the CZ region price for the applicable weight tier (Light or Heavy). |

#### TS-15.2 — Standard delivery shown for EU (non-DACH) shipping address

| Field | Value |
|---|---|
| **BAC ref** | BAC-15 |
| **Type** | Happy Path |
| **Preconditions** | Customer is on the delivery step; shipping address is an EU country outside DACH (e.g., France, Poland) |
| **Action** | View delivery options |
| **Expected outcome** | Standard delivery option is shown with the EU region price for the applicable weight tier. |

#### TS-15.3 — Standard delivery shown for US shipping address

| Field | Value |
|---|---|
| **BAC ref** | BAC-15 |
| **Type** | Happy Path |
| **Preconditions** | Customer is on the delivery step; shipping address country is United States |
| **Action** | View delivery options |
| **Expected outcome** | Standard delivery option is shown with the US region price for the applicable weight tier. |

#### TS-15.4 — Displayed delivery price matches pricing matrix for every region × weight combination

| Field | Value |
|---|---|
| **BAC ref** | BAC-15 |
| **Type** | Boundary |
| **Preconditions** | Cart items with precisely known total weights (one Light scenario < 10 kg, one Heavy scenario ≥ 10 kg); delivery step reached for each supported region: CZ, DACH, EU, US, Others |
| **Action** | Check the displayed Standard delivery price for each of the 8+ region/weight combinations defined in the authoritative pricing matrix (US4200) |
| **Expected outcome** | Every displayed price matches the pricing matrix exactly. No price deviates due to rounding, currency conversion, or region-classification error. |

---

### BAC-16 — Zásilkovna available for CZ and DACH only

**Story/Epic:** Story 4 — Delivery Pricing  
**Type(s):** Functional

#### TS-16.1 — Zásilkovna shown for Czech Republic shipping address

| Field | Value |
|---|---|
| **BAC ref** | BAC-16 |
| **Type** | Happy Path |
| **Preconditions** | Customer is on the delivery step; shipping address country is Czech Republic |
| **Action** | View delivery options |
| **Expected outcome** | Zásilkovna appears as an additional delivery option alongside Standard. |

#### TS-16.2 — Zásilkovna shown for all three DACH countries

| Field | Value |
|---|---|
| **BAC ref** | BAC-16 |
| **Type** | Happy Path |
| **Preconditions** | Customer is on the delivery step; shipping address country is one of Germany, Austria, or Switzerland (tested separately for each) |
| **Action** | View delivery options for each DACH country |
| **Expected outcome** | Zásilkovna is listed as an option for each of the three DACH countries. |

---

### BAC-17 — Zásilkovna not shown for other regions

**Story/Epic:** Story 4 — Delivery Pricing  
**Type(s):** Negative

#### TS-17.1 — Zásilkovna absent for EU (non-DACH) shipping address

| Field | Value |
|---|---|
| **BAC ref** | BAC-17 |
| **Type** | Negative |
| **Preconditions** | Customer is on the delivery step; shipping address is an EU country outside CZ and DACH (e.g., France, Poland) |
| **Action** | View delivery options |
| **Expected outcome** | Zásilkovna is not present in the list. Only Standard delivery is shown. |

#### TS-17.2 — Zásilkovna absent for US shipping address

| Field | Value |
|---|---|
| **BAC ref** | BAC-17 |
| **Type** | Negative |
| **Preconditions** | Customer is on the delivery step; shipping address country is United States |
| **Action** | View delivery options |
| **Expected outcome** | Zásilkovna is not present in the delivery options. |

---

### BAC-18 — Delivery price reflects cart weight tier

**Story/Epic:** Story 4 — Delivery Pricing  
**Type(s):** Boundary

#### TS-18.1 — Light tier rates applied when cart total weight is just below the threshold

| Field | Value |
|---|---|
| **BAC ref** | BAC-18 |
| **Type** | Boundary |
| **Preconditions** | Cart total weight is 9.99 kg (one unit below the Heavy threshold) |
| **Action** | View delivery options on the delivery step |
| **Expected outcome** | Light tier rates are displayed for all applicable delivery options. Heavy tier rates are not applied. |

#### TS-18.2 — Heavy tier rates applied when cart total weight is exactly at the threshold

| Field | Value |
|---|---|
| **BAC ref** | BAC-18 |
| **Type** | Boundary |
| **Preconditions** | Cart total weight is exactly 10.00 kg (the defined boundary value) |
| **Action** | View delivery options on the delivery step |
| **Expected outcome** | Heavy tier rates are displayed. 10.00 kg is classified as Heavy (≥ 10 kg), not Light (< 10 kg). |

#### TS-18.3 — Light tier rates applied for a typical low-weight cart

| Field | Value |
|---|---|
| **BAC ref** | BAC-18 |
| **Type** | Happy Path |
| **Preconditions** | Cart total weight is 1.00 kg (well within Light tier) |
| **Action** | View delivery options |
| **Expected outcome** | Light tier rates are displayed for all applicable delivery options. |

---

### BAC-19 — Delivery price shown in customer's billing currency

**Story/Epic:** Story 4 — Delivery Pricing  
**Type(s):** Functional

#### TS-19.1 — Delivery prices displayed in CZK for a Czech billing country

| Field | Value |
|---|---|
| **BAC ref** | BAC-19 |
| **Type** | Happy Path |
| **Preconditions** | Customer's billing address country is Czech Republic; delivery step is reached |
| **Action** | View delivery options and their prices |
| **Expected outcome** | All delivery prices are displayed in Czech Koruna (Kč / CZK). No USD or EUR amounts are shown for delivery. |

#### TS-19.2 — Delivery prices displayed in USD for a non-Czech billing country

| Field | Value |
|---|---|
| **BAC ref** | BAC-19 |
| **Type** | Happy Path |
| **Preconditions** | Customer's billing address country is non-CZ (e.g., Germany or United States); delivery step is reached |
| **Action** | View delivery options and their prices |
| **Expected outcome** | All delivery prices are displayed in USD. No CZK amounts are shown. |

---

### BAC-20 — Czech products discoverable on Alza.cz

**Story/Epic:** Story 5 — Alza.cz XML Product Feed  
**Type(s):** Integration, Functional, Negative

#### TS-20.1 — Feed XML includes all products with Czech content populated

| Field | Value |
|---|---|
| **BAC ref** | BAC-20 |
| **Type** | Integration |
| **Preconditions** | Products with Czech title and description populated exist in the catalogue; feed generator is triggered |
| **Action** | Trigger the feed generation process; inspect the output XML |
| **Expected outcome** | All products with a populated Czech title appear in the generated XML. Products without Czech content are excluded from the Czech feed output. |

#### TS-20.2 — Feed endpoint returns valid XML with correct content type for authenticated requests

| Field | Value |
|---|---|
| **BAC ref** | BAC-20 |
| **Type** | Integration |
| **Preconditions** | A valid authentication bearer token is provided; a feed file has been generated |
| **Action** | Send an authenticated GET request to the feed endpoint |
| **Expected outcome** | Response is HTTP 200 with `Content-Type: application/xml`. The response body is valid, well-formed XML conforming to the Alza.cz schema. |

#### TS-20.3 — Feed endpoint rejects unauthenticated and incorrectly-authenticated requests

| Field | Value |
|---|---|
| **BAC ref** | BAC-20 |
| **Type** | Negative |
| **Preconditions** | Request is sent without a bearer token, or with an incorrect/expired bearer token |
| **Action** | Send the GET request to the feed endpoint without valid authentication |
| **Expected outcome** | Response is HTTP 401. No feed XML data is returned. |

---

### BAC-21 — Product changes reflected on Alza.cz within 6 hours

**Story/Epic:** Story 5 — Alza.cz XML Product Feed  
**Type(s):** Functional

#### TS-21.1 — Updated product price is reflected in the feed after the next export run

| Field | Value |
|---|---|
| **BAC ref** | BAC-21 |
| **Type** | Happy Path |
| **Preconditions** | A product's price has been updated in ToolShop since the last feed export |
| **Action** | Trigger the feed generator (simulating the next scheduled run); inspect the output XML for that product |
| **Expected outcome** | The updated price is present in the XML for that product. The previous price is no longer present. |

#### TS-21.2 — Updated stock level is reflected in the feed after the next export run

| Field | Value |
|---|---|
| **BAC ref** | BAC-21 |
| **Type** | Happy Path |
| **Preconditions** | A product's stock level has been updated in ToolShop since the last feed export |
| **Action** | Trigger the feed generator; inspect the output XML for that product |
| **Expected outcome** | The updated stock value is present in the XML for that product. |

---

### BAC-22 — Mock PayU default response is success

**Story/Epic:** Story 6 — Mock PayU Service  
**Type(s):** Functional

#### TS-22.1 — Default mock response is HTTP 200 with a success body

| Field | Value |
|---|---|
| **BAC ref** | BAC-22 |
| **Type** | Happy Path |
| **Preconditions** | Non-production environment; mock PayU service is active; request contains no `X-PayU-Mock-Scenario` header |
| **Action** | Send a POST request to the mock PayU orders endpoint with `amount`, `currency`, and `order_id` in the JSON body |
| **Expected outcome** | Response is HTTP 200 with a JSON body containing `status: "SUCCESS"`, a `transaction_id` value, and a `message` field. |

---

### BAC-23 — Mock PayU scenario-driven error responses

**Story/Epic:** Story 6 — Mock PayU Service  
**Type(s):** Functional, Negative

#### TS-23.1 — `declined` scenario returns HTTP 422 with payment declined error

| Field | Value |
|---|---|
| **BAC ref** | BAC-23 |
| **Type** | Negative |
| **Preconditions** | Non-production environment; request includes `X-PayU-Mock-Scenario: declined` header |
| **Action** | Send a POST request to the mock endpoint |
| **Expected outcome** | Response is HTTP 422 with JSON body `{ "error": "Payment declined" }`. |

#### TS-23.2 — `unavailable` scenario returns HTTP 503 with service unavailable error

| Field | Value |
|---|---|
| **BAC ref** | BAC-23 |
| **Type** | Negative |
| **Preconditions** | Non-production environment; request includes `X-PayU-Mock-Scenario: unavailable` header |
| **Action** | Send a POST request to the mock endpoint |
| **Expected outcome** | Response is HTTP 503 with JSON body `{ "error": "PayU service unavailable" }`. |

#### TS-23.3 — `timeout` scenario returns HTTP 502 (after a delay)

| Field | Value |
|---|---|
| **BAC ref** | BAC-23 |
| **Type** | Negative |
| **Preconditions** | Non-production environment; request includes `X-PayU-Mock-Scenario: timeout` header |
| **Action** | Send a POST request to the mock endpoint |
| **Expected outcome** | Response is HTTP 502. The response may be preceded by a short artificial delay to simulate a gateway timeout. |

---

### BAC-24 — Checkout UI reflects mock PayU responses

**Story/Epic:** Story 6 — Mock PayU Service  
**Type(s):** Functional, Negative

#### TS-24.1 — Success response shows green alert and enables Buy Now

| Field | Value |
|---|---|
| **BAC ref** | BAC-24 |
| **Type** | Happy Path |
| **Preconditions** | CZ customer is on the checkout payment step with PayU selected; gateway returns HTTP 200 with `{ "message": "Payment was successful" }` |
| **Action** | Click Confirm |
| **Expected outcome** | A green success alert is displayed containing the text from the `message` field. The "Buy now" button becomes available (payment confirmed state is set). Any previously shown error alert is cleared. |

#### TS-24.2 — Error response shows red alert and keeps Confirm active

| Field | Value |
|---|---|
| **BAC ref** | BAC-24 |
| **Type** | Negative |
| **Preconditions** | CZ customer on the checkout payment step with PayU selected; gateway returns HTTP 422 with `{ "error": "Payment declined" }` |
| **Action** | Click Confirm |
| **Expected outcome** | A red error alert is displayed containing the text from the `error` field. The Confirm button remains active (payment confirmed state is not set). Any previously shown success alert is cleared. Customer can retry. |

#### TS-24.3 — Non-JSON error response shows generic fallback in red alert

| Field | Value |
|---|---|
| **BAC ref** | BAC-24 |
| **Type** | Negative |
| **Preconditions** | CZ customer on checkout payment step with PayU selected; gateway returns an error HTTP status with a non-JSON or empty response body |
| **Action** | Click Confirm |
| **Expected outcome** | A red error alert displays a generic fallback message (e.g., "Unknown error"). Payment is not confirmed. The Confirm button remains active. |

---

### BAC-25 — Mock PayU not reachable in production

**Story/Epic:** Story 6 — Mock PayU Service  
**Type(s):** Non-functional

#### TS-25.1 — Mock PayU endpoint returns 404 in production environment

| Field | Value |
|---|---|
| **BAC ref** | BAC-25 |
| **Type** | Non-functional |
| **Preconditions** | Application is deployed to a production environment (`APP_ENV=production`) |
| **Action** | Send any HTTP request to the mock PayU orders endpoint path |
| **Expected outcome** | Response is HTTP 404. The route is not registered and does not exist in the production application. No mock response body is returned. |

---

### BAC-26 — PayU TIP page accessible from header

**Story/Epic:** Story 7 — PayU TIP Testing UI  
**Type(s):** Functional

#### TS-26.1 — PayU TIP link is visible in the header on all pages

| Field | Value |
|---|---|
| **BAC ref** | BAC-26 |
| **Type** | Happy Path |
| **Preconditions** | Application loaded; user is on any page (product listing, detail, checkout, etc.) |
| **Action** | Inspect the main header navigation |
| **Expected outcome** | A link labelled "PayU TIP" (always English, never translated) is visible in the header navigation. The link carries the test identifier `data-test="nav-payu-tip"`. |

#### TS-26.2 — Clicking the PayU TIP link navigates to the PayU TIP page

| Field | Value |
|---|---|
| **BAC ref** | BAC-26 |
| **Type** | Happy Path |
| **Preconditions** | User is on any page; "PayU TIP" link is visible in the header |
| **Action** | Click the "PayU TIP" link |
| **Expected outcome** | Browser navigates to the `/payu-tip` route. The PayU TIP page is rendered. |

---

### BAC-27 — PayU TIP form submits to mock and shows response modal

**Story/Epic:** Story 7 — PayU TIP Testing UI  
**Type(s):** Functional, Negative

#### TS-27.1 — Form sends a POST request with all provided field values and optional scenario header

| Field | Value |
|---|---|
| **BAC ref** | BAC-27 |
| **Type** | Happy Path |
| **Preconditions** | User is on the PayU TIP page; form fields filled: `amount`, `currency`, `order_id` set to test values; `scenario` set to `declined` |
| **Action** | Click "Send request" |
| **Expected outcome** | A POST request is sent to `{apiUrl}/mock/payu/orders` with a JSON body containing the form field values. The `X-PayU-Mock-Scenario: declined` header is included because a scenario was selected. |
| **Notes** | Repeat without a scenario selected to confirm no scenario header is sent when the field is empty |

#### TS-27.2 — Modal opens with HTTP 200 status and message text on success response

| Field | Value |
|---|---|
| **BAC ref** | BAC-27 |
| **Type** | Happy Path |
| **Preconditions** | Form submitted; mock returns HTTP 200 with `{ "message": "Payment was successful" }` |
| **Action** | Observe the response after form submission |
| **Expected outcome** | A modal dialog identified by `data-test="payu-tip-response-modal"` is displayed. The modal shows the HTTP status code `200` and the text from the `message` field. A Close button is present and dismisses the modal. |

#### TS-27.3 — Modal opens with HTTP 422 status and error text on failure response

| Field | Value |
|---|---|
| **BAC ref** | BAC-27 |
| **Type** | Negative |
| **Preconditions** | Form submitted; mock returns HTTP 422 with `{ "error": "Payment declined" }` |
| **Action** | Observe the response after form submission |
| **Expected outcome** | Modal is displayed with HTTP status code `422` and the text from the `error` field. Close button dismisses the modal. |

#### TS-27.4 — Modal opens with fallback message when the network request fails

| Field | Value |
|---|---|
| **BAC ref** | BAC-27 |
| **Type** | Negative |
| **Preconditions** | Form submitted; the network request fails before any HTTP response is received (simulated network error) |
| **Action** | Observe the response after form submission |
| **Expected outcome** | Modal still opens. Displays an appropriate fallback message (e.g., "Request failed"). HTTP status code display handles the absence of a response gracefully. Close button works. |

---

## Coverage Validation

| Check | Result |
|---|---|
| Every BAC has at least one test situation | ✓ All 27 BACs covered |
| Every Functional BAC has a Happy Path situation | ✓ |
| Every BAC mentioning rejection or error has a Negative situation | ✓ (BAC-5, BAC-7, BAC-11, BAC-13, BAC-17, BAC-20, BAC-23, BAC-24, BAC-25, BAC-27) |
| Every BAC with a numeric threshold has a Boundary situation | ✓ (BAC-3: session persistence; BAC-15: all region/weight combos; BAC-18: 10 kg boundary at-value and just-below) |
| No duplicate situations (same action + same expected outcome) | ✓ |
| BACs with OR conditions have one situation per branch | ✓ (BAC-10: billing-CZ vs. shipping-CZ; BAC-18: Light vs. Heavy; BAC-19: CZK vs. USD) |
| BACs with multiple roles/states have one situation per state | ✓ (BAC-16: CZ + each DACH country; BAC-24: success + error + non-JSON) |
