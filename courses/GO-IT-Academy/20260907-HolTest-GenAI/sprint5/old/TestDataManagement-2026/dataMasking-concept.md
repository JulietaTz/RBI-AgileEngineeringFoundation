# Data Masking Concept for dataModel.example.md

Date: 2026-04-12
Scope: `dataModel.example.md` in learning journey `lj-002-2026-TestDataManagement`

## 1. Objective
This concept defines how to classify and mask data in the example relational model so that:
- sensitive data is protected,
- datasets remain useful for development, testing, and analytics,
- referential integrity is preserved across all related tables.

## 2. Classification Scale
- Restricted: severe impact if disclosed (payment credentials, authentication secrets, security/trace data, sensitive finance)
- Confidential: high sensitivity and controlled access required (PII, commercially sensitive data)

Default rule: if classification is uncertain, classify as the higher level.

## 3. Detection and Inference Approach
Classification is based on content semantics and technical patterns, not only column names.

Signals used:
- format/pattern checks (email, PAN, IBAN, IP, hash-like values)
- uniqueness and cardinality (identifier behavior)
- distribution and entropy (credentials/secrets/high-risk tokens)
- relationship context (foreign key and join behavior)
- business semantics inferred from table role and value structure

## 4. Global Masking Architecture
### 4.1 Token Vault
Use deterministic tokenization with a secure vault for all linkable identifiers and high-risk values.

Vault schema:
- vault_domain (e.g., CUSTOMER_ID, EMAIL, PAYMENT_PAN)
- original_value
- token_value
- created_at
- rotation_version

Rules:
- one original value maps to one stable token per vault_domain
- domains are separated to prevent cross-domain inference
- vault access is restricted and audited

### 4.2 Referential Integrity Rules
- Tokenize all primary and foreign keys with deterministic mapping.
- Keep joinability across tables (same source value -> same token in same domain).
- Preserve nullability and uniqueness constraints after masking.

### 4.3 Environment Rules
- Non-production test environments: masking mandatory for all Confidential and Restricted fields.
- Restricted raw values are never exported.
- Production-like analytics copies use generalized or synthetic finance where possible.

## 5. Classification Report
Confidence scale:
- High (>= 0.90)
- Medium (0.75-0.89)

### 5.1 customers
| Column | Semantic Type | Sensitivity | Confidence | Masking Technique |
|---|---|---|---|---|
| customer_id | Customer identifier | Confidential | High | Deterministic tokenization |
| first_name | Person name | Confidential | High | Pseudonymization (name dictionary) |
| last_name | Person name | Confidential | High | Pseudonymization (name dictionary) |
| email | Email address | Confidential | High | Deterministic tokenization preserving email format |
| phone_number | Phone number | Confidential | High | Partial masking or tokenization |
| date_of_birth | Date of birth | Restricted | High | Generalization to year/month bucket |
| customer_segment | Segment label | Confidential | Medium | Keep or low-variance remap |
| lifetime_value | Customer financial metric | Restricted | High | Synthetic value generation preserving distribution |
| created_at | Technical timestamp | Confidential | Medium | Date shift (consistent per customer) |
| updated_at | Technical timestamp | Confidential | Medium | Date shift (consistent per customer) |

### 5.2 addresses
| Column | Semantic Type | Sensitivity | Confidence | Masking Technique |
|---|---|---|---|---|
| address_id | Address identifier | Confidential | High | Deterministic tokenization |
| customer_id | Customer FK | Confidential | High | Tokenization aligned with customers.customer_id |
| street | Street address | Confidential | High | Synthetic address generation by locale |
| city | City | Confidential | Medium | Generalization or synthetic replacement |
| postal_code | Postal code | Confidential | High | Format-preserving randomization |
| country | Country | Confidential | Medium | Keep or constrained remap |
| is_default | Preference flag | Confidential | Medium | Keep |
| created_at | Technical timestamp | Confidential | Medium | Date shift |

### 5.3 user_accounts
| Column | Semantic Type | Sensitivity | Confidence | Masking Technique |
|---|---|---|---|---|
| account_id | Account identifier | Confidential | High | Deterministic tokenization |
| customer_id | Customer FK | Confidential | High | Tokenization aligned with customers.customer_id |
| username | Login identifier | Restricted | High | Deterministic tokenization preserving uniqueness |
| password_hash | Authentication secret/hash | Restricted | High | Redaction or replacement with non-real test hash |
| last_login | Security event timestamp | Restricted | Medium | Date shift |
| failed_login_count | Security metric | Restricted | Medium | Keep or bounded perturbation |
| is_locked | Security state | Restricted | Medium | Keep |

