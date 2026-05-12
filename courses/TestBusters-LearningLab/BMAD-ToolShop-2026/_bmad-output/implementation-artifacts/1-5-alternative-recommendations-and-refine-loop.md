# Story 1.5: Alternative recommendations and refine loop

Status: review

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a **shopper**,  
I want **to ask for alternatives or refine my task after the first set**,  
so that **I can recover from a weak first match without leaving the shop**.

## Context (Epic 1)

Epic 1 progressively builds guided discovery from entry to recoverable recommendation flow. Story 1.4 introduced bounded ranked results with cold-start fallback. Story 1.5 extends this with in-flow recovery controls so weak initial matches can be corrected without losing session continuity.

Cross-story sequencing:

- **Done in 1.1:** Guided entry points and dual-rail navigation.
- **Done in 1.2:** Task/chip submission and escape-to-search path.
- **Done in 1.3:** Journey/session continuity with `X-Journey-Id`.
- **Done in 1.4:** Ranked recommendation response + loading/error states.
- **This story (1.5):** Refine task and alternatives loop with in-place requery behavior.
- **Later (1.6):** Fallback-to-classic discovery parity and explicit no-trap guarantees.

## Acceptance Criteria

1. **Refine and alternative controls trigger re-ranking**  
   **Given** an initial ranked set is shown  
   **When** I choose **Refine** or **More like this** / alternatives  
   **Then** the client re-submits context and replaces cards with a new ranked set (FR6).

2. **Recovery triad behavior is preserved**  
   **Given** recommendation API responses fail or do not satisfy the shopper  
   **When** recovery actions are presented  
   **Then** UI supports retry -> refine -> search ordering per UX-DR11/UX guidance and remains usable without dead ends.

3. **Session continuity remains intact through refinement**  
   **Given** I refine recommendations multiple times in one flow  
   **When** each refinement request is sent  
   **Then** journey/session correlation (`X-Journey-Id`) is preserved and no duplicate ad-hoc client path bypasses `RecommendationApiService` (FR26 foundation).

4. **No regression to Story 1.4 ranking contract**  
   **Given** Story 1.4 response shape (`items`, `meta`)  
   **When** alternatives/refinements are introduced  
   **Then** response contract stays stable and existing ranking/cold-start behavior remains backward-compatible for current clients (FR60).

## Tasks / Subtasks

- [x] **Frontend: refine and alternatives interaction model** (AC: 1, 2)
  - [x] Add explicit **Refine** action in recommendation UI and wire to resubmission path.
  - [x] Add **More like this** (or equivalent alternative trigger) on recommendation cards to request a refreshed ranked set.
  - [x] Preserve current loading skeleton behavior and avoid full-page blocking states.

- [x] **Frontend: request payload extension for loop context** (AC: 1, 3, 4)
  - [x] Extend recommendation request model to carry optional refinement hints (for example: `mode`, `seedProductId`, or extra chip/task adjustments) without breaking existing callers.
  - [x] Ensure all recommendation loop requests still flow through `RecommendationApiService` so `X-Journey-Id` propagation remains centralized.
  - [x] Keep search escape (`Use search instead`) and existing form validation untouched.

- [x] **Backend: alternative/refinement handling** (AC: 1, 3, 4)
  - [x] Extend recommendation service logic to interpret refinement hints and return adjusted rankings.
  - [x] Keep bounded result count and stable response contract from Story 1.4.
  - [x] Ensure repeated refinement requests remain deterministic and session-correlated.

- [x] **API contract: OpenAPI update for loop semantics** (AC: 4)
  - [x] Document optional recommendation request fields for refine/alternative flows (`mode`, `seedProductId`).
  - [x] Keep response schema and header usage references consistent with Story 1.4 contract.
  - [ ] Regenerate/export API docs artifact according to repo workflow. *(blocked in this environment: `php` runtime unavailable)*

- [x] **Tests and quality gates** (AC: 1-4)
  - [x] Frontend tests for refine action, alternative action, and replacement of recommendation set.
  - [x] Frontend tests for recovery triad ordering in error state.
  - [x] Backend feature tests for refinement request handling and contract stability.
  - [x] Validate build/lint/test for touched layers, or document environment blockers with command evidence.

