# Story 4.2: Conversion attribution to recommendation context

Status: review

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a **product owner**,
I want **orders to record recommendation source context**,
so that **we can measure uplift**.

## Acceptance Criteria

1. **Given** a completed purchase that originated from guided or fallback paths
   **When** the order is persisted
   **Then** attribution fields store experiment/recommendation source per FR23 schema (FR23, FR24).
2. **And** missing AI context does not block order completion (FR24, FR25).

## Tasks / Subtasks

- [x] **Attribution schema and mapping** (AC: 1)
  - [x] Define/extend order attribution fields for journey, recommendation source, and experiment context per FR23.
  - [x] Map checkout payload/session context into attribution fields on order persistence.
  - [x] Ensure field defaults are explicit for fallback/unknown contexts.
- [x] **Checkout persistence integration** (AC: 1, 2)
  - [x] Persist attribution metadata as part of standard checkout completion flow.
  - [x] Ensure missing AI metadata does not fail validation or transaction commit.
  - [x] Keep backward compatibility with existing order API consumers.
- [x] **Attribution observability and contracts** (AC: 1)
  - [x] Expose persisted attribution fields in relevant internal/admin surfaces if required by existing contracts.
  - [x] Update API/OpenAPI docs if contract fields are externally visible.
- [x] **Tests and quality gates** (AC: 1, 2)
  - [x] Add tests for order completion with guided context and fallback context.
  - [x] Add tests proving checkout succeeds when attribution metadata is missing.
  - [x] Run build/lint/tests and document environment blockers if present.

## Dev Notes

### Context and dependency notes

- Builds on Story 4.1 journey propagation and Story 3.x guided/fallback recommendation paths.
- Attribution must be additive analytics context, not a functional prerequisite for checkout.
- Schema decisions should align with telemetry and analytics epics to avoid rework.

### Architecture and UX guardrails

- Preserve existing checkout and order transaction boundaries; avoid cross-cutting changes in unrelated flows.
- Keep null-safe attribution handling to satisfy FR24/FR25 no-blocking requirement.
- Reuse existing order persistence models and migration conventions.

### Likely touch points

- `sprint5/API/app/Http/Controllers/**/*.php`
- `sprint5/API/app/Services/**/*.php`
- `sprint5/API/app/Models/**/*.php`
- `sprint5/API/database/migrations/*.php`
- `sprint5/API/tests/Feature/**/*.php`
- `sprint5/UI/src/app/checkout/**/*.ts` (if client payload changes are needed)

### References

- [Source: `_bmad-output/planning-artifacts/epics.md` (Story 4.2)]
- [Source: `_bmad-output/planning-artifacts/prd.md` (FR23, FR24, FR25)]
- [Source: `_bmad-output/planning-artifacts/architecture.md` (order persistence and contract compatibility constraints)]

## Dev Agent Record

### Agent Model Used

Codex 5.3

### Debug Log References

- `ReadFile _bmad-output/planning-artifacts/epics.md` (Epic 4, Story 4.2)
- `ReadFile _bmad-output/implementation-artifacts/sprint-status.yaml` (story tracking update prep)
- `ReadFile sprint5/UI/src/app/checkout/payment/payment.component.ts` (checkout payload mapping)
- `ReadFile sprint5/API/app/Models/Invoice.php` (OpenAPI and persistence schema updates)
- `npm run --prefix "sprint5/UI" build -- --configuration development` (pass)
- `npm run --prefix "sprint5/UI" lint` (pass)
- `npm run --prefix "sprint5/UI" test -- --watch=false --browsers=ChromeHeadless` (blocked: ChromeHeadless cannot start)
- `php artisan test --filter InvoiceTest` (blocked: `php` runtime unavailable in sandbox)

### Completion Notes List

- Added checkout attribution context handling on UI side to map guided recommendation source into checkout payload metadata.
- Added fallback defaults (`recommendation_context=fallback`, `recommendation_source=fallback`) when no guided attribution exists, ensuring non-blocking checkout.
- Added DB schema support for order attribution fields: `recommendation_context`, `recommendation_source`, and `experiment_variant`.
- Updated invoice request validation and model fillable/select logic to persist and expose attribution fields with backward compatibility.
- Updated OpenAPI model schema to document attribution fields on invoice request/response contracts.
- Added API feature tests for both guided attribution persistence and successful checkout without attribution metadata.
- Added UI tests for checkout attribution context service and guided add-to-cart attribution capture behavior.
- Validation: UI build/lint pass. UI/browser tests and PHP feature tests remain environment-blocked in sandbox.

### File List

- `_bmad-output/implementation-artifacts/4-2-conversion-attribution-to-recommendation-context.md`
- `sprint5/UI/src/app/_services/checkout-attribution.service.ts`
- `sprint5/UI/src/app/_services/checkout-attribution.service.spec.ts`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.ts`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.spec.ts`
- `sprint5/UI/src/app/checkout/payment/payment.component.ts`
- `sprint5/API/database/migrations/2026_04_20_184500_add_recommendation_attribution_to_invoices_table.php`
- `sprint5/API/app/Models/Invoice.php`
- `sprint5/API/app/Http/Requests/Invoice/StoreInvoice.php`
- `sprint5/API/app/Http/Controllers/InvoiceController.php`
- `sprint5/API/tests/Feature/InvoiceTest.php`

---

**Story completion status:** Comprehensive developer guide created for Story 4.2.
