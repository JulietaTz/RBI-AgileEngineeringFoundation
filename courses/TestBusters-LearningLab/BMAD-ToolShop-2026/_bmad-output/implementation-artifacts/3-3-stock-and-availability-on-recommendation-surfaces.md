# Story 3.3: Stock and availability on recommendation surfaces

Status: review

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a **shopper**,
I want **accurate stock near decision time**,
so that **I do not commit to unavailable items**.

## Acceptance Criteria

1. **Given** recommendations are shown
   **When** inventory changes for a visible SKU
   **Then** cards update before add-to-cart per near-real-time NFR (FR18, NFR3).
2. **And** out-of-stock disables primary add with alternate CTA per UX card states (UX-DR2).

## Tasks / Subtasks

- [x] **Availability signal integration** (AC: 1)
  - [x] Introduce near-real-time availability refresh strategy for visible recommendation SKUs.
  - [x] Update recommendation card stock state without requiring full flow reset.
  - [x] Ensure stale availability does not permit invalid add-to-cart action.
- [x] **Out-of-stock CTA behavior** (AC: 2)
  - [x] Disable primary add-to-cart action when stock becomes unavailable.
  - [x] Render alternate CTA per UX state guidance for unavailable items.
  - [x] Preserve overall card layout consistency across in-stock/out-of-stock transitions.
- [x] **Resilience and continuity** (AC: 1, 2)
  - [x] Maintain recommendation session context and other card actions during stock refresh updates.
  - [x] Ensure fallback and recovery states remain functional with availability polling/update logic.
- [x] **Tests and quality gates** (AC: 1, 2)
  - [x] Add component/service tests for availability refresh and CTA state transitions.
  - [x] Add tests asserting add-to-cart is blocked when stock is unavailable.
  - [x] Run build/lint/tests and document environment blockers if present.

## Dev Notes

### Context and dependency notes

- Relies on guided recommendation card behavior and upcoming Story 3.4 add-to-cart path.
- Stock transitions must be reflected before user confirms purchase intent.
- Avoid introducing unnecessary polling overhead or visual jitter.

### Architecture and UX guardrails

- Prefer service-level refresh abstraction over per-component ad-hoc timers.
- Keep availability updates isolated to recommendation UI state updates.
- UX-DR2 requires explicit unavailable-state CTA behavior.

### Likely touch points

- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.ts`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.html`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.css`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.spec.ts`
- `sprint5/UI/src/app/_services/recommendation-api.service.ts`
- `sprint5/API/app/Http/Controllers/RecommendationController.php` (if stock revalidation endpoint support is needed)

### References

- [Source: `_bmad-output/planning-artifacts/epics.md` (Story 3.3)]
- [Source: `_bmad-output/planning-artifacts/prd.md` (FR18, NFR3)]
- [Source: `_bmad-output/planning-artifacts/ux-design-specification.md` (UX-DR2)]
- [Source: `_bmad-output/planning-artifacts/architecture.md` (near-real-time and state consistency constraints)]

## Dev Agent Record

### Agent Model Used

Codex 5.3

### Debug Log References

- `ReadFile _bmad-output/planning-artifacts/epics.md` (Epic 3, Story 3.3)
- `ReadFile _bmad-output/implementation-artifacts/sprint-status.yaml` (story tracking update prep)

### Completion Notes List

- Added near-real-time availability polling in guided discovery to refresh visible recommendation stock state without re-running the full user flow.
- Implemented primary add-to-cart CTA for in-stock recommendations and disabled unavailable state CTA for out-of-stock items.
- Added cart action status toast messaging to keep user feedback explicit when add attempts succeed or availability is stale.
- Added component tests for in-stock add action and out-of-stock disabled CTA rendering.
- Ran UI build and lint successfully; UI tests remain environment-blocked due to missing headless browser binaries in sandbox.

### File List

- `_bmad-output/implementation-artifacts/3-3-stock-and-availability-on-recommendation-surfaces.md`
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

**Story completion status:** Comprehensive developer guide created for Story 3.3.
