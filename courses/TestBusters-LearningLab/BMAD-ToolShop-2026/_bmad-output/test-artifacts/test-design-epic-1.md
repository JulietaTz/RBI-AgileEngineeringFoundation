---
workflowStatus: completed
totalSteps: 5
stepsCompleted:
  - step-01-detect-mode
  - step-02-load-context
  - step-03-risk-and-testability
  - step-04-coverage-plan
  - step-05-generate-output
lastStep: step-05-generate-output
nextStep: ''
lastSaved: '2026-04-20T14:00:00Z'
---

# Test Design: Epic 1 - Story 1.5 Focus

**Date:** 2026-04-20  
**Author:** Rudolfgroetz / Murat (TEA)  
**Status:** Draft

---

## Executive Summary

**Scope:** Epic-level test design focused on Story 1.5 (`alternative recommendations and refine loop`) and regression protection for Stories 1.4/1.3.

**Risk Summary:**

- Total risks identified: 6
- High-priority risks (score >= 6): 3
- Critical categories: BUS, TECH, OPS

**Coverage Summary:**

- P0 scenarios: 6 (~14-20 hours)
- P1 scenarios: 8 (~12-20 hours)
- P2/P3 scenarios: 6 (~4-10 hours)
- **Total effort**: ~30-50 hours (~4-7 working days)

---

## Not in Scope

| Item | Reasoning | Mitigation |
|---|---|---|
| Full fallback flow ownership (`1.6`) | Explicitly separated story scope | Add regression checks that search escape path still works |
| Explainability confidence/source UI (`Epic 2`) | Not part of Story 1.5 ACs | Keep response contract stable for future explainability fields |
| Checkout attribution | Epic 4 scope | Preserve `X-Journey-Id` continuity for downstream attribution |

---

## Risk Assessment

### High-Priority Risks (Score >=6)

| Risk ID | Category | Description | Probability | Impact | Score | Mitigation | Owner | Timeline |
|---|---|---|---:|---:|---:|---|---|---|
| R-001 | BUS | Refine/alternative actions do not meaningfully change ranking; user trust drops after weak first set | 3 | 2 | 6 | Add mode-aware API tests (`refine`, `more_like_this`) and UI assertions for set replacement | QA + Dev | Story 1.5 completion |
| R-002 | TECH | Request schema drift between UI and API for `mode`/`seedProductId` causes silent fallback behavior | 2 | 3 | 6 | Lock request schema in OpenAPI + typed model + feature tests | Dev | Story 1.5 completion |
| R-003 | OPS | Recovery triad ordering regresses in error states and creates dead-end UX | 2 | 3 | 6 | Explicit UI test for retry -> refine -> search order and accessibility-safe rendering | QA | Story 1.5 completion |

### Medium-Priority Risks (Score 3-4)

| Risk ID | Category | Description | Probability | Impact | Score | Mitigation | Owner |
|---|---|---|---:|---:|---:|---|---|
| R-004 | DATA | `seedProductId` invalid/missing leads to unstable ranking behavior | 2 | 2 | 4 | Add backend tests for invalid seed inputs and deterministic fallback | Dev |
| R-005 | PERF | Repeated refine loops add latency and hurt perceived responsiveness | 2 | 2 | 4 | Keep list bounded to <=3 and monitor response times in profiling evidence | Dev |

### Low-Priority Risks (Score 1-2)

| Risk ID | Category | Description | Probability | Impact | Score | Action |
|---|---|---|---:|---:|---:|---|
| R-006 | SEC | Loop fields introduce minor input-surface expansion (`mode`, `seedProductId`) | 1 | 2 | 2 | Validate enum/string inputs and rely on existing auth/rate-limits |

### Residual Risk

Because browser and PHP runtime execution are currently blocked in this environment, full automated runtime confirmation remains pending even though build/lint and type checks pass.

---

## Entry Criteria

- [ ] Story 1.5 artifact is `ready-for-dev`
- [ ] Story 1.4 response contract (`items`, `meta`) remains baseline
- [ ] `RecommendationApiService` is the only recommendation HTTP path in UI
- [ ] OpenAPI source is editable in API controller annotations

## Exit Criteria

- [ ] All P0 scenarios pass
- [ ] P1 pass rate >=95%
- [ ] No open high-risk unresolved items (R-001..R-003)
- [ ] Request contract and response contract documented and backward-compatible

---

## Test Coverage Plan

> Priority P0/P1/P2/P3 indicates business/risk priority, not execution timing.

### P0 (Critical)

**Criteria:** Core recovery loop behavior + high-risk + no acceptable workaround.

