---
artifact_id: QG02-TOOLSHOP-001
title: QG-02 Evidence - ToolShop Test Conditions
artifact_type: quality-gate-evidence
project: toolshop
version: 0.1
status: draft
created_by: test-designer
created_at: 2026-07-17T22:32:12+02:00
source_inputs:
  - quality-gates/qg-02-analysis-ready.md
  - projects/toolshop/working/test-conditions.md
  - projects/toolshop/working/reviews/test-conditions-review.md
assumptions:
  - A-01: Independent review REV-TCN-TOOLSHOP-001 (PASS) is the evidence basis for the independent-review criterion.
open_questions:
  - Q-01: SUT environment/version still unknown (carried forward; does not fail QG-02).
---

# QG-02: Test Analysis Ready — Evidence Record

## Evaluated Artifact

- Artifact: `projects/toolshop/working/test-conditions.md`
- Artifact ID: `TCN-TOOLSHOP-001`
- Version: `0.1`
- Status: `draft` (independently reviewed; not human-approved)

## Criteria Assessment

| # | Criterion | Result | Evidence |
|---|---|---|---|
| 1 | Every test condition has a unique ID. | PASS | IDs `TC-REG-001`–`TC-REG-009` unique in `TCN-TOOLSHOP-001` sections and Traceability Matrix; confirmed in `REV-TCN-TOOLSHOP-001` QG-1 |
| 2 | Every test condition references a source. | PASS | Each condition’s Source Requirement cites AC-1…AC-5 and/or `US-REG-001`; review US-1–US-5 / QG-2 |
| 3 | Critical and high product risks have explicit coverage. | PASS | Coverage Summary maps R-01–R-05; Critical priority on `TC-REG-004`, `TC-REG-005`, `TC-REG-009`; review QG-3 |
| 4 | Positive and negative conditions are considered. | PASS | Positive: `001`, `003`, `006`, `007`, `009`; Negative/exception: `002`, `004`, `005`, `008`; review QG-4 |
| 5 | Relevant boundaries and exceptions are considered. | PASS | Password BVA `TC-REG-005/006/007`; duplicate-email exception `TC-REG-004`; review QG-5 |
| 6 | Priorities are assigned. | PASS | Priority on all nine conditions; review QG-6 |
| 7 | Suggested techniques are reasonable. | PASS | EP, BVA, Error Guessing, Decision Table hint; review QG-7 |
| 8 | Duplicates are removed or justified. | PASS | `TC-REG-008` Notes justify intentional overlap; review QG-8 |
| 9 | Ambiguities are documented. | PASS | Front matter `open_questions`; condition Notes; “Explicitly Not Derived…”; review QG-9 |
| 10 | The artifact received an independent review. | PASS | `projects/toolshop/working/reviews/test-conditions-review.md` (`REV-TCN-TOOLSHOP-001`), recommendation **PASS**, no Critical/Major findings |

### Fail-Condition Check

| Fail condition | Triggered? | Evidence |
|---|---|---|
| Critical risks have no test condition | No | R-01 / R-02 / R-03 covered (`REV-TCN-TOOLSHOP-001` QG fail-check) |
| Conditions are not traceable | No | Coverage Summary + Traceability Matrix + Source Requirement fields |
| Invented requirements are used | No | “Explicitly Not Derived…” and Notes forbid invented syntax/complexity/message text |
| Independent review reports unresolved critical findings | No | No Critical findings in `REV-TCN-TOOLSHOP-001` |

## Decision

**PASS**

All mandatory QG-02 criteria are satisfied with evidence. Formal recording completes the pending analysis gate so Workflow 04 may proceed on the reviewed conditions.

## Decision Rule Applied

- PASS: all mandatory criteria are satisfied; no fail condition triggered.

## Limitations of This Gate Result

- This gate confirms analysis readiness only.
- It does **not** substitute human approval (QG-04).
- It does **not** authorise execution claims or automated code.
- Open questions Q-01 / Q-BA-01–05 remain open for design oracles.
