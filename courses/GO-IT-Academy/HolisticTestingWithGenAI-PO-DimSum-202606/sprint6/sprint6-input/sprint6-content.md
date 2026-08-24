# Sprint 6 — consolidated input

This document aggregates all material from `sprint6-input/`.

---

## sprint6-sprintGoal.md

# Sprint Goal - Sprint 6

## Go to Czech market

Establish the foundation for entering the Czech market as an alternative growth strategy after the cancellation of the planned China expansion, by enabling Czech customer support through marketplace integration with Alza.cz, Czech language capabilities, localized product offerings, and integration of the local payment solution PayU to validate market acceptance and commercial potential in Czechia.

## Security enhancements

In parallel, improve the security foundation of the webshop by implementing secure password hashing and replacing the current authentication library with a more modern and maintainable authentication solution to reduce security risks and support future scalability.

## User Stories
- US2300-Czech Language Support 
- US2350-Provide Product Articles in Czech Language
- US2450-Delivery-Cost
- US3100-Add PayU Payment Support for Czech Customers
- US3150-New-Delivery-Options
- US3206-MD5 Hashed password
- US3409-Fix vulnerability in "Auth-X32"
- US4700-Login during checkout workflow
- US5003-Replace ORMapper

---

## US2300-support-czech-language.md

# US2300-Czech Language Support 

## Add Czech Language Support to the Webshop

As a customer from the Czech Republic
I want to use the webshop in the Czech language
So that I can understand products, navigation, and the checkout process more easily and trust the webshop during my purchase journey.

## Description

To support the expansion into the Czech market, the webshop shall provide full Czech language support for customer-facing content and workflows.

## The implementation should include:

Language switcher in the webshop

- Czech translations for navigation and UI elements
- Czech product titles and descriptions
- Czech error and validation messages


---

## US2350-czech-products.md

# User Story: Add Czech Product Content to the Webshop

## US2350-Provide Product Articles in Czech Language

As a customer from the Czech Republic
I want to read product information in Czech
So that I can understand the products and make informed purchasing decisions.

## Description

To support the Czech market expansion, the webshop shall provide Czech-language product content for selected articles and product categories.



---

## US2450-delivery-cost.md

# Delivery Costs
Free Standard Shipping: 
- Applies to all orders over €75 (excluding "Oversized" items).

Flat Rate Shipping: 
- €5.95 for orders under €75. 

Heavy/Oversized Surcharge: 
- Any single item over 25kg or large dimensions adds €20 surcharge, regardless of order total. 

Express Option: 
- €12.95 for next-day delivery (only for in-stock items).

---

## US3100-support-payment-type-PayU.md

# User Story: Integrate PayU Czech Republic as Payment Method

## US3100-Add PayU Payment Support for Czech Customers

As a customer from the Czech Republic  
I want to pay using a PayU Czech Republic local payment solution  
So that I can complete my purchase securely and conveniently.

## Description

To support the expansion into the Czech market, the webshop shall integrate PayU as an additional payment method for Czech customers.

## The integration should support:

- Local Czech payment options supported by PayU
- Secure payment processing
- Payment status synchronization
- Czech currency support where applicable

## Acceptance Criteria

### Functional

- Customer can select PayU during checkout
- Payment method is only displayed for supported regions

### Non-Functional

- Payment integration follows security best practices
- Sensitive payment information is not stored in the webshop


---

## US3206-hashed-password.md

#  **US3206 - MD5 Hashed password**

- Background: Due to a wrong implemented function, currently the password is stored in plain text.

## US3206-MD5 Hashed password

As a shop admin,
I want that the password is stored hashed with MD5
to improve the security of the shop.


## Acceptance Criteria:

UC1: Set/Change via UI
- at registration: set new password
- at profile: change password
- at login: change password
- at forgot password:


UC2: Convert existing passwords
- the existing passwords are converted to hashed MD5
to improve the security of the shop.

---

## US3409-replace-authlib.md

# US3409 - Replace Auth-Lib**

## Background: 
A security scan found a vulnerability X2499 in the used open-source library "Auth-X32". 
According to ISECC-2499 the lib should be replaced with the lib "Auth-XX64"