## Dev Notes

### Previous story intelligence (1.4)

- Story 1.4 introduced recommendation cards, loading skeletons, retry error state, and fixed `items` + `meta` response contract.
- Story 1.4 explicitly requires route-family continuity with Story 1.3; 1.5 should not introduce route migration scope.
- Story 1.4 reinforced centralized API usage through `RecommendationApiService`; preserve this anti-duplication constraint.
- Environment limitations from previous story remain: browser binary and `php` runtime may block local sandbox test execution.

### Architecture compliance

- Keep implementation in canonical `sprint5` paths:
  - UI: `sprint5/UI/src/app/guided-discovery/`, `_services/`, `models/`
  - API: `sprint5/API/app/Http/Controllers/`, `app/Services/`, `tests/Feature/`
- Maintain recommendation contract conventions:
  - camelCase keys for new recommendation payload/response fields
  - consistent `X-Journey-Id` usage
  - no alternate recommendation HTTP client path
- Keep UX recovery pattern aligned with architecture/UX recommendation triad (retry/refine/search).

### Technical guardrails

- **No route-family drift:** Continue extending the existing recommendation route family from Stories 1.3/1.4.
- **No contract break:** Keep Story 1.4 response keys and semantics stable while adding optional request hints.
- **Deterministic loop behavior:** Refinement requests should be predictable and not oscillate due to non-deterministic scoring.
- **Session continuity:** All loop requests must preserve journey correlation and avoid resetting client session state.
- **Scope discipline:** Do not pull in full fallback switching logic from Story 1.6.

### Backend implementation guidance

- Likely touch points:
  - `sprint5/API/app/Http/Controllers/RecommendationController.php`
  - `sprint5/API/app/Services/RecommendationService.php`
  - `sprint5/API/tests/Feature/RecommendationSessionTest.php` (or split recommendation feature tests)
- Suggested approach:
  - Add optional refine parameters in request validation.
  - Integrate refine/alternative hints into ranking heuristics as incremental scoring constraints.
  - Keep controller thin; place loop logic in service methods.

### Frontend implementation guidance

- Likely touch points:
  - `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.ts`
  - `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.html`
  - `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.spec.ts`
  - `sprint5/UI/src/app/models/recommendation-request.ts`
  - `sprint5/UI/src/app/_services/recommendation-api.service.ts`
- Implementation expectations:
  - Refine action should be obvious and keyboard-accessible.
  - Alternative action should be card-local (for “more like this”) without visual chaos.
  - Recovery actions should remain present even during transient errors.

### OpenAPI / contract notes

- Update recommendation request schema with optional refinement fields.
- Keep response schema backward-compatible with Story 1.4.
- Preserve reusable header parameter references for journey correlation.
- API docs artifact target: `sprint5/API/storage/api-docs/api-docs.json`.

### Testing requirements

- Backend:
  - Refinement requests modify ranking behavior in expected direction.
  - Contract shape remains stable under refine and alternative modes.
  - Session/journey continuity preserved in repeated loop calls.
- Frontend:
  - Refine and alternative actions trigger new requests and card replacement.
  - Error state displays recovery triad actions in intended UX order.
  - Existing task submission and search-escape behavior remains unchanged.
- Regression:
  - Story 1.4 loading/success/error surfaces remain functional.

### Library / framework requirements

- Angular 20 standalone patterns with existing guided-discovery component approach.
- Laravel controller/service split with existing OpenAPI annotation style.
- No new dependencies expected for this story.

### File structure requirements (expected touch list)

| Area | Path |
|------|------|
| Story artifact | `_bmad-output/implementation-artifacts/1-5-alternative-recommendations-and-refine-loop.md` |
| Sprint tracking | `_bmad-output/implementation-artifacts/sprint-status.yaml` |
| UI guided flow | `sprint5/UI/src/app/guided-discovery/` |
| UI request model/service | `sprint5/UI/src/app/models/recommendation-request.ts`, `sprint5/UI/src/app/_services/recommendation-api.service.ts` |
| API recommendation logic | `sprint5/API/app/Http/Controllers/RecommendationController.php`, `sprint5/API/app/Services/RecommendationService.php` |
| API tests | `sprint5/API/tests/Feature/` |
| OpenAPI artifact | `sprint5/API/storage/api-docs/api-docs.json` |

