# ISO 29119 Test Plan - Release 1 (Stories 1.1-1.5)

## 1. Introduction

### 1.1 Purpose
This test plan defines scope, strategy, controls, and reporting for Release 1 of guided discovery capabilities (Stories 1.1 through 1.5). It is intended for QA, developers, product owner, and stakeholders approving release readiness.

### 1.2 Scope
**System under test**
- `sprint5/UI` (Angular guided discovery UI)
- `sprint5/API` (Laravel recommendation/session API)

**Features included (Release 1)**
- Story 1.1: Storefront entry and dual-rail navigation for guided discovery
- Story 1.2: Task prompt bar and contextual chips
- Story 1.3: Journey identifier and recommendation session API
- Story 1.4: Ranked recommendations with cold-start behavior
- Story 1.5: Alternative recommendations and refine loop

**Features excluded**
- Story 1.6 fallback parity and any Epic 2+ stories
- Explainability metadata surfaces beyond current release behavior
- Checkout attribution and advanced analytics pipelines

### 1.3 References
- `_bmad-output/planning-artifacts/prd.md`
- `_bmad-output/planning-artifacts/epics.md`
- `_bmad-output/planning-artifacts/architecture.md`
- `_bmad-output/planning-artifacts/ux-design-specification.md`
- `_bmad-output/implementation-artifacts/1-1-storefront-entry-and-dual-rail-navigation-for-guided-discovery.md`
- `_bmad-output/implementation-artifacts/1-2-task-prompt-bar-and-contextual-chips.md`
- `_bmad-output/implementation-artifacts/1-3-journey-identifier-and-recommendation-session-api.md`
- `_bmad-output/implementation-artifacts/1-4-ranked-recommendations-with-cold-start-behavior.md`
- `_bmad-output/implementation-artifacts/1-5-alternative-recommendations-and-refine-loop.md`
- `_bmad-output/test-artifacts/T1T5testDesign.md`
- ISO/IEC/IEEE 29119 (test documentation and process guidance)

---

## 2. Test Items

- Guided discovery shell and routing integration in Angular UI
- Task input form, contextual chips, validations, and search escape path
- Recommendation API client behavior and session bootstrap flow
- Backend recommendation endpoints:
  - `POST /recommendation-sessions`
  - `POST /recommendations`
- `X-Journey-Id` propagation and continuity behavior
- Ranked response contract (`journeyId`, `items`, `meta`) and cold-start/fallback metadata
- Refine and "more like this" loop actions (`mode`, `seedProductId`)
- Localization strings for guided discovery states (loading/error/recovery/refine controls)

Release target: first production-candidate build containing Stories 1.1-1.5.

---

## 3. Test Basis

- Functional requirements: FR1-FR8, FR25, FR27, FR28, FR60 (Release 1 relevant slice)
- Non-functional requirements: performance (<500 ms target), accessibility (WCAG 2.1 AA), reliability and graceful degradation
- Story acceptance criteria in Story 1.1-1.5 artifacts
- Architecture constraints:
  - single `RecommendationApiService` usage on UI
  - camelCase recommendation payload/response conventions
  - session continuity via `X-Journey-Id`
- Risk-based test design and T1-T5 scenario technique for core user flows

---

## 4. Test Objectives

- Verify Release 1 guided discovery flow works end-to-end from entry to recommendation refinement.
- Validate recommendation contract stability and backward compatibility through Story 1.5.
- Confirm journey/session continuity across repeated recommendation requests.
- Confirm recovery behavior avoids user dead ends (retry -> refine -> search).
- Mitigate key product risks:
  - weak first-match recovery fails,
  - contract drift between UI and API,
  - accessibility/interaction regressions in guided flows.
- Provide sufficient confidence for release decision with transparent residual risk documentation.

---

## 5. Test Approach

### 5.1 Overall Approach
**Test levels**
- Unit: service logic and UI component logic
- Integration/API: endpoint contract and journey-header behavior
- System/component UI: guided journey states and interactions
- Acceptance/regression: cross-story behavior from 1.1 to 1.5

**Test types**
- Functional (primary)
- Usability/accessibility checks on critical journeys
- Basic performance checks on recommendation response behavior
- Resilience/error-handling checks for recovery states

**Manual vs automation**
- Automated: API feature tests and UI component/service tests where available
- Manual: exploratory and cross-browser checks, keyboard/screen-reader focused checks

**Exploratory focus**
- Cold-start behavior transitions
- Recovery triad ordering and dead-end prevention
- Rapid interaction and repeated refine loops

### 5.2 Test Design Techniques
- T1-T5 scenario-based design (`_bmad-output/test-artifacts/T1T5testDesign.md`)
- Equivalence partitioning (task/chip payload variants)
- Boundary value checks (empty/invalid task input, missing optional fields)
- Decision table style checks for mode combinations (`default`, `refine`, `more_like_this`)
- State transition checks (loading -> success/error -> retry/refine/search)