### 5.4 products
| Column | Semantic Type | Sensitivity | Confidence | Masking Technique |
|---|---|---|---|---|
| product_id | Product identifier | Confidential | High | Deterministic tokenization |
| product_name | Product descriptor | Confidential | Medium | Keep or synthetic catalog names |
| description | Product description | Confidential | Medium | Keep or redact sensitive strings |
| category | Product category | Confidential | Medium | Keep |
| price | Commercial price | Confidential | High | Scaling/perturbation with business constraints |
| cost_price | Internal cost | Restricted | High | Synthetic with margin-consistency rules |
| margin | Margin percentage | Restricted | High | Recompute from masked price/cost |
| supplier_id | Supplier reference | Confidential | High | Tokenization aligned with suppliers.supplier_id |
| stock_quantity | Inventory count | Confidential | Medium | Keep or bounded perturbation |
| created_at | Technical timestamp | Confidential | Medium | Date shift |

### 5.5 suppliers
| Column | Semantic Type | Sensitivity | Confidence | Masking Technique |
|---|---|---|---|---|
| supplier_id | Supplier identifier | Confidential | High | Deterministic tokenization |
| supplier_name | Company name | Confidential | High | Pseudonymization |
| contact_email | Business email | Confidential | High | Deterministic tokenization preserving format |
| contract_terms | Contract text | Restricted | High | Redaction or synthetic clause templates |
| rating | Supplier rating | Confidential | Medium | Keep or small perturbation |
| created_at | Technical timestamp | Confidential | Medium | Date shift |

### 5.6 orders
| Column | Semantic Type | Sensitivity | Confidence | Masking Technique |
|---|---|---|---|---|
| order_id | Order identifier | Confidential | High | Deterministic tokenization |
| customer_id | Customer FK | Confidential | High | Tokenization aligned with customers.customer_id |
| order_date | Order timestamp | Confidential | High | Date shift preserving sequence |
| order_status | Status enum | Confidential | Medium | Keep |
| total_amount | Revenue metric | Restricted | High | Synthetic generation with constraints |
| discount_amount | Discount metric | Confidential | High | Synthetic generation |
| tax_amount | Tax metric | Confidential | High | Synthetic generation by rate bands |
| net_amount | Derived amount | Restricted | High | Recompute from masked components |
| profit_amount | Profit metric | Restricted | High | Recompute from masked components |
| currency | Currency code | Confidential | Medium | Keep |
| sales_channel | Channel enum | Confidential | Medium | Keep |
| shipping_address_id | Address FK | Confidential | High | Tokenization aligned with addresses.address_id |
| billing_address_id | Address FK | Confidential | High | Tokenization aligned with addresses.address_id |

### 5.7 order_items
| Column | Semantic Type | Sensitivity | Confidence | Masking Technique |
|---|---|---|---|---|
| order_item_id | Item identifier | Confidential | High | Deterministic tokenization |
| order_id | Order FK | Confidential | High | Tokenization aligned with orders.order_id |
| product_id | Product FK | Confidential | High | Tokenization aligned with products.product_id |
| quantity | Quantity | Confidential | Medium | Keep or bounded perturbation |
| price_per_unit | Unit price | Confidential | High | Synthetic/perturbed |
| cost_per_unit | Internal unit cost | Restricted | High | Synthetic |
| discount | Discount | Confidential | Medium | Synthetic |
| total_price | Derived value | Confidential | High | Recompute |
| total_cost | Derived internal cost | Restricted | High | Recompute |
| item_profit | Derived profit | Restricted | High | Recompute |

### 5.8 payments
| Column | Semantic Type | Sensitivity | Confidence | Masking Technique |
|---|---|---|---|---|
| payment_id | Payment identifier | Confidential | High | Deterministic tokenization |
| order_id | Order FK | Confidential | High | Tokenization aligned with orders.order_id |
| payment_method | Method enum | Confidential | Medium | Keep |
| card_holder_name | Payment PII | Restricted | High | Tokenization or pseudonymization |
| card_number | PAN/card number | Restricted | High | Format-preserving tokenization (keep last4 if required) |
| card_expiry_date | Card expiry | Restricted | High | Format-preserving synthetic value |
| card_cvv | Card secret | Restricted | High | Full redaction (never retain) |
| iban | Bank account identifier | Restricted | High | Format-preserving tokenization |
| payment_status | Status enum | Confidential | Medium | Keep |
| payment_provider | Provider name | Confidential | Medium | Keep |
| transaction_fee | Financial metric | Confidential | High | Synthetic/perturbed |
| payment_date | Payment timestamp | Confidential | High | Date shift |

