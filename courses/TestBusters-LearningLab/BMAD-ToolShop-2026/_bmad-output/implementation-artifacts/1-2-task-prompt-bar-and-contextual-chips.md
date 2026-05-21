# Story 1.2: Task prompt bar and contextual chips

Status: review

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a **shopper**,  
I want **to describe my job and refine it with chips (material, environment, budget)**,  
So that **the system can narrow recommendations to my situation**.

## Context (Epic 1)

Epic 1 establishes a guided discovery lane that starts from intent and progressively narrows to confident picks. Story 1.1 already shipped the entry points and `/guided` shell. Story 1.2 is the first story that captures structured input and submits it.

Cross-story sequencing:

- **Done in 1.1:** `/guided` route, shell component, header/CTA entry points, i18n baseline.
- **This story (1.2):** Task prompt UI, contextual chips, submit validation, API request payload.
- **Later (1.3+):** stable journey/session id, ranking response rendering, alternatives/refine loop, fallback hardening.

## Acceptance Criteria

1. **Prompt + chips capture on guided route**  
   **Given** the guided flow route is active  
   **When** I see the task input area  
   **Then** I can enter free text for my job and optionally choose chips for **material**, **environment**, and **budget**.

2. **Validated submit with accessible errors**  
   **Given** I press submit  
   **When** required input is missing or invalid  
   **Then** the client validates on submit and shows inline errors with `aria-invalid` and `aria-describedby` wired correctly to the message element.

3. **Recommendation API request shape**  
   **Given** a valid task prompt submission  
   **When** I submit  
   **Then** the client sends a recommendation request payload containing task text and selected chip context (FR2, FR3) via the app's Angular service layer (no direct HTTP in template/component markup).

4. **Escape hatch remains available**  
   **Given** I do not want to continue guided input  
   **When** I activate **Use search instead**  
   **Then** I can leave to standard search/browse without losing session continuity intent (FR7 foundation).

## Tasks / Subtasks

- [x] **Build task prompt component in guided module** (AC: 1, 4)
  - [x] Create a focused component under `sprint5/UI/src/app/guided-discovery/` (for example `task-prompt-bar.component.*`) and mount it from `guided-discovery-shell.component.html`.
  - [x] Implement free-text prompt + chip groups (material/environment/budget) using Bootstrap-first controls.
  - [x] Add a tertiary **Use search instead** control routing to the existing search path (`/` with `#search` fragment) consistent with Story 1.1 behavior.

- [x] **Implement form model + submit validation** (AC: 2)
  - [x] Use Angular Reactive Forms; validate at submit time for required task text.
  - [x] Bind `aria-invalid` only when invalid state is active and `aria-describedby` to a stable error message id.
  - [x] Keep keyboard flow intact; Enter submits from the prompt input.

- [x] **Add recommendation request service and payload mapping** (AC: 3)
  - [x] Introduce or extend a dedicated service in `sprint5/UI/src/app/_services/` (for example `recommendation-api.service.ts`) for request dispatch.
  - [x] Define request DTO/interface under `sprint5/UI/src/app/models/` for task + chips payload.
  - [x] Send request from component through service only; avoid direct API logic in template.
  - [x] For Story 1.2, UI may keep response handling minimal (pending Story 1.4), but request invocation must be real and testable.

- [x] **Internationalization and accessibility polish** (AC: 1, 2, 4)
  - [x] Add Transloco keys for labels, placeholders, chip labels, errors, submit, and escape link.
  - [x] Update all locale files in `sprint5/UI/src/assets/i18n/*.json` (`en`, `de`, `es`, `fr`, `nl`, `tr`) to avoid missing-key regressions.
  - [x] Ensure focus order and visible focus remain Bootstrap-consistent.

- [x] **Tests** (AC: 1-4)
  - [x] Unit/component spec: form validity rules, chip toggle behavior, `aria-invalid` and `aria-describedby` binding.
  - [x] Service spec: request payload mapping and HTTP contract shape.
  - [x] Optional E2E smoke (if workflow includes Playwright in this slice): submit valid task from `/guided` and assert request fired (attempted via Karma/FirefoxHeadless in this environment, blocked by missing browser binary).

## Dev Notes

### Previous story intelligence (1.1)

- `/guided` shell already exists at `sprint5/UI/src/app/guided-discovery/` and supports contextual query params (`category`, `product`).
- Header parity and fallback path already use search fragment behavior; reuse this pattern for **Use search instead**.
- i18n already introduced keys under `pages.guided` and `header.menu`; extend these namespaces rather than creating disconnected structures.
- Avoid reworking cart/header logic added in 1.1; keep story scope limited to guided prompt and request initiation.

### Architecture compliance

- Use **Angular 20** + existing module structure under `sprint5/UI`.
- Follow service-boundary pattern: component handles UI state, dedicated service handles HTTP.
- Keep file naming `kebab-case` and colocated specs.
- Do not add backend endpoints in this story file; consume existing or mocked recommendation endpoint contract from UI service layer.

### UX requirements to enforce

- Task entry is a first-class path and must not replace/hide classic search.
- Prompt bar should support compact/expanded behavior eventually; for this story, prioritize one clean, accessible implementation in guided shell.
- Recovery/escape path must stay visible and low friction.

### API and payload guardrails

- Use a single request model for task input and chips, with explicit fields.
- Normalize chip values before request (stable keys, not translated labels).
- Do not encode business logic in template strings or DOM attributes.

