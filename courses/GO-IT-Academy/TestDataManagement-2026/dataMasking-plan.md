# Data Masking Concept for Sprint 5 Datamodel

## Overview
This document outlines the data masking strategy for all tables in the sprint 5 datamodel, ensuring compliance with GDPR and best practices for data privacy and protection.

## Masking Principles
- **PII/SPII**: Always masked using strong techniques (anonymization, pseudonymization, redaction).
- **Business Sensitive**: Masked using tokenization or synthetic data where possible.
- **Non-Sensitive**: No masking required.
- **Referential Integrity**: Tokenization must be consistent across tables for linked IDs.
- **Synthetic Data**: Used for financials and metrics to avoid leakage of real values.

## Masking Techniques
- **Anonymization**: Irreversible replacement (e.g., emails → anon@domain.com)
- **Pseudonymization**: Reversible with a key, preserves relationships (e.g., names → NameX)
- **Tokenization**: Consistent mapping for IDs (e.g., UUID → tok_001)
- **Redaction**: Remove or blank out (e.g., phone → [REDACTED])
- **Generalization**: Reduce precision (e.g., DOB → year only)
- **Synthetic Data**: Replace with realistic but fake values

## Example Table (customers)
| Column           | Classification | Masking | Technique         |
|------------------|---------------|---------|------------------|
| customer_id      | Business Sensitive | Yes    | Tokenization      |
| first_name       | PII           | Yes    | Pseudonymization  |
| last_name        | PII           | Yes    | Pseudonymization  |
| email            | PII           | Yes    | Anonymization     |
| phone_number     | PII           | Yes    | Redaction         |
| date_of_birth    | PII           | Yes    | Generalization    |
| customer_segment | Non-Sensitive | No     | -                |
| lifetime_value   | Business Sensitive | Yes    | Synthetic Data    |
| created_at       | Non-Sensitive | No     | -                |
| updated_at       | Non-Sensitive | No     | -                |

## Risk Notes
- Consistent tokenization is required for all UUIDs to maintain referential integrity.
- Masking must prevent re-identification and linkage attacks.
- Password hashes and contract terms must never be exposed.

## Compliance
This strategy is designed to meet GDPR and DORA requirements for data minimization and protection.