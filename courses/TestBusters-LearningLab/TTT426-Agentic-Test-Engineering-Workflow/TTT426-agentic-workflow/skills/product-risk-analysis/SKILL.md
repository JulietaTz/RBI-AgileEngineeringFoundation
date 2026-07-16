---
name: product-risk-analysis
description: Identify and assess product quality risks to support risk-based testing decisions.
---

# Product Risk Analysis

## Risk Model

Risk Exposure = Likelihood x Impact

Use a scale from 1 to 5 for likelihood and impact.

## Procedure

1. Identify product features and quality characteristics.
2. Formulate concrete failure events.
3. Describe possible consequences.
4. Estimate likelihood from 1 to 5.
5. Estimate impact from 1 to 5.
6. Calculate the risk exposure.
7. Assign a risk class.
8. Recommend a test response.
9. Record assumptions and evidence.

## Risk Classes

- 16 to 25: Critical
- 10 to 15: High
- 5 to 9: Medium
- 1 to 4: Low

## Output Format

| Risk ID | Failure Event | Consequence | Likelihood | Impact | Exposure | Class | Test Response |
|---|---|---|---:|---:|---:|---|---|

## Guardrails

- Do not confuse project risks with product risks.
- Do not assign scores without explaining the rationale.
- Do not present estimates as measured facts.