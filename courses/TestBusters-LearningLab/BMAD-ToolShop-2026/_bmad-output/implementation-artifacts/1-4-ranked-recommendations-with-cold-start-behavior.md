# Story 1.4: Ranked recommendations with cold-start behavior

Status: review

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a **shopper**,  
I want **a short ranked list of products for my task**,  
so that **I can decide quickly even with no prior history**.

## Context (Epic 1)

Epic 1 establishes the guided discovery foundation in small increments. Story 1.2 created task/chip capture and Story 1.3 added journey/session continuity. Story 1.4 is the first story that must return and render a meaningful ranked set with explicit cold-start behavior.

Cross-story sequencing:

- **Done in 1.1:** Guided entry points and dual-rail navigation.
- **Done in 1.2:** Task prompt bar, chip capture, recommendation request payload.
- **Done in 1.3:** Session bootstrap and `X-Journey-Id` propagation.
- **This story (1.4):** Rank and return a short recommendation set with fallback logic and UI skeleton states.
- **Later (1.5+):** Refine/alternative loops and full fallback controls.

## Acceptance Criteria

1. **Ranked recommendation response under normal load**  
   **Given** a valid task payload (new or returning user, guest or authenticated per policy)  
   **When** I request recommendations  
   **Then** I receive **<=3** ranked items within the **500 ms** budget under normal load with skeleton placeholders during fetch (FR4, FR5, FR8, FR27, FR28, NFR1, NFR4, UX-DR11).
   **And** acceptance is verified with an explicit gate: p95 recommendation API latency <=500 ms in local/dev profiling evidence (or equivalent benchmark harness evidence), documented in the story completion notes.

2. **Cold-start safe defaults**  
   **Given** a shopper has little or no recommendation history  
   **When** ranking executes  
   **Then** cold-start uses documented safe defaults (rules + popularity) without blocking on external ML (FR8).

3. **Session-aware and contract-aligned output**  
   **Given** recommendation APIs are consumed by Angular clients and partners  
   **When** recommendations are returned  
   **Then** response fields and OpenAPI remain aligned with versioned API conventions, and `X-Journey-Id` continuity remains intact for correlation (FR26, FR60, architecture).  
   **And** for Epic 1 implementation continuity, Story 1.4 extends the existing recommendation route family introduced in Story 1.3 (no route-family migration in this story; if `/v1` migration is required, treat as a separate contract/versioning story).

4. **No regression in guided flow stability**  
   **Given** Story 1.2/1.3 guided input and session flows  
   **When** ranking is introduced  
   **Then** submit/escape behavior remains stable and errors degrade gracefully without trapping users away from classic discovery (FR7, FR25, UX parity).

## Tasks / Subtasks

- [x] **Backend: ranked recommendation endpoint behavior** (AC: 1, 3)
  - [x] Implement/extend recommendation ranking service path for task + chip input using existing API route introduced in Story 1.3 (keep current route family unchanged in this story).
  - [x] Return a bounded ranked payload (`<=3` items) with stable, documented ordering and typed response fields.
  - [x] Preserve `X-Journey-Id` correlation behavior from Story 1.3 in request handling and logs (no PII leakage).

- [x] **Backend: cold-start strategy and fallback ladder** (AC: 2, 4)
  - [x] Implement a deterministic fallback ladder: task/chip signals -> catalog constraints (availability/price) -> popularity/rule defaults.
  - [x] Ensure cold-start does not depend on external model availability; external ranking paths must be optional/non-blocking.
  - [x] Add explicit behavior for empty/weak candidates using a fixed response shape contract: always return `items` array (possibly empty), `meta` object with `isColdStart` boolean and `fallbackReason` string enum (`no_history`, `low_signal`, `no_candidates`).

- [x] **API contract: OpenAPI and schema alignment** (AC: 3)
  - [x] Update controller annotations to reflect recommendation response schema for ranked items.
  - [x] Keep reusable `X-Journey-Id` parameter references consistent where recommendation operations require it.
  - [x] Document empty-result response shape and `meta.isColdStart` / `meta.fallbackReason` fields in OpenAPI.
  - [ ] Regenerate/export API docs artifact per repository workflow. *(blocked in this environment: `php` runtime unavailable)*

- [x] **Frontend: recommendation strip/cards + loading states** (AC: 1, 4)
  - [x] Extend guided discovery UI to render the ranked set (max 3 cards) after successful submit.
  - [x] Add skeleton placeholders during request lifecycle and clear error/retry presentation without full-page blocking spinner.
  - [x] Keep "Use search instead" and existing task form interactions unchanged.

