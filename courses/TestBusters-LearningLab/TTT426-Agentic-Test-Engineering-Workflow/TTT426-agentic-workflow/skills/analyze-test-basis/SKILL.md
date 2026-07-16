---
name: analyze-test-basis
description: Analyze requirements and other test basis documents for testability, ambiguity, contradiction and missing information.
---

# Analyze Test Basis

## Use This Skill When

- a new requirement or user story enters the workflow,
- test conditions must be derived,
- the quality of the test basis is unknown,
- requirements changed.

## Required Inputs

- requirement or user story
- acceptance criteria
- relevant business rules
- known constraints

## Procedure

1. Identify each explicit requirement statement.
2. Assign or preserve a unique requirement identifier.
3. Identify actors, triggers, conditions, actions and outcomes.
4. Check whether the expected behaviour is observable.
5. Check for ambiguous wording.
6. Check for missing boundaries and error behaviour.
7. Check for contradictions with other sources.
8. Identify assumptions required for testing.
9. Separate facts, assumptions and questions.
10. Produce an analysis table.

## Output Format

| Requirement ID | Finding Type | Finding | Severity | Test Impact | Proposed Clarification |
|---|---|---|---|---|---|

## Quality Criteria

- No invented requirement details.
- Every finding references its source.
- Ambiguities are explained, not only labelled.
- Test impact is stated.