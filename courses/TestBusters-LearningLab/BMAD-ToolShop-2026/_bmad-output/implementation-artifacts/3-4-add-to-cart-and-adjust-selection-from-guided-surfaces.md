# Story 3.4: Add to cart and adjust selection from guided surfaces

Status: review

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a **shopper**,
I want **to add from cards and adjust quantity before checkout**,
so that **I can finalize my basket efficiently**.

## Acceptance Criteria

1. **Given** in-stock recommendations
   **When** I click **Add to cart** (primary)
   **Then** cart service updates and confirmation matches existing shop behavior (FR20, FR21, UX-DR10).
2. **And** only one primary CTA dominates the card region (UX-DR10).

## Tasks / Subtasks

- [x] **Guided add-to-cart action wiring** (AC: 1)
  - [x] Add primary add-to-cart CTA on recommendation cards using existing cart service behavior.
  - [x] Show confirmation state consistent with current storefront/cart interaction patterns.
  - [x] Ensure add action honors current stock state constraints from Story 3.3.
- [x] **Quantity adjustment before checkout** (AC: 1)
  - [x] Provide quantity adjustment control path on guided/cart transition surfaces.
  - [x] Ensure quantity changes persist in cart and remain consistent with checkout expectations.
  - [x] Handle invalid quantity bounds gracefully.
- [x] **CTA hierarchy enforcement** (AC: 2)
  - [x] Keep a single dominant primary CTA in the card action region.
  - [x] Ensure secondary and accent actions remain visually subordinate.
- [x] **Tests and quality gates** (AC: 1, 2)
  - [x] Add component tests for add-to-cart success and quantity adjustment behavior.
  - [x] Add tests validating CTA hierarchy/class assignments for card action region.
  - [x] Run build/lint/tests and document environment blockers if present.

## Dev Notes

### Context and dependency notes

- Depends on Story 3.3 stock state handling and existing checkout/cart service contracts.
- Must preserve guided flow context while transitioning from recommendation decision to basket adjustment.

### Architecture and UX guardrails

- Reuse existing cart service and checkout data model patterns; avoid parallel cart logic.
- Respect UX-DR10 by keeping action hierarchy explicit and consistent.
- Keep guided experience responsive without introducing duplicate cart notifications.

### Likely touch points

- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.html`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.ts`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.css`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.spec.ts`
- `sprint5/UI/src/app/_services/cart.service.ts`
- `sprint5/UI/src/app/checkout/cart/*.ts`
- `sprint5/UI/src/app/checkout/cart/*.html`

### References

- [Source: `_bmad-output/planning-artifacts/epics.md` (Story 3.4)]
- [Source: `_bmad-output/planning-artifacts/prd.md` (FR20, FR21)]
- [Source: `_bmad-output/planning-artifacts/ux-design-specification.md` (UX-DR10)]
- [Source: `_bmad-output/planning-artifacts/architecture.md` (cart/checkout service boundaries)]

## Dev Agent Record

### Agent Model Used

Codex 5.3

### Debug Log References

- `ReadFile _bmad-output/planning-artifacts/epics.md` (Epic 3, Story 3.4)
- `ReadFile _bmad-output/implementation-artifacts/sprint-status.yaml` (story tracking update prep)
- `ReadFile sprint5/UI/src/app/checkout/cart/cart.component.ts` (align guided cart confirmation behavior)
- `npm run --prefix "sprint5/UI" build -- --configuration development`
- `npm run --prefix "sprint5/UI" lint`
- `npm run --prefix "sprint5/UI" test -- --watch=false --browsers=ChromeHeadless` (blocked: missing browser binary)

### Completion Notes List

- Implemented guided card add-to-cart flow using existing `CartService` contract and storefront-consistent `ToastrService` confirmation messages.
- Added per-recommendation quantity controls on guided cards, with normalization to supported bounds (`1..999999999`) before cart submission.
- Kept stock safeguards from Story 3.3 by blocking add attempts when cards are out-of-stock.
- Preserved CTA hierarchy with a single dominant primary action (`guided-primary-cta`) in card action regions and subordinate secondary/accent actions.
- Added component tests covering in-stock add, quantity normalization bounds, and CTA class hierarchy checks.
- Validation: UI build and lint pass; UI unit tests remain environment-blocked because `ChromeHeadless` cannot start in sandbox.

### File List

- `_bmad-output/implementation-artifacts/3-4-add-to-cart-and-adjust-selection-from-guided-surfaces.md`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.ts`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.html`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.css`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.spec.ts`

---

**Story completion status:** Comprehensive developer guide created for Story 3.4.