- [x] **Tests and quality gates** (AC: 1-4)
  - [x] Backend feature tests for ranked response bounds/order and cold-start fallback behavior.
  - [ ] Backend unit tests for pure ranking/fallback functions where applicable. *(deferred: service-level coverage expanded in feature tests in this story)*
  - [x] Frontend component/service tests for loading -> success -> error states and max-item rendering.
  - [ ] Add a lightweight performance evidence step (benchmark/profiling run) and record p95 latency evidence against the <=500 ms gate. *(blocked in this environment: `php` runtime unavailable to run API benchmark harness)*
  - [x] Validate touched layers with build/lint/tests (or document sandbox blockers with evidence).

## Dev Notes

### Previous story intelligence (1.3)

- `RecommendationApiService` now bootstraps/resumes session and sends `X-Journey-Id`; do not bypass it with a second recommendation HTTP client.
- Backend already has recommendation session + task submission scaffolding; extend that baseline instead of creating parallel endpoint families.
- Story 1.3 documented environment limits (`php` unavailable in sandbox, browser binaries missing). Keep evidence logging explicit when validating.
- Story 1.3 intentionally deferred ranked rendering/cold-start logic to this story; implement here without scope bleed into alternatives loop (1.5).

### Architecture compliance

- Keep implementation in canonical sprint5 structure:
  - API: `sprint5/API/routes/api.php`, `app/Http/Controllers/`, `app/Services/`.
  - UI: `sprint5/UI/src/app/guided-discovery/`, `_services/`, `models/`.
- Preserve naming/casing conventions:
  - API paths plural and stable.
  - JSON response keys camelCase for new recommendation endpoints.
  - Header casing exactly `X-Journey-Id`.
- Keep recommendation logic server-side; Angular remains presentation + orchestration.
- Maintain graceful degradation patterns and fallback parity with classic discovery.

### Technical guardrails

- **Latency guardrail:** Optimize for perceived sub-500 ms recommendation steps under normal load; avoid extra synchronous hops on hot path.
- **Latency evidence:** Capture and document p95 API latency evidence for Story 1.4 acceptance (not just functional pass/fail tests).
- **Bounded response:** Do not return long lists in this story; strict cap at 3 ranked items for decisive UX.
- **Cold-start ladder:** Prefer deterministic, explainable defaults (rules + popularity) before any optional model influence.
- **Response contract stability:** Always return stable keys (`items`, `meta`) even when no candidates are found.
- **Catalog reality checks:** Ranking candidates should respect availability and baseline quality constraints to avoid low-trust results.
- **Observability:** Log journey correlation and latency metrics without raw task text/PII in production logs.
- **No reinvention:** Reuse existing `RecommendationApiService` and session/journey plumbing from Story 1.3.

### Backend implementation guidance

- Likely touch points:
  - `sprint5/API/app/Http/Controllers/RecommendationController.php`
  - `sprint5/API/app/Services/RecommendationService.php`
  - `sprint5/API/tests/Feature/RecommendationSessionTest.php` (or split recommendation feature test files)
  - `sprint5/API/tests/Unit/` for pure ranking helper tests
- Recommended approach:
  - Introduce a small ranking method that accepts task context and returns normalized ranked DTOs.
  - Encapsulate fallback strategy in service-level helper(s) to keep controller thin.
  - Preserve current validation/error envelope style.

### Frontend implementation guidance

- Likely touch points:
  - `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.ts`
  - `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.html`
  - `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.spec.ts`
  - `sprint5/UI/src/app/models/` (ranked response interfaces)
  - `sprint5/UI/src/app/_services/recommendation-api.service.ts`
- UI behaviors to preserve:
  - Existing validation semantics (`aria-invalid`, `aria-describedby`).
  - Existing escape route (`Use search instead`).
  - Add skeleton/error states in-place (no disruptive navigation changes).

### OpenAPI / contract notes

- Keep recommendation endpoint documentation current before merge.
- Reuse `components.parameters.XJourneyId` and avoid header naming drift.
- Ensure response schema includes ranked item list and remains compatible with integration consumers (FR60).
- Document cold-start metadata fields and empty-result shape so frontend rendering logic is deterministic.
- Regenerated artifact target: `sprint5/API/storage/api-docs/api-docs.json`.

### Testing requirements

- Backend:
  - `<=3` ranked items constraint validated.
  - Cold-start path returns deterministic safe defaults.
  - Journey header continuity remains accepted.
- Frontend:
  - Skeleton visible during fetch, then replaced by cards on success.
  - Error state shows controlled retry path without regressing task form behavior.
  - Rendering enforces max visible recommendation cards.
- Regression:
  - Existing guided submit and search-escape flows unchanged.
  - No breakage to cart/session continuity expectations.

### Library / framework requirements

- Angular 20 standalone component/service patterns already used in sprint5 UI.
- Laravel API conventions and swagger-php/l5-swagger annotations for contract updates.
- No new dependencies expected; implement with existing stack first.

### File structure requirements (expected touch list)