### 5.9 shipments
| Column | Semantic Type | Sensitivity | Confidence | Masking Technique |
|---|---|---|---|---|
| shipment_id | Shipment identifier | Confidential | High | Deterministic tokenization |
| order_id | Order FK | Confidential | High | Tokenization aligned with orders.order_id |
| tracking_number | Shipment tracker | Confidential | High | Format-preserving tokenization |
| carrier | Carrier name | Confidential | Medium | Keep or remap |
| shipping_cost | Logistics cost | Restricted | High | Synthetic/perturbed |
| shipped_date | Shipment timestamp | Confidential | Medium | Date shift |
| delivery_date | Delivery timestamp | Confidential | Medium | Date shift preserving lead time |
| status | Shipment status | Confidential | Medium | Keep |

### 5.10 reviews
| Column | Semantic Type | Sensitivity | Confidence | Masking Technique |
|---|---|---|---|---|
| review_id | Review identifier | Confidential | High | Deterministic tokenization |
| product_id | Product FK | Confidential | High | Tokenization aligned with products.product_id |
| customer_id | Customer FK | Confidential | High | Tokenization aligned with customers.customer_id |
| rating | Product rating | Confidential | Medium | Keep |
| comment | Free text (may contain PII) | Restricted | High | NLP redaction + synthetic paraphrase |
| sentiment_score | Derived metric | Confidential | Medium | Recompute from masked comment or keep bounded |
| created_at | Timestamp | Confidential | Medium | Date shift |

### 5.11 user_sessions
| Column | Semantic Type | Sensitivity | Confidence | Masking Technique |
|---|---|---|---|---|
| session_id | Session identifier | Confidential | High | Deterministic tokenization |
| customer_id | Customer FK | Confidential | High | Tokenization aligned with customers.customer_id |
| ip_address | Network identifier | Restricted | High | Tokenization or subnet-preserving anonymization |
| user_agent | Device/browser fingerprint data | Restricted | High | Generalization to family/version bucket |
| session_start | Session timestamp | Confidential | Medium | Date shift |
| session_end | Session timestamp | Confidential | Medium | Date shift preserving duration |
| conversion_flag | Behavior flag | Confidential | Medium | Keep |

### 5.12 wishlist
| Column | Semantic Type | Sensitivity | Confidence | Masking Technique |
|---|---|---|---|---|
| wishlist_id | Wishlist identifier | Confidential | High | Deterministic tokenization |
| customer_id | Customer FK | Confidential | High | Tokenization aligned with customers.customer_id |
| product_id | Product FK | Confidential | High | Tokenization aligned with products.product_id |
| added_at | Timestamp | Confidential | Medium | Date shift |

### 5.13 sales_metrics
| Column | Semantic Type | Sensitivity | Confidence | Masking Technique |
|---|---|---|---|---|
| metric_id | Metric identifier | Confidential | High | Deterministic tokenization |
| date | Reporting date | Confidential | Medium | Date shift or period aggregation |
| total_revenue | Strategic financial KPI | Restricted | High | Synthetic generation |
| total_profit | Strategic financial KPI | Restricted | High | Synthetic generation |
| total_orders | KPI | Confidential | Medium | Synthetic scaling |
| average_order_value | KPI | Confidential | Medium | Recompute from synthetic values |
| conversion_rate | KPI | Confidential | Medium | Keep banded value |
| customer_acquisition_cost | Strategic KPI | Restricted | High | Synthetic generation |

### 5.14 product_performance
| Column | Semantic Type | Sensitivity | Confidence | Masking Technique |
|---|---|---|---|---|
| performance_id | Performance identifier | Confidential | High | Deterministic tokenization |
| product_id | Product FK | Confidential | High | Tokenization aligned with products.product_id |
| date | Reporting date | Confidential | Medium | Date shift |
| units_sold | KPI | Confidential | Medium | Synthetic scaling |
| revenue | Financial KPI | Restricted | High | Synthetic generation |
| profit | Financial KPI | Restricted | High | Synthetic generation |
| return_rate | KPI | Confidential | Medium | Keep banded value |

