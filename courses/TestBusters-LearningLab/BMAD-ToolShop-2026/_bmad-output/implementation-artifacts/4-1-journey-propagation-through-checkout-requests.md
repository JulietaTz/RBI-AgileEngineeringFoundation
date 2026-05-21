# Story 4.1: Journey propagation through checkout requests

Status: review

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a **shopper**,
I want **my journey ID to persist through checkout API calls**,
so that **support and analytics can reconstruct my path**.

## Acceptance Criteria

1. **Given** I have an active `journeyId` from guided discovery
   **When** I proceed through checkout steps
   **Then** the client attaches the agreed header or metadata on checkout-related API calls (FR26, architecture).
2. **And** behavior is documented in OpenAPI where externally visible.

## Tasks / Subtasks

- [x] **Checkout journey propagation wiring** (AC: 1)
  - [x] Identify checkout API calls that need journey context propagation.
  - [x] Attach journey header/metadata consistently from client service layer.
  - [x] Ensure guided and fallback flows both preserve journey continuity.
- [x] **Backend request handling alignment** (AC: 1)
  - [x] Accept and preserve journey context on checkout-relevant endpoints.
  - [x] Ensure missing journey metadata does not break standard checkout.
  - [x] Keep behavior compatible with existing checkout contracts.
- [x] **OpenAPI documentation update** (AC: 2)
  - [x] Document journey propagation behavior for externally visible endpoints.
  - [x] Include header/metadata format and optionality rules.
- [x] **Tests and quality gates** (AC: 1, 2)
  - [x] Add tests validating checkout calls include journey metadata when available.
  - [x] Add tests validating checkout success when journey metadata is absent.
  - [x] Run build/lint/tests and document environment blockers if present.

## Dev Notes

### Context and dependency notes

- Depends on journey/session behavior implemented in Epic 1 and recommendation flows in Epics 2-3.
- Must avoid introducing checkout-only journey state that diverges from guided discovery source of truth.
- Journey propagation must support both AI-guided and fallback storefront paths.

### Architecture and UX guardrails

- Reuse existing Angular API service and interceptor patterns where possible; avoid per-component header logic.
- Keep Laravel API behavior backward-compatible for clients that do not yet send journey metadata.
- Do not add required AI-specific checkout steps; checkout remains standard-first.

### Likely touch points

- `sprint5/UI/src/app/_services/*`
- `sprint5/UI/src/app/checkout/**/*.ts`
- `sprint5/API/app/Http/Controllers/**/*.php`
- `sprint5/API/routes/api.php`
- `sprint5/API/tests/Feature/**/*.php`
- `sprint5/API/app/Models/**/*.php` (if persistence changes are needed)

### References

- [Source: `_bmad-output/planning-artifacts/epics.md` (Story 4.1)]
- [Source: `_bmad-output/planning-artifacts/prd.md` (FR26)]
- [Source: `_bmad-output/planning-artifacts/architecture.md` (journey propagation and API contract constraints)]

## Dev Agent Record

### Agent Model Used

Codex 5.3

### Debug Log References

- `ReadFile _bmad-output/planning-artifacts/epics.md` (Epic 4, Story 4.1)
- `ReadFile _bmad-output/implementation-artifacts/sprint-status.yaml` (story tracking update prep)
- `ReadFile sprint5/UI/src/app/_helpers/auth.interceptor.ts` (checkout request propagation point)
- `ReadFile sprint5/UI/src/app/_services/recommendation-api.service.ts` (journey source integration)
- `ReadFile sprint5/API/app/Http/Controllers/InvoiceController.php` (header handling + OpenAPI updates)
- `npm run --prefix "sprint5/UI" build -- --configuration development` (pass)
- `npm run --prefix "sprint5/UI" lint` (pass)
- `npm run --prefix "sprint5/UI" test -- --watch=false --browsers=ChromeHeadless` (blocked: ChromeHeadless cannot start)
- `php artisan test --filter InvoiceTest` (blocked: `php` runtime unavailable in sandbox)

### Completion Notes List

- Added a shared UI journey context service and reused it in recommendation + checkout flows to keep journey continuity centralized.
- Updated HTTP interceptor logic to attach `X-Journey-Id` on checkout-related API calls (`/carts`, `/payment/check`, `/invoices`) when journey context exists.
- Added interceptor unit tests proving journey header propagation for checkout requests, non-propagation for non-checkout requests, and safe behavior when journey is absent.
- Added API migration and invoice model support for `journey_id` persistence, keeping it nullable for backward compatibility.
- Updated invoice create endpoints to ingest `X-Journey-Id` header and persist it without requiring AI context.
- Updated OpenAPI annotations for invoice and payment checkout endpoints to document optional journey header propagation.
- Added feature tests validating invoice creation persists `journey_id` when header is provided and still succeeds when header is missing.
- Validation: UI build + lint pass. UI unit tests and API feature tests are environment-blocked in sandbox (missing ChromeHeadless / missing `php`).

### File List

- `_bmad-output/implementation-artifacts/4-1-journey-propagation-through-checkout-requests.md`
- `sprint5/UI/src/app/_services/journey-context.service.ts`
- `sprint5/UI/src/app/_services/recommendation-api.service.ts`
- `sprint5/UI/src/app/_helpers/auth.interceptor.ts`
- `sprint5/UI/src/app/_helpers/auth.interceptor.spec.ts`
- `sprint5/API/database/migrations/2026_04_20_181500_add_journey_id_to_invoices_table.php`
- `sprint5/API/app/Models/Invoice.php`
- `sprint5/API/app/Http/Controllers/InvoiceController.php`
- `sprint5/API/app/Http/Controllers/PaymentController.php`
- `sprint5/API/tests/Feature/InvoiceTest.php`

---

**Story completion status:** Comprehensive developer guide created for Story 4.1.
