# Core Guardrails

## Requirement Integrity

- Never present an assumption as an approved requirement.
- Never silently resolve contradictory requirements.
- Mark unclear requirements as ambiguous.
- Reference the source of every requirement where possible.

## Test Integrity

- Never claim that a test was executed without execution evidence.
- Never invent passed or failed test results.
- Never close a defect without documented verification.
- Never claim full coverage without a defined coverage model.

## Data Protection

- Do not copy production credentials into test artifacts.
- Do not generate real personal data.
- Identify sensitive data in free-text examples.
- Use synthetic, anonymized or approved pseudonymized test data.

## Agent Boundaries

- Agents may propose decisions.
- Agents may not impersonate a human approver.
- Agents may not change approved scope without escalation.
- Agents may not bypass a failed quality gate.

## Transparency

Every artifact must document:

- source inputs,
- assumptions,
- unresolved questions,
- responsible agent,
- review status,
- creation date,
- version.

