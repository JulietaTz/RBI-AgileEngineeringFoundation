# Story 2.2: Explainability accordion and AI disclosure surfaces

Status: review

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a **shopper**,  
I want **optional deeper explanation without cluttering the card**,  
so that **I can dig in only when needed**.

## Acceptance Criteria

1. **Given** a recommendation card  
   **When** I expand "Why details"  
   **Then** accordion follows `aria-expanded` and moves focus into the panel (FR9, UX-DR3, NFR16).
2. **And** collapsed state is default (UX-DR3).

## Tasks / Subtasks

- [x] **Frontend: explainability accordion behavior** (AC: 1, 2)
  - [x] Add collapsible "Why details" section to recommendation cards.
  - [x] Default all panels to collapsed state.
  - [x] Wire `aria-expanded` and correct control/panel linkage.
- [x] **Accessibility and focus management** (AC: 1)
  - [x] Move keyboard focus into expanded panel when activated.
  - [x] Preserve logical focus return when panel closes.
  - [x] Ensure screen-reader friendly panel labels and descriptions.
- [x] **Disclosure surface consistency** (AC: 1)
  - [x] Ensure expanded details complement Story 2.1 one-line rationale and source metadata.
  - [x] Keep disclosure language concise and non-marketing.
- [x] **Tests and quality gates** (AC: 1, 2)
  - [x] Add component tests for collapsed-by-default behavior.
  - [x] Add tests for `aria-expanded` toggling and focus movement.
  - [x] Validate build/lint/tests and document blockers if environment-limited.

## Dev Notes

### Context and dependency notes

- Builds directly on Story 2.1 recommendation metadata surfaces.
- Must not regress Story 1.4/1.5 recommendation card actions and loading/error states.
- Keep card density balanced: short rationale visible, long explanation behind progressive disclosure.

### Architecture and UX guardrails

- Use Angular/Bootstrap collapse/accordion patterns already in project conventions.
- Preserve recommendation API/client flow through existing service boundaries.
- UX-DR3 requires accessible expandable panels with predictable keyboard behavior.

### Likely touch points

- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.html`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.ts`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.spec.ts`
- `sprint5/UI/src/assets/i18n/*.json`

### References

- [Source: `_bmad-output/planning-artifacts/epics.md` (Story 2.2)]
- [Source: `_bmad-output/planning-artifacts/prd.md` (FR9)]
- [Source: `_bmad-output/planning-artifacts/architecture.md` (UI/API split and accessibility constraints)]
- [Source: `_bmad-output/planning-artifacts/ux-design-specification.md` (UX-DR3)]

## Dev Agent Record

### Agent Model Used

Codex 5.3

### Debug Log References

- `ReadFile _bmad-output/planning-artifacts/epics.md` (Story 2.2)
- `rg _bmad-output/planning-artifacts/prd.md` for FR9
- `rg _bmad-output/planning-artifacts/ux-design-specification.md` for accordion/disclosure guidance
- `npm run --prefix "sprint5/UI" build -- --configuration development` (pass)
- `npm run --prefix "sprint5/UI" lint` (pass)
- `npm run --prefix "sprint5/UI" test -- --watch=false --browsers=ChromeHeadless` (blocked: ChromeHeadless cannot start in this environment)

### Completion Notes List

- Added a collapsible `Why details` disclosure control to each recommendation card with `aria-controls` and `aria-expanded` linkage.
- Implemented collapsed-by-default behavior and single-panel expansion state.
- Added focus management so expanding moves focus into the panel and collapsing returns focus to the toggle control.
- Kept disclosure copy concise and complementary to Story 2.1 rationale/source metadata.
- Added component tests for collapsed-by-default behavior and `aria-expanded` + focus movement behavior.
- Verified build and lint; unit test command remains environment-blocked due to missing ChromeHeadless runtime.

### File List

- `_bmad-output/implementation-artifacts/2-2-explainability-accordion-and-ai-disclosure-surfaces.md`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.ts`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.html`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.spec.ts`
- `sprint5/UI/src/assets/i18n/en.json`
- `sprint5/UI/src/assets/i18n/de.json`
- `sprint5/UI/src/assets/i18n/es.json`
- `sprint5/UI/src/assets/i18n/fr.json`
- `sprint5/UI/src/assets/i18n/nl.json`
- `sprint5/UI/src/assets/i18n/tr.json`

### Change Log

- 2026-04-20: Implemented Story 2.2 explainability accordion UI, accessibility/focus handling, and component tests; validated with build/lint and documented unit-test environment blocker.

---

**Story completion status:** Comprehensive developer guide created for Story 2.2.