### Testing requirements

- Maintain current testing stack (Angular specs + existing tooling).
- Add focused tests rather than broad brittle snapshots.
- Validate accessibility bindings (`aria-invalid`, `aria-describedby`) in unit/component tests.

### Library / framework requirements

- Respect pinned versions already in workspace (`@angular/* 20.0.x`, `bootstrap 5.3.x`).
- Reuse existing Transloco and Router patterns introduced in Story 1.1.

### File structure requirements (expected touch list)

| Area | Path |
|------|------|
| Guided shell integration | `sprint5/UI/src/app/guided-discovery/guided-discovery-shell.component.html` |
| New prompt component | `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.ts` (+ html/css/spec) |
| Service layer | `sprint5/UI/src/app/_services/recommendation-api.service.ts` (new or update) |
| Models | `sprint5/UI/src/app/models/` (request interface for task/chips) |
| i18n | `sprint5/UI/src/assets/i18n/*.json` |

### Out of scope (explicit)

- Journey/session id persistence and `X-Journey-Id` header propagation (Story 1.3).
- Ranked recommendation rendering and cold-start card UX (Story 1.4).
- Alternatives/refine loop response UX (Story 1.5).
- Full fallback state management hardening (Story 1.6).

### Git intelligence summary

- Recent repo activity is heavily BMad artifact and skill updates plus Story 1.1 app changes; there is no stable existing recommendation UI service yet in sprint5 UI.
- Keep this story additive in `sprint5/UI` and avoid touching older sprint trees (`sprint1`-`sprint4`) unless strictly necessary.

### Latest technical information

- Angular reactive forms guidance continues to rely on standard semantic labels, clear error association, and ARIA wiring; `aria-invalid` and `aria-describedby` should be tied to actual invalid state and message id.
- Bootstrap 5.3 accessibility guidance emphasizes not relying on visual style/color alone and using contextual text (`visually-hidden` where needed) for status-like chip/badge content.

### References

- [Source: `_bmad-output/planning-artifacts/epics.md` (Epic 1, Story 1.2 ACs)]
- [Source: `_bmad-output/planning-artifacts/prd.md` (FR2, FR3, FR7)]
- [Source: `_bmad-output/planning-artifacts/architecture.md` (Angular brownfield boundaries, service patterns, naming conventions)]
- [Source: `_bmad-output/planning-artifacts/ux-design-specification.md` (task prompt bar pattern, accessibility and fallback expectations)]
- [Source: `_bmad-output/implementation-artifacts/1-1-storefront-entry-and-dual-rail-navigation-for-guided-discovery.md` (implemented routing/header/i18n context)]

## Dev Agent Record

### Agent Model Used

Codex 5.3

### Debug Log References

- `git log -5 --oneline`
- `git show --name-only --pretty=format:"%h %s" -5`
- `npm run --prefix "sprint5/UI" build -- --configuration development` (passes)
- `npm run --prefix "sprint5/UI" lint` (passes)
- `npm run --prefix "sprint5/UI" test -- --watch=false --include='src/app/guided-discovery/**/*.spec.ts' --include='src/app/_services/recommendation-api.service.spec.ts'` (blocked: Firefox binary unavailable in sandbox)

### Completion Notes List

- Story context created from epic/PRD/architecture/UX artifacts plus previous story implementation output and recent git activity.
- Guardrails include concrete paths, scope boundaries, and regression prevention points for Story 1.2.
- Implemented `TaskPromptBarComponent` on `/guided` with reactive form validation, chip selection state, and submit flow.
- Added `RecommendationApiService` and typed `RecommendationRequest` payload model for task/chip API calls.
- Wired a tertiary "Use search instead" escape path to `/#search`.
- Added service + component specs for payload mapping, chip behavior, and accessibility attributes.
- Extended `pages.guided.task` i18n keys across `en`, `de`, `es`, `fr`, `nl`, `tr`.

### File List

- `_bmad-output/implementation-artifacts/1-2-task-prompt-bar-and-contextual-chips.md`
- `_bmad-output/implementation-artifacts/sprint-status.yaml` (status updates to in-progress and review)
- `sprint5/UI/src/app/guided-discovery/guided-discovery-shell.component.ts`
- `sprint5/UI/src/app/guided-discovery/guided-discovery-shell.component.html`
- `sprint5/UI/src/app/guided-discovery/guided-discovery-shell.component.spec.ts`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.ts`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.html`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.css`
- `sprint5/UI/src/app/guided-discovery/task-prompt-bar.component.spec.ts`
- `sprint5/UI/src/app/_services/recommendation-api.service.ts`
- `sprint5/UI/src/app/_services/recommendation-api.service.spec.ts`
- `sprint5/UI/src/app/models/recommendation-request.ts`
- `sprint5/UI/src/assets/i18n/en.json`
- `sprint5/UI/src/assets/i18n/de.json`
- `sprint5/UI/src/assets/i18n/es.json`
- `sprint5/UI/src/assets/i18n/fr.json`
- `sprint5/UI/src/assets/i18n/nl.json`
- `sprint5/UI/src/assets/i18n/tr.json`

### Change Log

- 2026-04-20: Implemented Story 1.2 task prompt bar, contextual chips, validation/a11y wiring, recommendation request service, and locale updates; build + lint pass, test execution blocked by missing browser binary in environment.

---

**Story completion status:** Ultimate context engine analysis completed - comprehensive developer guide created.
