# Masking Strategy Rules

- Use **Anonymization** when:
  - Data must never be reversible
  - Used in non-production environments

- Use **Pseudonymization** when:
  - Relationships must be preserved
  - Data linkage is required

- Use **Tokenization** when:
  - Format must remain consistent
  - External mapping is acceptable

- Use **Synthetic Data** when:
  - Real data is too sensitive
  - Testing requires realistic distributions

- Use **Obfuscation / Generalization** when:
  - Business data needs protection but remains usable

---

## Decision Guidelines

- Always classify before masking
- Prefer stronger protection if uncertain
- Preserve referential integrity where required
- Consider combination risks across tables
- Treat identifiers as high-risk if linkable
- Assume GDPR applies unless stated otherwise

---

## Constraints

- NEVER expose real sensitive data
- ALWAYS justify masking decisions
- DO NOT skip classification
- PRIORITIZE data protection over convenience

