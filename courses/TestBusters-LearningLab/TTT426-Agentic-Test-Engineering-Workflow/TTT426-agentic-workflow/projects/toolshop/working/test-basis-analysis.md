---
artifact_id: TBA-TOOLSHOP-001
title: ToolShop Test Basis Analysis - Registration Increment
artifact_type: test-basis-analysis
project: toolshop
version: 0.1
status: draft
created_by: test-analyst
created_at: 2026-07-17T14:07:28+02:00
source_inputs:
  - projects/toolshop/working/test-context.md
  - projects/toolshop/input/registration-user-story.md
  - projects/toolshop/input/product-description.md
  - projects/toolshop/working/reviews/qg-01-context-ready-evidence.md
  - skills/analyze-test-basis/SKILL.md
assumptions:
  - A-01: AC-1–AC-5 are the authoritative functional expectations until a newer approved requirement artifact exists (from TCX A-04).
  - A-02: Increment scope remains customer registration only (from TCX A-01 / A-05).
  - A-03: Human request to run Workflow 03 answers Q-04 for this increment (planning deferred).
open_questions:
  - Q-01: Which ToolShop environment/version is the SUT?
  - Q-02: Who are the named human stakeholders/approvers?
  - Q-03: Are additional legal/privacy/security constraints required?
  - Q-BA-01: What email format/syntax rules apply (if any beyond uniqueness)?
  - Q-BA-02: Are there password rules beyond minimum length of eight characters?
  - Q-BA-03: What constitutes “invalid input” for first name, last name, email and password?
  - Q-BA-04: What exact error message content, placement and persistence are expected?
  - Q-BA-05: Is login verification limited to credential acceptance after registration, or does it include session/navigation behaviour?
---

# Test Basis Analysis

## 1. Analysis Scope

Analyze the registration test basis for the ToolShop first increment so that
traceable test conditions can be derived.

In scope: customer registration as defined in
`projects/toolshop/input/registration-user-story.md` and scoped by
`TCX-TOOLSHOP-001`.

Out of scope for this analysis: inventing UI layout details, inventing
non-documented validation rules, browse/catalogue, checkout/orders, and
detailed test case design.

## 2. Input Validation

| Required Input | Status | Notes |
|---|---|---|
| Test context | Available | `TCX-TOOLSHOP-001` v0.1; QG-01 = PASS |
| In-scope requirements | Available | Registration user story + AC-1–AC-5 |
| Product risk analysis | Partial / missing formal artifact | Only preliminary risks R-01–R-05 in test context |
| Applicable policies | Missing / empty | `knowledge/test-policy.md` has no usable content |
| Terminology | Empty | `instructions/terminology.md` empty; use TTT426 agent definitions |

Non-blocking for draft analysis: missing formal PRA and empty policy are
documented; preliminary risks from the reviewed context are used and labelled.

## 3. Requirement Decomposition

| Requirement ID | Source Statement | Actor | Trigger | Conditions | Action | Observable Outcome |
|---|---|---|---|---|---|---|
| US-REG-001 | Visitor creates a customer account to order products | Visitor | Chooses to register | Visitor not yet using a customer account for ordering | Create customer account | Account creation path exists; business goal is enablement of ordering (ordering itself out of scope) |
| AC-1 | Register with first name, last name, email and password | Visitor | Submits registration | Required fields provided | Accept registration input set | Registration can be completed using those four fields |
| AC-2 | Email address must be unique | System | Registration with an email | Email already used by an existing account | Reject or prevent duplicate registration | Duplicate email is not accepted as a new unique account |
| AC-3 | Password must contain at least eight characters | System | Registration with password | Password length evaluated | Enforce minimum length | Passwords shorter than eight characters are not accepted |
| AC-4 | After successful registration, the user can log in | Registered user | Attempts login after successful registration | Registration completed successfully | Authenticate with registered credentials | Login succeeds with the newly created credentials |
| AC-5 | Invalid input is rejected and an error message is displayed | System | Registration with invalid input | Input classified as invalid | Reject submission and show error | Registration fails; an error message is displayed |

## 4. Facts / Assumptions / Questions

### Facts (explicit in sources)

- Product under test is ToolShop; first increment focuses on customer registration.
- Registration fields named in AC-1: first name, last name, email, password.
- Email uniqueness is required (AC-2).
- Password minimum length is eight characters (AC-3).
- Successful registration must enable subsequent login (AC-4).
- Invalid input must be rejected with an error message (AC-5).
- QG-01 for the test context passed; context remains draft (not human-approved).

### Assumptions (not approved requirements)

