# Story 2.3: Explicit and implicit feedback capture

Status: review

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a **shopper**,  
I want **to mark "Not helpful" and have my clicks inform quality**,  
so that **the product improves without heavy forms**.

## Acceptance Criteria

1. **Given** recommendations are visible  
   **When** I use the feedback chip and optional "Tell us more"  
   **Then** events are recorded through the shared consent instrumentation shell (Story 2.4) and rejected when consent is not valid (FR13, FR14, FR30, UX-DR5).
2. **And** confirmation uses polite toast `role="status"` (UX-DR11).

## Tasks / Subtasks

- [x] **Frontend: feedback interaction surfaces** (AC: 1, 2)
  - [x] Add "Not helpful" interaction and optional detail path ("Tell us more").
  - [x] Emit explicit feedback payload with recommendation/session context.
  - [x] Show confirmation toast with `role="status"` and non-disruptive timing.
- [x] **Implicit feedback capture hooks** (AC: 1)
  - [x] Define client-side hooks for click/selection signals on recommendation cards.
  - [x] Ensure events include journey/session correlation and minimal required metadata.
- [x] **Server-side consent shell integration (consume, do not re-implement)** (AC: 1)
  - [x] Route feedback events through the Story 2.4 consent-aware telemetry path.
  - [x] Reuse shared consent rejection behavior; do not introduce story-local consent logic.
- [x] **Tests and quality gates** (AC: 1, 2)
  - [x] Add component tests for feedback UI interactions and toast semantics.
  - [x] Add API/integration tests for consent-gated event handling.
  - [x] Validate build/lint/tests and document environment blockers if present.

## Dev Notes

### Context and dependency notes

- Extends Story 2.1/2.2 trust surfaces with active feedback loop.
- Must preserve Story 1 recommendation behaviors and avoid adding friction to primary buying flow.
- Events should be additive and not block core recommendation rendering.
- Story 2.4 is a prerequisite platform shell; Story 2.3 consumes it for feedback telemetry.

### Architecture and UX guardrails

- Consent gating must be honored both client-side and server-side.
- Keep telemetry and recommendation traffic separated by responsibility while preserving journey correlation.
- Avoid logging raw PII in feedback event payloads/logs.
- UX-DR5 and UX-DR11 govern feedback pattern and status messaging.

### Likely touch points

- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.html`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.ts`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.spec.ts`
- `sprint5/API/app/Http/Controllers/*` (telemetry endpoint integration)
- `sprint5/API/app/Services/*` (consent-aware event handling)
- `sprint5/API/tests/Feature/*`

### References

- [Source: `_bmad-output/planning-artifacts/epics.md` (Story 2.3)]
- [Source: `_bmad-output/planning-artifacts/prd.md` (FR13, FR14, FR30)]
- [Source: `_bmad-output/planning-artifacts/architecture.md` (consent-gated telemetry, logging constraints)]
- [Source: `_bmad-output/planning-artifacts/ux-design-specification.md` (UX-DR5, UX-DR11)]

## Dev Agent Record

### Agent Model Used

Codex 5.3

### Debug Log References

- `ReadFile _bmad-output/planning-artifacts/epics.md` (Story 2.3)
- `rg _bmad-output/planning-artifacts/prd.md` for FR13/FR14/FR30
- `rg _bmad-output/planning-artifacts/architecture.md` for consent/telemetry patterns
- `rg _bmad-output/planning-artifacts/ux-design-specification.md` for feedback/toast guidance
- `npm run --prefix "sprint5/UI" build -- --configuration development` (pass)
- `npm run --prefix "sprint5/UI" lint` (pass)
- `npm run --prefix "sprint5/UI" test -- --watch=false --browsers=ChromeHeadless` (blocked: ChromeHeadless cannot start in this environment)
- `php artisan test --filter RecommendationSessionTest` (blocked: `php` runtime not available in this environment)

### Completion Notes List

- Added explicit feedback controls on recommendation cards: `Not helpful` plus optional `Tell us more` detail input.
- Added polite feedback confirmation toast using `role="status"` with non-disruptive auto-hide timing.
- Implemented implicit feedback hooks for `refine` and `more like this` actions with session/journey correlation.
- Added frontend feedback telemetry service and payload model for consistent event dispatch.
- Added backend consent-gated feedback endpoint (`/recommendations/feedback`) and consent-aware storage path via service.
- Added frontend component tests for toast semantics and feedback event emission and API feature tests for consent-accepted and consent-rejected feedback handling.
- Verified build/lint, and documented environment blockers for browser-based UI tests and PHP-based API tests.

### File List

- `_bmad-output/implementation-artifacts/2-3-explicit-and-implicit-feedback-capture.md`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.html`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.ts`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.spec.ts`
- `sprint5/UI/src/app/_services/recommendation-api.service.ts`
- `sprint5/UI/src/app/_services/recommendation-feedback.service.ts`
- `sprint5/UI/src/app/models/recommendation-feedback.ts`
- `sprint5/UI/src/assets/i18n/en.json`
- `sprint5/UI/src/assets/i18n/de.json`
- `sprint5/UI/src/assets/i18n/es.json`
- `sprint5/UI/src/assets/i18n/fr.json`
- `sprint5/UI/src/assets/i18n/nl.json`
- `sprint5/UI/src/assets/i18n/tr.json`
- `sprint5/API/app/Http/Controllers/RecommendationController.php`
- `sprint5/API/app/Services/RecommendationFeedbackService.php`
- `sprint5/API/routes/api.php`
- `sprint5/API/tests/Feature/RecommendationSessionTest.php`

### Change Log

- 2026-04-20: Implemented Story 2.3 explicit/implicit feedback capture with consent-gated API path, accessible status toast, and updated UI/API tests.

---

**Story completion status:** Comprehensive developer guide created for Story 2.3.
