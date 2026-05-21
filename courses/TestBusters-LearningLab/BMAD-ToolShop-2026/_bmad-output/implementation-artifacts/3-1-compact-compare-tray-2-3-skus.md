# Story 3.1: Compact compare tray (2-3 SKUs)

Status: review

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a **shopper**,
I want **to compare up to three recommendations on task-relevant attributes**,
so that **I can choose without opening many tabs**.

## Acceptance Criteria

1. **Given** I select Compare on one or two items
   **When** the tray opens (sticky desktop, offcanvas mobile)
   **Then** I can lock at most **three** SKUs and keyboard-navigate the comparison table (FR16, UX-DR4, NFR16).
2. **And** closing the tray does not empty my cart (UX-DR4).

## Tasks / Subtasks

- [x] **Frontend compare tray shell** (AC: 1, 2)
  - [x] Add compare selection action on recommendation cards and maintain selected SKU list.
  - [x] Enforce max 3 compared SKUs with clear UI feedback when limit is reached.
  - [x] Implement tray container behavior: sticky desktop and offcanvas/mobile.
- [x] **Compare table and keyboard support** (AC: 1)
  - [x] Render task-relevant attribute rows for selected SKUs.
  - [x] Support keyboard navigation across compare controls and table cells.
  - [x] Ensure ARIA semantics and focus order for accessibility compliance.
- [x] **State isolation and regression guardrails** (AC: 2)
  - [x] Ensure closing compare tray only affects compare state and not cart state.
  - [x] Verify existing guided recommendation actions (refine/more-like-this/feedback) remain intact.
- [x] **Tests and quality gates** (AC: 1, 2)
  - [x] Add component tests for max-selection behavior and tray open/close state.
  - [x] Add tests for keyboard navigation behavior and compare table accessibility hooks.
  - [x] Run build/lint/tests and document environment blockers if present.

## Dev Notes

### Context and dependency notes

- Depends on Epic 2 guided recommendation card structure and action density.
- Must preserve Story 2.6 CTA hierarchy and accent usage conventions while adding compare actions.
- Compare interaction should not mutate checkout cart data structures.

### Architecture and UX guardrails

- Reuse Angular standalone component patterns in guided-discovery modules.
- Use Bootstrap responsive behavior for sticky/offcanvas layouts; avoid introducing a new UI framework.
- UX-DR4 requires compact compare with keyboard-first usability.

### Likely touch points

- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.html`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.ts`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.css`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.spec.ts`
- `sprint5/UI/src/assets/i18n/*.json`

### References

- [Source: `_bmad-output/planning-artifacts/epics.md` (Story 3.1)]
- [Source: `_bmad-output/planning-artifacts/prd.md` (FR16)]
- [Source: `_bmad-output/planning-artifacts/ux-design-specification.md` (UX-DR4)]
- [Source: `_bmad-output/planning-artifacts/architecture.md` (Angular/Bootstrap constraints)]

## Dev Agent Record

### Agent Model Used

Codex 5.3

### Debug Log References

- `ReadFile _bmad-output/planning-artifacts/epics.md` (Epic 3, Story 3.1)
- `ReadFile _bmad-output/implementation-artifacts/sprint-status.yaml` (story tracking update prep)
- `npm run --prefix "sprint5/UI" build -- --configuration development` (pass)
- `npm run --prefix "sprint5/UI" lint` (pass)
- `npm run --prefix "sprint5/UI" test -- --watch=false --browsers=ChromeHeadless` (blocked: ChromeHeadless cannot start in this environment)

### Completion Notes List

- Added compare selection action on recommendation cards with max-3 locking and user-visible limit messaging.
- Implemented compact compare tray behavior with sticky desktop mode and fixed mobile offcanvas-like mode.
- Added compare table for task-relevant attributes (price, availability, confidence, source).
- Implemented keyboard arrow navigation across compare table cells using row/column focus targeting.
- Ensured closing tray only affects compare state and does not mutate cart/recommendation state.
- Added component tests for compare tray open/close, max selection enforcement, and keyboard navigation behavior.

### File List

- `_bmad-output/implementation-artifacts/3-1-compact-compare-tray-2-3-skus.md`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.ts`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.html`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.css`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.spec.ts`
- `sprint5/UI/src/assets/i18n/en.json`
- `sprint5/UI/src/assets/i18n/de.json`
- `sprint5/UI/src/assets/i18n/es.json`
- `sprint5/UI/src/assets/i18n/fr.json`
- `sprint5/UI/src/assets/i18n/nl.json`
- `sprint5/UI/src/assets/i18n/tr.json`

### Change Log

- 2026-04-20: Implemented Story 3.1 compare tray selection/limit behavior, responsive tray modes, keyboard navigation, and supporting UI tests.

---

**Story completion status:** Comprehensive developer guide created for Story 3.1.