- A-01–A-03 as listed in front matter.
- Preliminary product risks R-01–R-05 from `TCX-TOOLSHOP-001` are used as the risk basis until a formal product risk analysis exists.

### Open Questions

See front matter `open_questions` (Q-01–Q-03 from context; Q-BA-01–Q-BA-05 from this analysis).

## 5. Analysis Findings

| Requirement ID | Finding Type | Finding | Severity | Test Impact | Proposed Clarification |
|---|---|---|---|---|---|
| AC-1 | Ambiguity | Required fields are named, but allowed character sets, max lengths, mandatory/optional nuances and empty-value treatment are not specified. | Medium | Boundary and negative conditions for names/email must stay provisional; do not invent format rules. | Define per-field validation rules (empty, whitespace-only, max length, allowed characters). |
| AC-1 | Missing information | No statement whether additional registration fields exist beyond the four named fields. | Low | Design must not assert presence/absence of extra UI fields. | Confirm whether only these four fields are required and sufficient. |
| AC-2 | Ambiguity | “Unique” does not define comparison rules (case sensitivity, trimming, alias normalisation). | High | Duplicate-email conditions need an explicit comparison assumption or clarification before verdicts. | Specify uniqueness matching rules for email. |
| AC-2 | Missing information | No email syntax/format rule is stated (only uniqueness). | High | Email-format negative tests would invent requirements if treated as mandatory. | Confirm whether RFC-like / application-specific email format rules apply. |
| AC-3 | Ambiguity | Only a lower bound (“at least eight”) is given; no upper bound, complexity, allowed characters, or whitespace rules. | High | BVA can cover length 7/8; complexity/charset tests remain out of evidence-based scope. | Confirm max length, complexity and character-set rules, if any. |
| AC-4 | Ambiguity | “Can log in” does not define success criteria (landing page, session, tokens) or timing. | Medium | Post-registration login check should verify credential acceptance only unless clarified. | Define observable login success criteria for this increment. |
| AC-4 | Missing information | No statement on whether auto-login after registration is required or whether manual login is expected. | Low | Avoid inventing auto-login behaviour in conditions/cases. | Clarify expected post-registration authentication flow. |
| AC-5 | Ambiguity | “Invalid input” is undefined relative to AC-1–AC-3 (which invalid cases beyond short password / duplicate email?). | High | Negative set may under- or over-cover without a definition of invalid. | Enumerate invalid classes per field and combinations. |
| AC-5 | Ambiguity | Error message content, location, language and persistence are unspecified (aligns with context R-05). | Medium | Pass/fail on message text/location would be subjective without clarification. | Provide expected messages or acceptance rule (presence of actionable error is enough). |
| AC-5 | Missing information | No distinction between field-level vs form-level errors, or multiple simultaneous errors. | Low | Multi-error behaviour should not be asserted. | Clarify single vs multiple error presentation. |
| US-REG-001 / Product | Missing information | SUT environment/version URL not fixed (context Q-01). | High for execution | Analysis can proceed; execution design remains blocked until SUT is fixed. | Nominate ToolShop version/environment. |
| TB-03 / Policy | Missing information | Test policy file empty; no organisational technique or coverage mandates available. | Medium | Technique recommendations are analyst judgement, not policy-driven. | Provide usable test policy content. |
| Product risks | Missing information | No formal product risk analysis artifact; only preliminary risks R-01–R-05. | Medium | Risk linkage uses preliminary IDs; risk class may change after formal PRA. | Complete Workflow 02 / formal product risk analysis if required. |
| Cross-source | Contradiction | None detected between product description, user story and test context for registration scope. | — | No contradiction-driven conditions. | — |

## 6. Testability Summary

| Area | Testable now? | Comment |
|---|---|---|
| Happy-path registration with four fields | Yes | Observable account creation / successful registration outcome needed at design time |
| Email uniqueness | Yes (behaviour) | Matching rules still ambiguous |
| Password minimum length | Yes | Clear numeric lower bound supports BVA |
| Login after registration | Partially | Credential-based login check is testable; UI success details unclear |
| Invalid input + error message | Partially | Rejection + message presence testable; message content/location unclear |
| Email syntax | No (as mandatory rule) | Not specified — do not invent |
| Password complexity | No | Not specified — do not invent |

## 7. Recommended Next Step Within Workflow 03

1. Derive test conditions from this analysis and preliminary risks R-01–R-05.
2. Request independent review of the test conditions.
3. Run QG-02 after review evidence exists.
4. Hand over to Test Designer only after QG-02 readiness is established.