| Area | Path |
|------|------|
| Story artifact | `_bmad-output/implementation-artifacts/1-4-ranked-recommendations-with-cold-start-behavior.md` |
| Sprint tracking | `_bmad-output/implementation-artifacts/sprint-status.yaml` |
| API controller/service | `sprint5/API/app/Http/Controllers/RecommendationController.php`, `sprint5/API/app/Services/RecommendationService.php` |
| API tests | `sprint5/API/tests/Feature/`, `sprint5/API/tests/Unit/` |
| UI guided flow | `sprint5/UI/src/app/guided-discovery/` |
| UI service/models | `sprint5/UI/src/app/_services/recommendation-api.service.ts`, `sprint5/UI/src/app/models/` |
| OpenAPI artifact | `sprint5/API/storage/api-docs/api-docs.json` |

### Out of scope (explicit)

- Alternative recommendation loop and refine controls (Story 1.5).
- Full fallback switching flow ownership beyond preserving current escape paths (Story 1.6).
- Explainability confidence/source surfaces (Epic 2 stories).
- Checkout attribution implementation depth (Epic 4).

### Git intelligence summary

- Recent commits suggest incremental, additive story-by-story delivery.
- Continue extending existing recommendation/service files instead of broad refactors.
- Keep code placement aligned to existing sprint5 layout and avoid parallel structures.

### Latest technical information

- OpenAPI 3 supports reusable header parameter definitions under `components.parameters` with `$ref` reuse across operations; keep this pattern for `X-Journey-Id`.
- Cold-start recommendation best practice remains a ranked fallback ladder: explicit session/task context first, then constraints, then popularity/rule defaults; avoid blocking on heavier personalization dependencies during initial response.

### References

- [Source: `_bmad-output/planning-artifacts/epics.md` (Epic 1, Story 1.4 acceptance criteria)]
- [Source: `_bmad-output/planning-artifacts/prd.md` (FR4, FR5, FR7, FR8, FR25, FR26, FR27, FR28, FR60)]
- [Source: `_bmad-output/planning-artifacts/architecture.md` (recommendation boundaries, naming/casing, OpenAPI, latency, journey header)]
- [Source: `_bmad-output/planning-artifacts/ux-design-specification.md` (recommendation card patterns, skeleton/loading, fallback parity, accessibility)]
- [Source: `_bmad-output/implementation-artifacts/1-3-journey-identifier-and-recommendation-session-api.md` (previous-story implementation baseline)]

## Dev Agent Record

### Agent Model Used

Codex 5.3

### Debug Log References

- `git log -5 --oneline`
- Web research: OpenAPI reusable parameter guidance (`components.parameters` + `$ref`)
- Web research: cold-start fallback ladder best practices (rules/popularity + contextual constraints)
- `npm run --prefix "sprint5/UI" build -- --configuration development`
- `npm run --prefix "sprint5/UI" lint`
- `npm run --prefix "sprint5/UI" test -- --watch=false --browsers=ChromeHeadless --include="src/app/_services/recommendation-api.service.spec.ts" --include="src/app/guided-discovery/task-prompt-bar.component.spec.ts"` *(blocked: Chrome binary unavailable in sandbox)*
- `php artisan test tests/Feature/RecommendationSessionTest.php` *(blocked: `php` command unavailable in sandbox)*

### Completion Notes List

- Implemented ranked recommendation response contract with `items` and `meta` (`isColdStart`, `fallbackReason`) while preserving Story 1.3 route family and journey header continuity.
- Added deterministic cold-start fallback scoring in `RecommendationService` and updated controller response/OpenAPI annotations.
- Expanded API feature tests for bounded result count and fallback reason behavior (`low_signal`, `no_candidates`).
- Added guided UI recommendation cards, loading skeletons, retry error state, and response model typing in Angular service/component layers.
- Updated i18n strings for recommendation loading/results/error surfaces across existing locales.
- Build and lint succeeded; frontend test and backend test execution remain environment-blocked (missing browser and `php` runtime).

### File List

- `_bmad-output/implementation-artifacts/1-4-ranked-recommendations-with-cold-start-behavior.md`
- `_bmad-output/implementation-artifacts/sprint-status.yaml` (status updates: in-progress -> review)
- `sprint5/API/app/Http/Controllers/RecommendationController.php`
- `sprint5/API/app/Services/RecommendationService.php`
- `sprint5/API/tests/Feature/RecommendationSessionTest.php`
- `sprint5/UI/src/app/_services/recommendation-api.service.ts`
- `sprint5/UI/src/app/_services/recommendation-api.service.spec.ts`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.ts`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.html`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.css`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.spec.ts`
- `sprint5/UI/src/app/models/recommendation-response.ts`
- `sprint5/UI/src/assets/i18n/en.json`
- `sprint5/UI/src/assets/i18n/de.json`
- `sprint5/UI/src/assets/i18n/es.json`
- `sprint5/UI/src/assets/i18n/fr.json`
- `sprint5/UI/src/assets/i18n/nl.json`
- `sprint5/UI/src/assets/i18n/tr.json`

---

**Story completion status:** Implementation complete and moved to review with environment blockers documented.