## US3409-Fix vulnerability in "Auth-X32"
As the security officer
I want that the used open-source library "xauth-X32" will be replaced with the lib "xauth-X64"
to close the security vulnerability ISECC-2499.


## ACC1: A vulnerability scan do not report an X2499 critical finding.

# Security Vulnerability: ISEC-2499

## Summary

**ISEC-2499** is a high-severity authentication vulnerability identified in the authentication library `xauth-X32`.

The vulnerability affects systems running:
- `xauth-X32`
- on Apple devices using **macOS Tahoe**

The issue can allow authentication session manipulation under specific runtime conditions caused by incompatible low-level memory handling in the legacy `xauth-X32` implementation.

---

## Severity

- Severity: **High**
- CVSS: `8.1`
- Affected Environment:
  - macOS Tahoe systems only

Other operating systems:
- Medium to Low impact
- No confirmed exploitation scenario currently known

---

## Technical Description

The `xauth-X32` library uses a legacy 32-bit session token handling mechanism that becomes unstable on macOS Tahoe due to changes in the operating system’s authentication runtime and memory isolation behavior.

Under specific conditions, attackers may:
- Reuse partially invalidated session tokens
- Trigger authentication state inconsistencies
- Bypass parts of the session validation flow
- Cause unauthorized session continuation

The issue is primarily observed:
- During concurrent authentication requests
- On long-running sessions
- When session refresh tokens are rotated

---

## Affected Components

- User authentication service
- Session management
- Token refresh handling
- Persistent login functionality

---

## Impact

Potential impacts include:
- Unauthorized account access
- Session hijacking scenarios
- Authentication instability
- Increased risk for privilege misuse

The vulnerability is considered especially critical for:
- E-commerce systems
- Systems handling personal data
- Public-facing authentication portals

---

## Mitigation

### Recommended Fix

Replace:
- `xauth-X32`

With:
- `xauth-X64`

The new `xauth-X64` version introduces:
- Updated 64-bit token handling
- Improved session isolation
- macOS Tahoe compatibility
- Hardened authentication lifecycle management
- Improved concurrency handling

---

## Additional Recommendations

- Force logout of all active sessions after deployment
- Rotate authentication secrets
- Monitor authentication logs for abnormal session reuse
- Perform regression testing for:
  - Login
  - Logout
  - Session timeout
  - Password reset
  - Multi-device login scenarios

---

## Risk Note

Systems not running macOS Tahoe currently show no confirmed exploitation path resulting in full compromise. However, due to the increasing adoption of Tahoe-based environments, remediation should be prioritized.

---

## US4700-new-step-login-in-checkout-flow.md

# New step (login) in checkout workflow**

## Background: 
Until now, when the user is not logged-in and runs the checkout workflow, he/she can not finish the workflow. 
This is very annoying for the user and many customer did not finished the check-out.


## US4700-Login during checkout workflow

As a user who is not logged in
I want the possibility to perform a login during the checkout workflow
to be able to finish the workflow.

## Acceptance Criteria:

### UC1: User is not logged-in
Given the user is not logged-in
And on the checkout-UI
When the user clicks "Proceed to Checkout"
Then the login dialogue will be displayed
And after a successful login the first checkout step will be displayed.


### UC2: User is logged-in
Given the user is logged-in
And on the checkout-UI
When the user clicks "Proceed to Checkout"
Then the first checkout step will be displayed.


---

## US5003-replace-ORMapper.md

# US5003 - Replace OR-Mapper

Background: Due to security and performance reasons the maintainance of ORMapper Versions 1.2 is deprecated. 
We must replace the version 1.2 with 2.0. In the version 1.2 fields was stored in the wrong database fields 
althougt the mapping configuration was correct.


## US5003-Replace-ORMapper

As the data engineer
I want that the actual ORMapper V1.2 will be replace with ORMapper V2.0
In order to improve the security and performance of the data access
and fix the mapping bug.


### Acceptance Criteria
- ACC1: All data structure are migrated to the new schema of V2.0.
- ACC2: Data structure ACCESS is denormalized.
- ACC3: A full table scan of reference table REF\_ORG\_Data (in PERF-ENV-Cloud-098) take < 1,5sec.
