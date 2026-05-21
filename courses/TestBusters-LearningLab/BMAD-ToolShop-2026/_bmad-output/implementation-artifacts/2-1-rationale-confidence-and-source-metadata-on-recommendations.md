# Story 2.1: Rationale, confidence, and source metadata on recommendations

Status: review

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a **shopper**,  
I want **plain-language "why," confidence cues, and source type (AI / rule / curated)**,  
so that **I can trust or challenge suggestions**.

## Acceptance Criteria

1. **Given** ranked items are returned  
   **When** cards render  
   **Then** each item shows rationale, confidence cue, and source label and meets disclosure for AI-driven aid (FR9, FR10, FR11, FR12, UX-DR2, NFR17).
2. **And** one-line rationale is visible before marketing fluff per UX guidelines (UX-DR2).
3. **And** recommendation API contract and UI model remain aligned for `rationale`, `confidence`, and `source` fields, documented in OpenAPI where applicable (FR60 alignment).

## Tasks / Subtasks

- [x] **Frontend: recommendation card metadata rendering** (AC: 1, 2)
  - [x] Add one-line rationale field to recommendation card body.
  - [x] Add confidence cue surface on each recommendation card.
  - [x] Add source label (AI / rule / curated) with clear copy and visual hierarchy.
- [x] **Contract and mapping alignment** (AC: 1)
  - [x] Extend UI recommendation response model with rationale/confidence/source fields.
  - [x] Extend backend recommendation response DTO/transformer to provide rationale/confidence/source values.
  - [x] Update OpenAPI recommendation response schema with the new fields and examples.
  - [x] Add API feature tests asserting presence and type of rationale/confidence/source on each returned item.
  - [x] Keep backward compatibility for existing Story 1.x response structure.
  - [x] Ensure mapping remains centralized in existing recommendation flow.
- [x] **Disclosure and accessibility compliance** (AC: 1, 2)
  - [x] Ensure rationale remains visible in primary card region (not hidden behind expansion).
  - [x] Verify semantics/labeling for assistive tech and avoid ambiguous abbreviations.
- [x] **Tests and quality gates** (AC: 1, 2)
  - [x] Add component tests for rationale/confidence/source rendering.
  - [x] Add regression tests for Story 1.4/1.5 recommendation surfaces.
  - [x] Validate build/lint/tests and capture environment blockers when applicable.

## Dev Notes

### Context and dependency notes

- Depends on Epic 1 recommendation card foundation (Stories 1.4-1.6).
- Do not replace existing card action hierarchy (`Add to cart`/secondary actions) when introducing metadata.
- Keep recommendation API calls centralized through `RecommendationApiService`.

### Architecture and UX guardrails

- Preserve camelCase response conventions for new recommendation fields.
- Maintain `X-Journey-Id` flow and existing contract compatibility rules (FR60 baseline).
- UX requires one-line rationale visible by default and progressive disclosure for deeper details (UX-DR2).
- Do not introduce a second HTTP client or duplicate recommendation rendering component tree.

### Likely touch points

- `sprint5/UI/src/app/models/recommendation-response.ts`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.html`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.ts`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.spec.ts`
- `sprint5/UI/src/assets/i18n/*.json` (if new labels/copy are introduced)

### References

- [Source: `_bmad-output/planning-artifacts/epics.md` (Story 2.1)]
- [Source: `_bmad-output/planning-artifacts/prd.md` (FR9-FR12)]
- [Source: `_bmad-output/planning-artifacts/architecture.md` (recommendation contracts, API/client guardrails)]
- [Source: `_bmad-output/planning-artifacts/ux-design-specification.md` (UX-DR2, trust and explanation patterns)]

## Dev Agent Record

### Agent Model Used

Codex 5.3

### Debug Log References

- `ReadFile _bmad-output/planning-artifacts/epics.md` (Epic 2 block)
- `rg _bmad-output/planning-artifacts/prd.md` for FR9-FR12
- `rg _bmad-output/planning-artifacts/architecture.md` for recommendation contract constraints
- `rg _bmad-output/planning-artifacts/ux-design-specification.md` for explanation/disclosure guidance
- `ReadFile sprint5/UI/src/app/models/recommendation-response.ts`
- `ReadFile sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.html`
- `ReadFile sprint5/API/app/Services/RecommendationService.php`
- `ReadFile sprint5/API/app/Http/Controllers/RecommendationController.php`
- `ReadFile sprint5/API/tests/Feature/RecommendationSessionTest.php`
- `npm run --prefix "sprint5/UI" build -- --configuration development`
- `npm run --prefix "sprint5/UI" lint`
- `npm run --prefix "sprint5/UI" test -- --watch=false --browsers=ChromeHeadless --include="src/app/guided-discovery/task-prompt-bar.component.spec.ts" --include="src/app/_services/recommendation-api.service.spec.ts"` *(blocked: ChromeHeadless cannot start in sandbox)*
- `php artisan test tests/Feature/RecommendationSessionTest.php` *(blocked: php runtime unavailable in sandbox)*

### Completion Notes List

- Added rationale/confidence/source metadata to recommendation item contract in UI and API layers.
- Updated recommendation card rendering to surface one-line rationale, confidence cue, and source label.
- Added translation keys for metadata labels and value enums across supported locales.
- Updated OpenAPI recommendation response schema for the new metadata fields.
- Extended backend feature tests and frontend component tests for metadata contract/rendering assertions.
- Build and lint passed; UI/browser tests and PHP feature tests remain environment-blocked in this sandbox.
- Set status to `review`.

### File List

- `_bmad-output/implementation-artifacts/2-1-rationale-confidence-and-source-metadata-on-recommendations.md`
- `_bmad-output/implementation-artifacts/sprint-status.yaml`
- `sprint5/UI/src/app/models/recommendation-response.ts`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.html`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.spec.ts`
- `sprint5/UI/src/assets/i18n/en.json`
- `sprint5/UI/src/assets/i18n/de.json`
- `sprint5/UI/src/assets/i18n/es.json`
- `sprint5/UI/src/assets/i18n/fr.json`
- `sprint5/UI/src/assets/i18n/nl.json`
- `sprint5/UI/src/assets/i18n/tr.json`
- `sprint5/API/app/Services/RecommendationService.php`
- `sprint5/API/app/Http/Controllers/RecommendationController.php`
- `sprint5/API/tests/Feature/RecommendationSessionTest.php`

### Change Log

- 2026-04-20: Implemented Story 2.1 recommendation metadata contract + UI rendering and moved to review.

---

**Story completion status:** Implementation complete and moved to review with validation evidence and blockers documented.
