# Story 1.3: Journey identifier and recommendation session API

Status: review

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a **shopper**,  
I want **a stable journey identifier across my session**,  
so that **later checkout and support can correlate my discovery path**.

## Context (Epic 1)

Epic 1 builds AI-guided discovery in increments. Story 1.1 established guided entry points and route shell; Story 1.2 captured task + chip inputs and sends recommendation requests from UI. Story 1.3 introduces cross-request correlation via recommendation session and journey id propagation.

Cross-story sequencing:

- **Done in 1.1:** `/guided` route and storefront entry parity.
- **Done in 1.2:** `RecommendationApiService`, typed task payload model, guided task form.
- **This story (1.3):** create/resume recommendation session, return `journeyId`, propagate `X-Journey-Id` on subsequent recommendation calls, OpenAPI alignment.
- **Later (1.4+):** ranked results, alternatives/refine loop, fallback and attribution depth.

## Acceptance Criteria

1. **Recommendation session bootstrap**  
   **Given** I start or resume guided discovery  
   **When** the client requests or creates a recommendation session  
   **Then** API responds with a stable `journeyId` and session metadata for the guided flow.

2. **Journey header propagation**  
   **Given** a `journeyId` exists for the active shopper flow  
   **When** client submits recommendation-related API calls  
   **Then** the Angular client sends `X-Journey-Id` on subsequent recommendation requests per architecture guidance.

3. **OpenAPI documentation alignment**  
   **Given** recommendation session and recommendation request endpoints are available  
   **When** API documentation is generated  
   **Then** OpenAPI includes journey header parameter and session resource contract (FR60 compatibility).

4. **Backward compatibility and no regression**  
   **Given** existing guided flow from Story 1.2  
   **When** session bootstrap is unavailable or delayed  
   **Then** system fails gracefully (clear error path / retry behavior) without breaking classic browsing or cart continuity.

## Tasks / Subtasks

- [x] **Backend: recommendation session resource + journey id generation** (AC: 1, 4)
  - [x] Add/extend API endpoint(s) under Laravel API v1 recommendation area (for example `/v1/recommendation-sessions` create/resume).
  - [x] Return a canonical response containing `journeyId` and enough metadata for UI correlation (timestamps/status optional by design).
  - [x] Persist session/journey mapping in DB or cache using existing architectural conventions (no ad-hoc global state).

- [x] **Backend: recommendation requests accept journey header** (AC: 2)
  - [x] Validate and consume `X-Journey-Id` for recommendation endpoints where correlation is required.
  - [x] Ensure invalid/missing header behavior is explicit and does not crash request handling.
  - [x] Add or update feature tests for session creation and header-aware recommendation requests.

- [x] **API contract: OpenAPI update** (AC: 3)
  - [x] Document session endpoint and response schema in OpenAPI.
  - [x] Add reusable header parameter under components (for `X-Journey-Id`) and reference it in relevant operations.
  - [ ] Regenerate/export API docs artifact according to repo workflow. *(blocked in this environment: `php` runtime unavailable)*

- [x] **Frontend: session bootstrap + header propagation** (AC: 1, 2, 4)
  - [x] Extend `RecommendationApiService` to create/resume session and store `journeyId` for current guided context.
  - [x] Add request path(s) so recommendation calls include `X-Journey-Id` once available.
  - [x] Prefer centralized propagation approach (service-level headers or interceptor) to avoid duplicated per-call header code.
  - [x] Keep Story 1.2 task form UX stable; no regression in submit/escape path.

- [x] **Tests and quality gates** (AC: 1-4)
  - [x] Backend tests: session endpoint contract, header validation/propagation behavior, regression path.
  - [x] Frontend tests: service-level mapping for session response, header inclusion on subsequent requests.
  - [x] Build/lint/test pipelines pass for touched layers or document environment blockers with evidence.

## Dev Notes

### Previous story intelligence (1.2)

- `RecommendationApiService` exists in `sprint5/UI/src/app/_services/recommendation-api.service.ts` and currently posts task payload to `/recommendations` without session bootstrap.
- Guided payload model exists at `sprint5/UI/src/app/models/recommendation-request.ts`.
- Guided task UI and i18n keys are in place; Story 1.3 should extend service behavior rather than reworking prompt UX.
- Story 1.2 test execution in this environment was browser-binary constrained; preserve this context when reporting validation evidence.

### Architecture compliance

- Keep Angular UI changes in canonical `sprint5/UI` tree; avoid older sprint directories unless explicitly required.
- Maintain service boundary: Angular components stay presentation-focused; API behavior belongs in service/interceptor layer.
- Follow naming and structure from architecture:
  - API v1 recommendation controllers/services in Laravel recommendation domain.
  - Header naming consistency: `X-Journey-Id` (exact casing).
  - OpenAPI updates required for new public routes and headers.

### Technical guardrails

- **Journey ID lifecycle:** session bootstrap should define authoritative `journeyId`; client must not generate incompatible IDs that conflict with server contract.
- **Propagation strategy:** one canonical mechanism (service helper or interceptor) for header injection to prevent drift.
- **Graceful degradation:** if session call fails, expose controlled retry/fallback path and avoid silent corruption of correlation data.
- **Data/privacy:** do not leak PII in logs while correlating by journey id.

### Backend implementation guidance

- Likely touch points:
  - `sprint5/API/routes/api.php`
  - `sprint5/API/app/Http/Controllers/Api/V1/Recommendation/`
  - `sprint5/API/app/Services/Recommendation/`
  - `sprint5/API/app/Http/Requests/Api/V1/`
  - `sprint5/API/tests/Feature/Api/V1/`
- Keep controller thin and validation explicit; business logic in service layer.
- Reuse existing auth/rate-limit and error envelope conventions.

