# Story 2.5: Data subject deletion and minimization for recommendation data

Status: review

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a **shopper**,  
I want **to request deletion and rely on minimized retention**,  
so that **my data rights are honored**.

## Acceptance Criteria

1. **Given** an authenticated user requests deletion of recommendation-related personal data  
   **When** the request is processed  
   **Then** propagation runs per SLA across in-scope stores: recommendation session data, recommendation telemetry/events, and derived analytics views that still contain user-linkable identifiers (FR31, FR32, NFR9, NFR28-NFR30).
2. **And** audit log records the request outcome without storing unnecessary PII (FR49 alignment).
3. **And** request lifecycle SLA is explicit: acknowledgement within 24h and full propagation completion within 30 days, with status visible for operations/support.

## Tasks / Subtasks

- [x] **Deletion request processing flow** (AC: 1)
  - [x] Implement authenticated deletion request handling path.
  - [x] Trigger propagation across in-scope stores:
    - recommendation session storage
    - recommendation telemetry/event storage
    - derived analytics views/materializations with user-linkable identifiers
  - [x] Record completion/failure status for SLA tracking.
  - [x] Expose status model (`requested`, `in-progress`, `completed`, `failed`) for operational tracking.
- [x] **Data minimization enforcement** (AC: 1)
  - [x] Review recommendation/telemetry payload persistence for unnecessary personal fields.
  - [x] Apply minimization updates to storage and service-layer DTO mapping.
  - [x] Ensure retention/deletion policy alignment for touched entities.
- [x] **Audit and compliance logging** (AC: 2)
  - [x] Add audit event for deletion request lifecycle outcome.
  - [x] Ensure audit trail excludes non-required personal attributes.
- [x] **Tests and quality gates** (AC: 1, 2)
  - [x] Add feature/integration tests for deletion propagation path.
  - [x] Add tests asserting minimization and audit constraints.
  - [x] Validate build/lint/tests and document blockers when environment-limited.

## Dev Notes

### Context and dependency notes

- This story introduces compliance-critical behavior; correctness and traceability outweigh feature speed.
- Coordinate with existing recommendation/telemetry schema and any retention jobs already present.

### Architecture and policy guardrails

- Architecture mandates strict PII vs analytics separation and erasure propagation.
- Use migrations/service patterns already established in Laravel codebase.
- Keep audit logging immutable and privacy-safe.
- Avoid introducing ad-hoc deletion logic outside service boundaries.
- Do not hard-delete immutable audit events; redact or pseudonymize user-linkable fields when required by policy.

### Likely touch points

- `sprint5/API/app/Http/Controllers/*`
- `sprint5/API/app/Services/*`
- `sprint5/API/database/migrations/*`
- `sprint5/API/tests/Feature/*`
- `sprint5/API/tests/Unit/*` (if service-level deletion logic is isolated)

### References

- [Source: `_bmad-output/planning-artifacts/epics.md` (Story 2.5)]
- [Source: `_bmad-output/planning-artifacts/prd.md` (FR31, FR32, NFR9, NFR28-NFR30)]
- [Source: `_bmad-output/planning-artifacts/architecture.md` (PII separation, retention/deletion, audit requirements)]

## Dev Agent Record

### Agent Model Used

Codex 5.3

### Debug Log References

- `ReadFile _bmad-output/planning-artifacts/epics.md` (Story 2.5)
- `rg _bmad-output/planning-artifacts/prd.md` for FR31/FR32 and NFR28-NFR30
- `rg _bmad-output/planning-artifacts/architecture.md` for deletion/minimization/audit constraints
- `npm run --prefix "sprint5/UI" build -- --configuration development` (pass)
- `npm run --prefix "sprint5/UI" lint` (pass)
- `npm run --prefix "sprint5/UI" test -- --watch=false --browsers=ChromeHeadless` (blocked: ChromeHeadless cannot start in this environment)
- `php artisan test --filter RecommendationDataDeletionTest` (blocked: `php` runtime not available in this environment)

### Completion Notes List

- Added authenticated recommendation data deletion endpoints for request submission and latest status retrieval.
- Implemented deletion lifecycle service with statuses (`requested` → `in-progress` → `completed`/`failed`) and explicit SLA timestamps (`ack_due_at`, `completion_due_at`).
- Added deletion propagation across in-scope stores by pseudonymizing user-linkable identifiers in recommendation telemetry and derived analytics records.
- Added audit-safe compliance logging with deterministic user reference hash and outcome details, without storing direct personal identifiers.
- Added persistence schema for deletion requests, telemetry events, derived analytics records, and deletion audit logs.
- Applied minimization on recommendation telemetry persistence by storing only required metadata (`source`) and dropping free-text detail payloads.
- Added feature tests covering deletion lifecycle/status and audit minimization constraints.

### File List

- `_bmad-output/implementation-artifacts/2-5-data-subject-deletion-and-minimization-for-recommendation-data.md`
- `sprint5/API/database/migrations/2026_04_20_160000_create_recommendation_compliance_tables.php`
- `sprint5/API/app/Models/RecommendationDataDeletionRequest.php`
- `sprint5/API/app/Models/RecommendationTelemetryEvent.php`
- `sprint5/API/app/Models/RecommendationAnalyticsView.php`
- `sprint5/API/app/Models/RecommendationDeletionAuditLog.php`
- `sprint5/API/app/Services/RecommendationDataDeletionService.php`
- `sprint5/API/app/Http/Controllers/RecommendationDataDeletionController.php`
- `sprint5/API/routes/api.php`
- `sprint5/API/app/Services/RecommendationFeedbackService.php`
- `sprint5/API/app/Http/Controllers/RecommendationController.php`
- `sprint5/API/tests/Feature/RecommendationDataDeletionTest.php`

### Change Log

- 2026-04-20: Implemented Story 2.5 deletion propagation and minimization flow with SLA lifecycle status model, audit-safe logging, schema support, and feature tests.

---

**Story completion status:** Comprehensive developer guide created for Story 2.5.
