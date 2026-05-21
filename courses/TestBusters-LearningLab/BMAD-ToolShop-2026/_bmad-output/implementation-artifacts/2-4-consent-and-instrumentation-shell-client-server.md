# Story 2.4: Consent and instrumentation shell (client + server)

Status: review

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a **shopper**,  
I want **analytics and personalization hooks to respect my CMP choices**,  
so that **tracking only happens when allowed**.

## Acceptance Criteria

1. **Given** CMP states blocked / partial / full  
   **When** recommendation impressions or clicks would fire  
   **Then** the shell blocks or gates GA4/internal events accordingly (FR29, FR30, UX-DR6, NFR8).
2. **And** server rejects telemetry writes without valid consent context (FR30).

## Tasks / Subtasks

- [x] **Client instrumentation shell** (AC: 1)
  - [x] Implement CMP-state adapter for blocked/partial/full behavior.
  - [x] Gate recommendation telemetry events based on resolved consent state.
  - [x] Ensure no event leakage when consent is blocked.
- [x] **Server enforcement middleware/service** (AC: 2)
  - [x] Enforce consent context on telemetry ingest endpoints.
  - [x] Return predictable rejection behavior for non-compliant writes.
  - [x] Keep rejection path observable for diagnostics/audit.
- [x] **Contract and policy alignment** (AC: 1, 2)
  - [x] Document consent-state schema used between client and server.
  - [x] Keep telemetry payload minimal and policy-compliant.
- [x] **Tests and quality gates** (AC: 1, 2)
  - [x] Add client tests for blocked/partial/full event behavior.
  - [x] Add backend tests for consent-required telemetry writes.
  - [x] Validate build/lint/tests; record environmental blockers with command evidence.

## Dev Notes

### Context and dependency notes

- Story 2.4 provides enforcement shell used by Story 2.3 feedback and later analytics stories.
- Must be implemented as a cross-cutting guardrail, not as isolated per-component checks.
- This story establishes the canonical consent gate behavior; downstream stories must consume this shell rather than re-implementing consent enforcement.

### Architecture and UX guardrails

- Architecture mandates consent-gated telemetry on both client and server.
- Use existing service boundaries; do not spread telemetry concerns across unrelated UI components.
- Keep logs auditable but avoid unnecessary PII storage.
- UX-DR6 governs blocked/partial/full behavior expectations.

### Likely touch points

- `sprint5/UI/src/app/_services/*` (instrumentation/consent adapter)
- `sprint5/UI/src/app/guided-discovery/*` (event wiring via shared shell)
- `sprint5/API/app/Http/Controllers/*` (telemetry ingest)
- `sprint5/API/app/Http/Middleware/*` or service-layer consent guards
- `sprint5/API/tests/Feature/*`

### References

- [Source: `_bmad-output/planning-artifacts/epics.md` (Story 2.4)]
- [Source: `_bmad-output/planning-artifacts/prd.md` (FR29, FR30, consent requirements)]
- [Source: `_bmad-output/planning-artifacts/architecture.md` (consent-gated telemetry, API governance)]
- [Source: `_bmad-output/planning-artifacts/ux-design-specification.md` (UX-DR6)]

## Dev Agent Record

### Agent Model Used

Codex 5.3

### Debug Log References

- `ReadFile _bmad-output/planning-artifacts/epics.md` (Story 2.4)
- `rg _bmad-output/planning-artifacts/prd.md` for FR29/FR30
- `rg _bmad-output/planning-artifacts/architecture.md` for consent telemetry enforcement
- `rg _bmad-output/planning-artifacts/ux-design-specification.md` for UX-DR6
- `npm run --prefix "sprint5/UI" build -- --configuration development` (pass)
- `npm run --prefix "sprint5/UI" lint` (pass)
- `npm run --prefix "sprint5/UI" test -- --watch=false --browsers=ChromeHeadless` (blocked: ChromeHeadless cannot start in this environment)
- `php artisan test --filter RecommendationSessionTest` (blocked: `php` runtime not available in this environment)

### Completion Notes List

- Added shared CMP-state adapter service with canonical states `blocked|partial|full`.
- Implemented client-side instrumentation gating for internal telemetry (blocked denied) and GA4 telemetry (full only).
- Updated recommendation feedback telemetry service to consume consent shell and prevent blocked-state event leakage.
- Added backend route middleware (`telemetry.consent`) to enforce consent context for telemetry ingest requests.
- Added predictable rejection behavior with structured `403` response and audit-friendly warning logs.
- Updated feedback endpoint OpenAPI contract with consent headers and aligned payload policy to minimal fields.
- Added UI service tests for blocked/partial/full consent behavior and backend feature tests for consent-required telemetry writes.

### File List

- `_bmad-output/implementation-artifacts/2-4-consent-and-instrumentation-shell-client-server.md`
- `sprint5/UI/src/app/_services/consent-instrumentation.service.ts`
- `sprint5/UI/src/app/_services/consent-instrumentation.service.spec.ts`
- `sprint5/UI/src/app/_services/recommendation-feedback.service.ts`
- `sprint5/UI/src/app/_services/recommendation-feedback.service.spec.ts`
- `sprint5/UI/src/app/_services/ga.service.ts`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.ts`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.spec.ts`
- `sprint5/API/app/Http/Middleware/EnsureTelemetryConsent.php`
- `sprint5/API/app/Http/Kernel.php`
- `sprint5/API/app/Http/Controllers/RecommendationController.php`
- `sprint5/API/routes/api.php`
- `sprint5/API/tests/Feature/RecommendationSessionTest.php`

### Change Log

- 2026-04-20: Implemented Story 2.4 consent instrumentation shell across client/server with middleware enforcement, consent-state contract headers, and consent behavior tests.

---

**Story completion status:** Comprehensive developer guide created for Story 2.4.