| Test ID | Requirement | Test Level | Risk Link | Notes |
|---|---|---|---|---|
| 1.5-API-001 | Accept `mode=refine` and return stable response contract | API | R-001,R-002 | Include contract shape assertions |
| 1.5-API-002 | Accept `mode=more_like_this` + `seedProductId` and return stable response | API | R-001,R-002 | Validate `journeyId`, `items`, `meta` |
| 1.5-UI-001 | Clicking Refine triggers re-request and replaces recommendation set | Component/UI | R-001 | Assert changed rendered set |
| 1.5-UI-002 | Clicking More like this issues request with seed product and replaces set | Component/UI | R-001,R-002 | Verify payload fields |
| 1.5-UI-003 | Error state shows retry -> refine -> search in visual/action order | Component/UI | R-003 | Regression-safe triad ordering |
| 1.5-API-003 | Repeated refinement preserves journey correlation behavior | API | R-002,R-004 | Header continuity contract |

### P1 (High)

| Test ID | Requirement | Test Level | Risk Link | Notes |
|---|---|---|---|---|
| 1.5-API-004 | Invalid/unknown mode validation behavior is explicit | API | R-002,R-006 | 422 or documented fallback |
| 1.5-API-005 | Missing/invalid `seedProductId` in more_like_this degrades deterministically | API | R-004 | No crash, contract stable |
| 1.5-UI-004 | Search escape remains available during error recovery | Component/UI | R-003 | No dead-end |
| 1.5-UI-005 | Loading skeleton remains visible during refine loop transitions | Component/UI | R-005 | No blocking spinner regression |
| 1.5-UI-006 | Existing Story 1.4 success rendering still works with new payload fields | Component/UI | R-001 | Backward compatibility check |
| 1.5-API-006 | Response still bounded to <=3 items in refine modes | API | R-005 | Protect performance/UX |
| 1.5-INT-001 | OpenAPI request schema includes optional loop fields | Contract/Docs | R-002 | Annotation-to-doc validation |
| 1.5-INT-002 | UI typed model and API schema stay aligned | Integration | R-002 | Build-time type safety + contract review |

### P2/P3 (Medium/Low)

| Test ID | Requirement | Test Level | Risk Link | Notes |
|---|---|---|---|---|
| 1.5-UI-007 | Locale keys for refine/more-like-this exist in active locales | UI/i18n | R-003 | Sanity coverage |
| 1.5-API-007 | Score adjustments remain deterministic for same inputs | Unit/API | R-001,R-004 | Candidate for service unit tests |
| 1.5-UI-008 | Keyboard accessibility for new refine/alternative buttons | UI | R-003 | Focus + action semantics |
| 1.5-API-008 | Low-signal/no-candidate fallback reasons remain valid in refine loop | API | R-001 | Contract stability |
| 1.5-OPS-001 | Log structure excludes raw task/PII in loop requests | API/Observability | R-006 | Policy verification |
| 1.5-PERF-001 | Lightweight timing sample for repeated refine requests | Perf | R-005 | Informational gate support |

---

## Execution Strategy (Simple)

- **PR:** Run all functional Story 1.5 API/UI tests if runtime remains under ~15 minutes.
- **Nightly/Weekly:** Defer only expensive non-functional runs (extended perf/chaos), not core loop behavior.
- **Gate rule:** No merge with failing P0 scenarios.

---

## Resource Estimates (Ranges)

- **P0:** ~14-20 hours  
- **P1:** ~12-20 hours  
- **P2/P3:** ~4-10 hours  
- **Total:** ~30-50 hours (~4-7 working days)

---

## Quality Gates

- **P0 pass rate:** 100%
- **P1 pass rate:** >=95%
- **Contract gate:** request schema and response schema match implementation and docs
- **Risk gate:** R-001..R-003 mitigations complete before story marked done
- **Coverage target:** >=80% of scoped scenarios in this plan (with explicit waivers if deferred)

---

## Assumptions and Dependencies

### Assumptions

1. Story 1.4 baseline (`items`, `meta`) remains authoritative.
2. Recommendation route family remains unchanged in Epic 1.
3. Browser and PHP runtime limitations may continue in this environment.

### Dependencies

1. API controller/service remain editable for loop mode enhancements.
2. OpenAPI regeneration command available in a full PHP-capable environment.
3. Existing guided-discovery component remains the story entry point for new actions.

---

## Interworking & Regression

| Service/Component | Impact | Regression Scope |
|---|---|---|
| `RecommendationApiService` | Adds optional loop fields (`mode`, `seedProductId`) | Existing submit flow and journey propagation tests |
| `RecommendationController` | Adds request validation and loop semantics | Story 1.4 contract and header behavior tests |
| `RecommendationService` | Adds refine/more-like-this scoring branch | Deterministic ranking and bounded result checks |
| `TaskPromptBarComponent` | Adds refine/alternative controls and triad ordering | Existing validation, submit, loading, error states |

---

## Follow-on Workflow Recommendation

- Next best move after this plan: run `TA` (`bmad-testarch-automate`) to generate/expand prioritized automated tests for this story.

