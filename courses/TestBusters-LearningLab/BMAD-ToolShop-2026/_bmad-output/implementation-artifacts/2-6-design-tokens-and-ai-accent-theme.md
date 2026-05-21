# Story 2.6: Design tokens and AI accent theme

Status: review

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a **shopper**,  
I want **consistent visual treatment of AI vs primary commerce actions**,  
so that **the UI feels coherent and accessible**.

## Acceptance Criteria

1. **Given** Bootstrap theme variables  
   **When** tokens are applied to guided surfaces  
   **Then** primary, secondary, and AI accent meet WCAG 2.1 AA contrast checks documented in story evidence (UX-DR8, NFR15).
2. **And** AI accent is not applied to every CTA (UX-DR10 scope).

## Tasks / Subtasks

- [x] **Token mapping for guided surfaces** (AC: 1, 2)
  - [x] Define or update CSS token mappings for primary/secondary/AI accent.
  - [x] Apply token usage on guided UI surfaces and recommendation components.
  - [x] Keep CTA hierarchy consistent (single dominant primary action, selective accent use).
- [x] **Contrast and accessibility validation** (AC: 1)
  - [x] Validate contrast combinations meet WCAG 2.1 AA targets.
  - [x] Document contrast evidence in `_bmad-output/test-artifacts/2-6-contrast-evidence.md` including token pairs, measured ratios, and pass/fail outcome.
- [x] **Theme consistency and regression checks** (AC: 1, 2)
  - [x] Ensure existing commerce surfaces are not unintentionally recolored.
  - [x] Preserve readability across supported breakpoints/states.
- [x] **Tests and quality gates** (AC: 1, 2)
  - [x] Add/extend UI tests where feasible for class/token assignment.
  - [x] Run lint/build validation and document blockers if test runtime constraints exist.

## Dev Notes

### Context and dependency notes

- This story is visual-system oriented and should avoid behavioral regressions in recommendation flows.
- Build on existing Bootstrap theme strategy already used by UI.
- Evidence artifact for review must be produced at `_bmad-output/test-artifacts/2-6-contrast-evidence.md`.

### Architecture and UX guardrails

- UX-DR8 requires token-based theming and AA contrast compliance.
- UX-DR10 requires strict CTA hierarchy; AI accent is contextual, not universal.
- Do not introduce a second UI design system.

### Likely touch points

- `sprint5/UI/src/styles.css` and/or theme variable files
- `sprint5/UI/src/app/guided-discovery/*.html`
- `sprint5/UI/src/app/guided-discovery/*.css`
- `sprint5/UI/src/assets/i18n/*.json` (if label changes are required)

### References

- [Source: `_bmad-output/planning-artifacts/epics.md` (Story 2.6)]
- [Source: `_bmad-output/planning-artifacts/prd.md` (NFR15)]
- [Source: `_bmad-output/planning-artifacts/ux-design-specification.md` (UX-DR8, UX-DR10)]
- [Source: `_bmad-output/planning-artifacts/architecture.md` (Bootstrap/Angular consistency constraints)]

## Dev Agent Record

### Agent Model Used

Codex 5.3

### Debug Log References

- `ReadFile _bmad-output/planning-artifacts/epics.md` (Story 2.6)
- `rg _bmad-output/planning-artifacts/prd.md` for NFR15
- `rg _bmad-output/planning-artifacts/ux-design-specification.md` for UX-DR8/UX-DR10
- `rg _bmad-output/planning-artifacts/architecture.md` for UI stack constraints
- `npm run --prefix "sprint5/UI" build -- --configuration development` (pass)
- `npm run --prefix "sprint5/UI" lint` (pass)
- `npm run --prefix "sprint5/UI" test -- --watch=false --browsers=ChromeHeadless` (blocked: ChromeHeadless cannot start in this environment)

### Completion Notes List

- Added guided-surface theme tokens (`--guided-primary-*`, `--guided-secondary-*`, `--guided-ai-accent-*`) in global styles.
- Applied selective token classes to guided CTAs to preserve hierarchy: dominant primary submit, contextual AI accent on `More like this`, and secondary styling on `Refine`.
- Kept AI accent scoped and not universally applied across all CTA buttons.
- Added UI test assertions for token class assignment on primary/secondary/accent buttons.
- Produced required contrast evidence artifact at `_bmad-output/test-artifacts/2-6-contrast-evidence.md` with token pair ratios and WCAG pass/fail outcomes.

### File List

- `_bmad-output/implementation-artifacts/2-6-design-tokens-and-ai-accent-theme.md`
- `sprint5/UI/src/styles.css`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.css`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.html`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.spec.ts`
- `_bmad-output/test-artifacts/2-6-contrast-evidence.md`

### Change Log

- 2026-04-20: Implemented Story 2.6 guided theme token mapping, selective AI accent usage, CTA hierarchy styling, class-assignment test coverage, and WCAG contrast evidence artifact.

---

**Story completion status:** Comprehensive developer guide created for Story 2.6.