## 6. Masking Decision Log
| Data Pattern / Type | Selected Technique | Justification |
|---|---|---|
| Primary keys and foreign keys | Deterministic tokenization | Maintains joins and referential integrity across all tables |
| Names (person/company) | Pseudonymization | Keeps realism and linguistic usability while hiding identities |
| Email | Format-preserving tokenization | Protects identity and preserves application validation behavior |
| Phone number | Partial mask or tokenization | Protects contact data and keeps format tests functional |
| Date of birth | Generalization | Reduces re-identification risk from exact birth dates |
| Password hash, CVV, contract text with secrets | Redaction or safe replacement | Prevents any recovery of secrets; strongest protection |
| PAN/IBAN/tracking/IP | Format-preserving tokenization | High-risk identifiers require strong reversible control in vault |
| Free-text comments | NLP redaction + synthetic rewrite | Removes latent PII leaks hidden in unstructured text |
| Revenue/cost/profit metrics | Synthetic generation + constrained recomputation | Preserves analytical usefulness without exposing true business values |
| Timestamps | Consistent date shift | Preserves event ordering and durations for test scenarios |

## 7. Utility Constraints for Test Data
- Keep uniqueness where required (`email`, `username`, keys).
- Keep valid domain formats (email, IBAN, PAN-like lengths, postal code patterns).
- Recompute dependent fields to keep arithmetic consistency:
  - `net_amount = total_amount - discount_amount + tax_amount`
  - `profit_amount = net_amount - total_cost_basis` (project-specific basis)
  - `item_profit = total_price - total_cost`
- Preserve timeline logic:
  - `order_date <= shipped_date <= delivery_date`
  - `session_start <= session_end`

## 8. Example Masked Dataset (Illustrative)
Original values are not shown. Tokens are examples only.

### customers (masked sample)
| customer_id | first_name | last_name | email | phone_number | date_of_birth | customer_segment | lifetime_value |
|---|---|---|---|---|---|---|---|
| TKN_CUST_0001 | Liam | Carter | tkn_mail_0001@example.test | +43-***-***-210 | 1988-01-01 | VIP | 12450.30 |
| TKN_CUST_0002 | Nora | Hayes | tkn_mail_0002@example.test | +43-***-***-778 | 1993-01-01 | Regular | 2980.40 |

### orders (masked sample)
| order_id | customer_id | order_date | total_amount | discount_amount | tax_amount | net_amount | profit_amount |
|---|---|---|---|---|---|---|---|
| TKN_ORD_1001 | TKN_CUST_0001 | 2025-06-10 10:21:00 | 210.00 | 10.00 | 38.00 | 238.00 | 54.00 |
| TKN_ORD_1002 | TKN_CUST_0002 | 2025-06-13 15:05:00 | 99.00 | 0.00 | 17.82 | 116.82 | 20.10 |

### payments (masked sample)
| payment_id | order_id | card_holder_name | card_number | card_expiry_date | card_cvv | iban |
|---|---|---|---|---|---|---|
| TKN_PAY_5001 | TKN_ORD_1001 | TKN_NAME_2001 | 400000******1234 | 09/29 | [REDACTED] | AT12TKN000000000001 |
| TKN_PAY_5002 | TKN_ORD_1002 | TKN_NAME_2002 | 555555******8899 | 03/30 | [REDACTED] | AT12TKN000000000002 |

## 9. Implementation Checklist
- Build classification metadata table (`table`, `column`, `semantic_type`, `sensitivity`, `technique`, `rule_version`).
- Implement token vault service with domain-separated deterministic mapping.
- Apply masking pipeline in this order:
  1) keys/FKs, 2) direct identifiers, 3) sensitive text/secrets, 4) financial synthesis, 5) derived-field recomputation, 6) integrity validation.
- Add automated quality gates:
  - no Restricted plaintext leakage,
  - foreign key integrity = 100%,
  - uniqueness constraints preserved,
  - numeric consistency checks pass.

## 10. Final Policy Statement
For this data model, all PII and business-critical financial/security fields are masked by default. Restricted data is strongly protected via redaction, format-preserving tokenization, or synthetic substitution. Confidential data is protected while preserving test realism and relational integrity.