### Out of scope (explicit)

- Full fallback mode switch ownership and classic-discovery continuity guarantees (Story 1.6).
- Explainability rationale/confidence/source UI (Epic 2).
- Checkout attribution depth and analytics conversion reporting (Epic 4).

### Git intelligence summary

- Recent commit history indicates additive changes in focused vertical slices.
- Continue story-by-story extension of current recommendation service/component surfaces; avoid broad refactors.

### Latest technical information

- UX guidance for recommendation errors emphasizes recovery triad ordering and no dead-end behavior.
- Recommendation API evolution should remain backward-compatible for active clients while introducing optional request refinement fields.

### References

- [Source: `_bmad-output/planning-artifacts/epics.md` (Epic 1, Story 1.5 acceptance criteria)]
- [Source: `_bmad-output/planning-artifacts/prd.md` (FR3, FR6, FR26, FR60, fallback/recovery expectations)]
- [Source: `_bmad-output/planning-artifacts/architecture.md` (recommendation boundaries, header propagation, contract evolution)]
- [Source: `_bmad-output/planning-artifacts/ux-design-specification.md` (retry/refine/search triad, recommendation recovery patterns)]
- [Source: `_bmad-output/implementation-artifacts/1-4-ranked-recommendations-with-cold-start-behavior.md` (current recommendation baseline)]

## Dev Agent Record

### Agent Model Used

Codex 5.3

### Debug Log References

- `git log -5 --oneline`
- Artifact review: Epic 1 story chain (1.4 -> 1.5 -> 1.6)
- Architecture/UX review: recommendation recovery triad and fallback parity expectations
- `npm run --prefix "sprint5/UI" build -- --configuration development`
- `npm run --prefix "sprint5/UI" lint`
- `npm run --prefix "sprint5/UI" test -- --watch=false --browsers=ChromeHeadless --include="src/app/guided-discovery/task-prompt-bar.component.spec.ts" --include="src/app/_services/recommendation-api.service.spec.ts"` *(blocked: browser binary unavailable in sandbox)*
- `php artisan test tests/Feature/RecommendationSessionTest.php` *(blocked: `php` command unavailable in sandbox)*

### Completion Notes List

- Added refine and more-like-this loop actions in guided recommendation UI and wired them to re-submit context without bypassing `RecommendationApiService`.
- Extended recommendation request contract with optional loop context (`mode`, `seedProductId`) and aligned backend validation/OpenAPI request schema.
- Updated recommendation service scoring to account for refine/more-like-this modes while preserving Story 1.4 response contract (`items`, `meta`).
- Expanded frontend and backend feature tests for refine/alternative flows and recovery triad action ordering.
- Build and lint passed; test execution remains environment-blocked by missing browser and `php` runtime.

### File List

- `_bmad-output/implementation-artifacts/1-5-alternative-recommendations-and-refine-loop.md`
- `_bmad-output/implementation-artifacts/sprint-status.yaml` (status updates: in-progress -> review)
- `sprint5/API/app/Http/Controllers/RecommendationController.php`
- `sprint5/API/app/Services/RecommendationService.php`
- `sprint5/API/tests/Feature/RecommendationSessionTest.php`
- `sprint5/UI/src/app/models/recommendation-request.ts`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.ts`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.html`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.spec.ts`
- `sprint5/UI/src/assets/i18n/en.json`
- `sprint5/UI/src/assets/i18n/de.json`
- `sprint5/UI/src/assets/i18n/es.json`
- `sprint5/UI/src/assets/i18n/fr.json`
- `sprint5/UI/src/assets/i18n/nl.json`
- `sprint5/UI/src/assets/i18n/tr.json`

---

**Story completion status:** Implementation complete and moved to review with environment blockers documented.