### 5.3 Test Automation Strategy
**Tools/frameworks**
- Angular unit/component tests (Karma/Jasmine)
- Laravel feature tests (`php artisan test`)
- Lint/build gates (`ng lint`, `ng build`)

**Automation scope**
- Story-specific coverage for 1.2-1.5 behaviors
- API contract checks for session and recommendation endpoints
- Regression checks on recommendation response shape

**CI/CD integration**
- Mandatory: build + lint + automated tests for touched layers
- Known local environment blockers (missing browser binaries / missing PHP runtime) must be recorded with evidence; full test run must occur in CI-capable environment before release sign-off.

---

## 6. Test Deliverables

- Test plan: `_bmad-output/test-artifacts/testplan-rel1.md`
- Test design artifacts:
  - `_bmad-output/test-artifacts/test-design-epic-1.md`
  - `_bmad-output/test-artifacts/1-5-t1t5-test-cases.md`
- Automated test suites and updates in:
  - `sprint5/UI/src/app/**/*.spec.ts`
  - `sprint5/API/tests/Feature/*.php`
- Test data fixtures/factories in existing UI/API test patterns
- Execution reports (CI logs and summarized release test report)
- Defect reports in project issue tracking workflow

---

## 7. Test Environment

**Software**
- UI: Node/npm + Angular toolchain
- API: PHP/Laravel + test database + cache

**Tools**
- Angular CLI, Karma/Jasmine
- Laravel Artisan test runner
- Git-based CI pipeline for full validation

**Test data**
- Product data covering:
  - normal candidates,
  - low-signal/cold-start cases,
  - no-candidate scenarios,
  - seeded product affinities for "more like this".

**Configuration management**
- Source controlled in Git
- Story and test artifacts under `_bmad-output`
- Versioned build artifacts tied to release branch/tag

---

## 8. Organizational Aspects

### 8.1 Roles and Responsibilities
- **Test Lead / QA owner:** planning, coverage review, gate recommendation
- **Test Engineers:** test design, execution, defect triage support
- **Developers (UI/API):** unit/integration implementation and bug fixes
- **Product Owner:** acceptance and priority decisions
- **Stakeholders:** release readiness and risk acceptance decisions

### 8.2 Communication
- Daily development sync for defect and blocker updates
- Test status shared at least once per day during release validation window
- Formal go/no-go review for release sign-off
- Escalation path: Test Lead -> Tech Lead -> Product Owner -> Release decision forum

---

## 9. Test Schedule

- **Phase 1 - Preparation:** finalize scenarios, data, and environment readiness
- **Phase 2 - Story-level validation:** run 1.1-1.5 targeted checks
- **Phase 3 - Release regression:** full Release 1 regression sweep
- **Phase 4 - Closure:** defect triage, retest, final quality summary

**Dependencies**
- Stable candidate build for UI and API
- CI environment with browser binaries and PHP runtime
- Story statuses remain at least `review` and are frozen for release branching

**Timeline**
- Estimated effort window: ~4-7 working days for full release validation and reruns (depends on defect churn and environment readiness).

---

## 10. Test Control

### 10.1 Entry Criteria
- Stories 1.1-1.5 implemented and in `review` status
- Build and lint pass in target environment
- Required test environments available (including browser + PHP runtime)
- Test data prepared for normal, cold-start, error, and refinement scenarios
- No unresolved blocker defect preventing basic guided flow execution

### 10.2 Exit Criteria
- 100% pass for P0 scenarios across Release 1 scope
- >=95% pass for P1 scenarios
- No open Sev1/Sev2 defects in Release 1 scope
- Contract and journey continuity checks passed
- Residual risks documented and accepted by Product Owner + Tech Lead

### 10.3 Suspension and Resumption Criteria
**Suspend when**
- Environment instability invalidates results
- Critical dependency unavailable (API/session bootstrap broken globally)
- Defect rate indicates build not testable (repeated blocker failures)

**Resume when**
- Blocking issues are fixed and verified in a new candidate build
- Impacted test set is rerun with clean evidence

---

## 11. Risk Management

### 11.1 Product Risks
- Recommendation loop fails to improve results (trust/conversion risk)
- API contract drift across stories 1.3-1.5
- Recovery triad regression causing dead-end UX
- Accessibility regressions on guided controls/states
- Latency degradation in recommendation response path

### 11.2 Project Risks
- Local test execution blockers (missing browser binaries, missing PHP runtime)
- Tight release timeline with cross-layer change set (UI + API + docs)
- Incomplete OpenAPI regeneration in constrained environments

### 11.3 Risk Mitigation
- Prioritize P0 contract/session/recovery tests first
- Run full suites in CI-capable environment before release decision
- Use T1-T5 scenarios for end-user workflow confidence
- Document and explicitly accept residual risk where environment limits remain
- Enforce strict retest on any recommendation contract change

---

## 12. Incident Management