### Frontend implementation guidance

- Likely touch points:
  - `sprint5/UI/src/app/_services/recommendation-api.service.ts`
  - `sprint5/UI/src/app/_helpers/` (if interceptor-based propagation chosen)
  - `sprint5/UI/src/app/models/` (session response model/header types)
  - `sprint5/UI/src/app/guided-discovery/` (only if minimal glue needed)
- Keep Story 1.2 task component contract stable.

### OpenAPI / contract notes

- Define reusable header parameter in `components.parameters` and reference in operations requiring `X-Journey-Id`.
- Document session resource (request/response) and recommendation operations with header semantics.
- Ensure generated docs stay backward-compatible with integration consumers (FR60).

### Testing requirements

- Backend:
  - Session create/resume happy path.
  - Missing/invalid journey header behavior.
  - Contract assertions for response schema.
- Frontend:
  - Service tests for bootstrap response parsing.
  - Header inclusion tests on recommendation requests after session acquisition.
- Regression:
  - Existing Story 1.2 guided submit flow still works.

### Library / framework requirements

- Angular 20 + existing standalone component/service patterns.
- Laravel API v1 and existing OpenAPI toolchain conventions.
- No new dependencies expected; if needed, require explicit user approval.

### File structure requirements (expected touch list)

| Area | Path |
|------|------|
| Story artifact | `_bmad-output/implementation-artifacts/1-3-journey-identifier-and-recommendation-session-api.md` |
| Sprint tracking | `_bmad-output/implementation-artifacts/sprint-status.yaml` |
| UI recommendation service | `sprint5/UI/src/app/_services/recommendation-api.service.ts` |
| UI models/interceptor (if needed) | `sprint5/UI/src/app/models/`, `sprint5/UI/src/app/_helpers/` |
| API routes/controllers/services | `sprint5/API/routes/api.php`, `sprint5/API/app/Http/Controllers/Api/V1/Recommendation/`, `sprint5/API/app/Services/Recommendation/` |
| API tests | `sprint5/API/tests/Feature/Api/V1/` |
| OpenAPI artifact/config | API docs generation path in `sprint5/API` |

### Out of scope (explicit)

- Ranked recommendation rendering and cold-start logic (Story 1.4).
- Alternative/refine loop UX and API semantics (Story 1.5).
- Full fallback switching mechanics (Story 1.6).
- Checkout attribution analytics implementation detail beyond journey-id foundation.

### Git intelligence summary

- Recent commit stream is dominated by BMad workflow and incremental UI changes; pattern is additive story-by-story implementation.
- Keep this story focused on journey/session foundation and avoid mixing in result-ranking behavior.

### Latest technical information

- OpenAPI 3 best practice supports reusable header parameters in `components.parameters` with `$ref` usage across endpoints; use this for `X-Journey-Id` consistency.
- Keep parameter naming and casing stable across operations to reduce client/server drift.

### References

- [Source: `_bmad-output/planning-artifacts/epics.md` (Epic 1, Story 1.3 acceptance criteria)]
- [Source: `_bmad-output/planning-artifacts/prd.md` (FR26, FR45-46, FR60)]
- [Source: `_bmad-output/planning-artifacts/architecture.md` (journey header, recommendation API, OpenAPI, service boundaries)]
- [Source: `_bmad-output/implementation-artifacts/1-2-task-prompt-bar-and-contextual-chips.md` (previous-story implementation learnings)]
- [Source: `sprint5/UI/src/app/_services/recommendation-api.service.ts` (current recommendation service baseline)]

## Dev Agent Record

### Agent Model Used

Codex 5.3

### Debug Log References

- `git log -5 --oneline`
- `php artisan test tests/Feature/RecommendationSessionTest.php` *(blocked: `php` command not available in sandbox)*
- `npm run --prefix "sprint5/UI" test -- --watch=false --browsers=ChromeHeadless --include="src/app/_services/recommendation-api.service.spec.ts" --include="src/app/guided-discovery/task-prompt-bar.component.spec.ts"` *(blocked: browser binary unavailable in sandbox)*
- `npm run --prefix "sprint5/UI" build -- --configuration development`
- `npm run --prefix "sprint5/UI" lint`

### Completion Notes List

- Added recommendation session bootstrap endpoint and recommendation submission endpoint with `X-Journey-Id` correlation handling.
- Implemented `RecommendationService` and `StoreRecommendationSession` request validation with cache-backed journey lifecycle.
- Extended `RecommendationApiService` to create/resume sessions, persist `journeyId`, and propagate `X-Journey-Id` on recommendation requests.
- Added backend feature coverage for session create/resume, invalid `journeyId`, and header-aware recommendation submissions.
- Added frontend service tests for session bootstrap + header propagation; build and lint pass, while automated tests are environment-blocked.

### File List

- `_bmad-output/implementation-artifacts/1-3-journey-identifier-and-recommendation-session-api.md`
- `_bmad-output/implementation-artifacts/sprint-status.yaml` (status updates: in-progress -> review)
- `sprint5/API/app/Http/Controllers/Controller.php`
- `sprint5/API/app/Http/Controllers/RecommendationController.php`
- `sprint5/API/app/Http/Requests/Recommendation/StoreRecommendationSession.php`
- `sprint5/API/app/Services/RecommendationService.php`
- `sprint5/API/routes/api.php`
- `sprint5/API/tests/Feature/RecommendationSessionTest.php`
- `sprint5/UI/src/app/_services/recommendation-api.service.ts`
- `sprint5/UI/src/app/_services/recommendation-api.service.spec.ts`
- `sprint5/UI/src/app/models/recommendation-session.ts`

---

**Story completion status:** Implementation complete and moved to review with environment blockers documented.
