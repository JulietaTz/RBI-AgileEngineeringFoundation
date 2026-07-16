---
name: review-test-artifact
description: Review a test artifact against its template, quality gate, traceability rules and source inputs.
---

# Review Test Artifact

## Procedure

1. Identify artifact type and version.
2. Load the matching template.
3. Load the applicable quality gate.
4. Check mandatory metadata.
5. Check completeness.
6. Check consistency with source inputs.
7. Check traceability.
8. Check for unsupported statements.
9. Classify findings.
10. Produce a review recommendation.

## Recommendation Values

- PASS
- CONDITIONAL PASS
- FAIL

## Mandatory Evidence

Every passed criterion must reference:

- a section,
- a table row,
- an identifier,
- or another explicit artifact location.