- Defects logged with severity, priority, reproduction steps, and evidence
- Severity classification:
  - **Sev1:** release blocker / critical path broken
  - **Sev2:** major feature degradation
  - **Sev3:** moderate defect with workaround
  - **Sev4:** minor/cosmetic issue
- Defect lifecycle: New -> Triaged -> In Progress -> Fixed -> Retest -> Closed
- Daily defect review during release test window

---

## 13. Configuration Management

- Version control: Git repository `practice-software-testing`
- Build management: release candidate builds from controlled branch/tag
- Test artifact management: `_bmad-output/test-artifacts` and CI logs
- Traceability: link test evidence to story IDs (1.1-1.5) and acceptance criteria

---

## 14. Metrics and Reporting

**Core metrics**
- Coverage of planned Release 1 scenarios (executed vs planned)
- Pass/fail rate by priority (P0/P1/P2)
- Defect counts by severity and status
- Reopen rate and fix verification cycle time
- Execution progress by story (1.1-1.5)

**Reporting cadence**
- Daily test status during active validation
- Final release quality report with go/no-go recommendation

**Stakeholders**
- QA/Test Lead, Tech Lead, Product Owner, release approvers

---

## 15. Approvals

| Name | Role | Approval Date | Status |
|---|---|---|---|
| TBD | Test Lead | TBD | Pending |
| TBD | Tech Lead | TBD | Pending |
| TBD | Product Owner | TBD | Pending |

---

# Appendix

## A. Glossary
- **Guided discovery:** AI-assisted intent capture and recommendation journey.
- **Recovery triad:** ordered recovery actions `retry -> refine -> search`.
- **Cold-start:** recommendation behavior without prior user history.
- **Journey ID:** correlation token (`X-Journey-Id`) for continuity and traceability.

## B. Test Data Strategy
- Use representative product catalog subsets for:
  - high-confidence match cases,
  - low-signal fallback cases,
  - no-candidate cases,
  - seeded alternatives for "more like this".
- Prefer generated/anonymized test data for compliance-safe execution.
- Ensure no production PII in automated test datasets.

## C. Tools
- **Test management/artifacts:** markdown artifacts under `_bmad-output/test-artifacts`
- **Automation UI:** Angular/Karma/Jasmine
- **Automation API:** Laravel feature tests
- **CI/CD:** project pipeline executing build/lint/test gates
- **Monitoring evidence:** structured logs and CI execution logs for release traceability

## D. Release 1 Traceability Matrix (Stories 1.1-1.5)

| Story | Primary Capability / AC Focus | FR/NFR Link | Planned Test IDs / Evidence |
|---|---|---|---|
| 1.1 | Guided entry and dual-rail navigation available and usable across breakpoints | FR1, FR25, UX-DR13, NFR15/NFR16 | R1-S11-UI-001 entry CTA visible; R1-S11-UI-002 dual-rail navigation behavior; R1-S11-UI-003 keyboard navigation and focus order |
| 1.2 | Task prompt, contextual chips, validation, and search escape | FR2, FR3, UX-DR1, UX-DR12 | R1-S12-UI-001 submit valid task; R1-S12-UI-002 chip toggle payload; R1-S12-UI-003 invalid task inline error; R1-S12-UI-004 use-search escape |
| 1.3 | Session bootstrap and `X-Journey-Id` continuity through API service | FR26, FR28, NFR20 | R1-S13-API-001 create session; R1-S13-API-002 resume session; R1-S13-INT-001 header propagation via `RecommendationApiService` |
| 1.4 | Ranked response with bounded items and cold-start/fallback behavior | FR4, FR5, FR8, NFR1 | R1-S14-API-001 ranked response contract (`journeyId`,`items`,`meta`); R1-S14-API-002 cold-start fallback reason; R1-S14-UI-001 loading skeleton; R1-S14-UI-002 error and retry state |
| 1.5 | Alternative/refine loop and recovery triad without contract/session regression | FR6, FR60, UX-DR11, NFR24 | `T1_recommendationLoop_initialRankedSetWithRefine_userGetsNewRankedSetAndSessionIsPreserved`; `T2_recommendationLoop_initialRankedSetWithMoreLikeThis_userGetsAlternativeRankedSetAroundSeedProduct`; `T3_recommendationLoop_apiFailureAfterInitialSet_userSeesRecoveryTriadAndCanRecoverWithRetryOrRefineOrSearch`; `T4_recommendationLoop_moreLikeThisWithoutValidSeedProductId_requestIsRejectedOrGracefullyHandledWithoutContractBreak`; `T5_recommendationLoop_repeatedRapidRefineAndAlternativeClicks_systemRemainsStableAndSessionConsistent` |

### Traceability Notes
- Detailed T1-T5 definitions and naming rationale are in `_bmad-output/test-artifacts/T1T5testDesign.md`.
- Story 1.5 concrete T1-T5 cases are maintained in `_bmad-output/test-artifacts/1-5-t1t5-test-cases.md`.
- Execution status for each test ID should be tracked in the release test report (Pass / Fail / Blocked / Not Run).
