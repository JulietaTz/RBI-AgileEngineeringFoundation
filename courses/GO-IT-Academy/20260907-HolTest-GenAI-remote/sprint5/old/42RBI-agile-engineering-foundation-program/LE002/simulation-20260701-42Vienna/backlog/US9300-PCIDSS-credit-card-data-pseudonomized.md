# User Story: Pseudonymize Stored Credit Card Data (PCI-DSS)

## Estimated:

## US9300 — Credit card data stored pseudonymized per PCI-DSS

As a security officer  
I want credit card data in ToolShop to be stored in a pseudonymized form  
So that the platform meets PCI-DSS requirements and reduces the risk of cardholder data exposure.

## Description

ToolShop accepts credit card payments during checkout. Today, payment records may persist sensitive cardholder data in plain or recoverable form (for example full PAN, cardholder name, expiry date, or CVV). PCI-DSS requires that stored cardholder data is protected; full PAN must not be kept unless strictly necessary, and sensitive authentication data such as CVV must never be stored after authorization.

This story introduces pseudonymization for any credit card fields that must remain in the database for order reconciliation, support, or audit. Pseudonymized values must not allow reconstruction of the original card data without access to a separate, secured tokenization vault or key.

## Acceptance Criteria

### Functional

- Credit card checkout continues to work for customers using the existing payment flow
- Stored payment records remain linkable to the corresponding order and customer account
- Authorized staff can still identify a payment method type and the last four digits of the card for support purposes

### Non-Functional

- Implementation aligns with PCI-DSS requirements for protection of stored cardholder data
- Pseudonymization is applied consistently across API, database, logs, and exports
- Reversible mapping between pseudonym and original PAN is restricted to a dedicated secure component

### Acceptance criteria

- TBD

## Reference

- [PCI Security Standards Council — PCI DSS](https://www.pcisecuritystandards.org/)
- ToolShop checkout payment flow (`POST /payment/check`, credit card method)
- Related backlog item: US1022 — Compliance Ready

