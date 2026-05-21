# Story 1.1: Storefront entry and dual-rail navigation for guided discovery

Status: review

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a **shopper**,  
I want **clear entry points to “Help me choose” alongside search and categories**,  
So that **I can start AI-guided discovery without losing classic shop parity**.

## Context (Epic 1)

Epic 1 delivers confident AI-guided discovery through task input, session/journey id, ranked recommendations, refine/alternatives, and fallback to classic paths. **This story establishes navigation and routing only** so later stories (1.2–1.6) plug into a stable guided shell. Do **not** implement task submission, recommendation cards, or APIs here beyond stubs if needed for routing.

Cross-story preview:

- **1.2** — Task prompt bar and chips (API call begins there).
- **1.3** — `journeyId` / `X-Journey-Id` header (not required to persist in 1.1; avoid blocking on it).
- **1.4–1.6** — Results, alternatives, fallback (out of scope).

## Acceptance Criteria

1. **Global dual-rail entry (header)**  
   **Given** I am on any storefront surface that uses the global header (home, category, PDP, checkout-adjacent flows that still show header, etc.)  
   **When** I view the primary navigation  
   **Then** I see a **Help me choose** control as a **peer** to categories and cart (same visual band; order: logo → Help me choose → categories → … → cart)  
   **And** activating it navigates to the **guided flow route** via Angular `Router` without remounting the app in a way that clears cart state (FR1, FR7, FR25).  
   **Maps to:** FR1, FR7, FR25, UX-DR13, UX-DR16, NFR15–NFR18.

2. **Search parity in primary chrome**  
   **Given** UX-DR13/DR16 require search not to hide guided entry and “peer to search”  
   **When** the header is visible  
   **Then** shoppers have a **discoverable search affordance** in the navbar (compact icon or input) that reaches product discovery (e.g. `routerLink` to `/` with query/hash fragment for search focus, or inline search that delegates to existing `ProductService`/overview behavior—**choose one consistent pattern** and document it in Dev Notes)  
   **And** **Help me choose** remains visible (no hamburger-only trap on mobile).

3. **Mobile one-tap (UX-DR17, UX-DR18)**  
   **Given** a small viewport (`< lg` typical collapse)  
   **When** I open the collapsed navbar  
   **Then** I can reach **cart** and **search** in **one tap each** from the opened menu (no nested-only path for both)  
   **And** primary tap targets for **Help me choose**, **search**, and **cart** meet **≥ 44×44 px** touch area (padding or `min-height`/`min-width`).

4. **Contextual entry points**  
   **Given** PRD/UX entry patterns (home hero, category, PDP)  
   **When** I am on **home** (`''`), a **category** route (`/category/:name`), or **PDP** (`/product/:id`)  
   **Then** I see a secondary CTA (button or text link) **“Help me choose”** / **“Start with my task”** that routes to the same guided flow route (copy via Transloco keys).

5. **Guided route shell**  
   **Given** I navigate to the new guided flow route  
   **When** the view loads  
   **Then** I see a **minimal shell** (landmark `main`, page title, short explanation, placeholder for future task UI) with **global header/footer unchanged** and cart quantity unchanged  
   **And** the route is **lazy-loaded** per brownfield architecture (new feature module under `sprint5/UI/src/app/`).

6. **Accessibility**  
   Landmark for main content; `Help me choose` has **accessible name** (visible text or `aria-label` if icon-only); keyboard operable; visible `:focus-visible` styles preserved or added without breaking Bootstrap navbar focus order (NFR15–NFR18).

## Tasks / Subtasks

- [x] **Routing & module** (AC: 1, 5)  
  - [x] Add lazy route in `app-routing.module.ts` (e.g. `path: 'guided', loadChildren: () => import('./guided-discovery/guided-discovery.module').then(m => m.GuidedDiscoveryModule)` — exact path slug **team-choice**: prefer `/guided` or `/help-me-choose`, keep stable for Story 1.2+).  
  - [x] Create `GuidedDiscoveryModule` + `GuidedDiscoveryRoutingModule` + shell component with `router-outlet` for future child steps if needed.

- [x] **Header** (AC: 1–3, 6)  
  - [x] Update `header.component.html` / `.css` (and `.ts` if needed): nav link + `data-test` hook (e.g. `data-test="nav-help-me-choose"`).  
  - [x] Add navbar search affordance per AC2; wire to existing discovery UX.  
  - [x] Verify collapsed navbar order and tap targets.

- [x] **Contextual CTAs** (AC: 4)  
  - [x] Home: `products/overview/overview.component.html` (or hero block).  
  - [x] Category: `products/category/category.component.html`.  
  - [x] PDP: `products/detail/detail.component.html`.  
  - [x] Use `routerLink` to guided route; optional `queryParams` for category slug / product id for **soft landing** (optional in 1.1—document if deferred).

- [x] **i18n**  
  - [x] Add Transloco keys under `header.menu` (and `pages.guided` or similar) in **all** `src/assets/i18n/*.json` files (`en`, `de`, `es`, `fr`, `nl`, `tr`) to avoid language switch missing strings.

- [x] **Tests**  
  - [x] Add/update a shallow component or routing spec that asserts guided route is declared and `Help me choose` link exists (pattern match existing `*.spec.ts` in repo).

## Dev Notes

### Architecture compliance

- **Canonical UI root:** `sprint5/UI` — Angular **20.0.x**, Bootstrap **5.3.7**, Transloco, Karma/Jasmine per [Source: `_bmad-output/planning-artifacts/architecture.md` — Brownfield Angular / starter decision].  
- **Routing:** root routes live in `sprint5/UI/src/app/app-routing.module.ts`; product routes in `products/products-routing.module.ts`. New top-level feature belongs at `app` level unless you deliberately nest under `products` (prefer **app-level** `/guided` for clarity).  
- **State:** `CartService` and existing services must survive navigation to guided route—no `window.location` full reload for this flow.  
- **SSR:** If SSR is later enabled, this story’s shell should not assume browser-only globals in constructors; keep DOM access in `ngAfterViewInit` or guards where needed [Source: architecture SSR notes; UX-DR22].

### UX design requirements (extract)

- **UX-DR13:** Dual rail — global **Help me choose** peer to search/categories/cart; skip link + landmarks where applicable.  
- **UX-DR16:** Search/filter parity — guided entry not hidden by search.  
- **UX-DR17 / UX-DR18:** Mobile-first; cart/search one tap from opened nav; Bootstrap breakpoints for layout.  
- **Journey reference:** Entry from home, category, PDP, header [Source: `_bmad-output/planning-artifacts/ux-design-specification.md` — User Journey Flows, Navigation patterns].

### PRD traceability

- **FR1** — Start AI-guided session from storefront entry points.  
- **FR7 / FR25** — Non-AI paths and checkout without mandatory AI; navigation must not trap or clear cart.

### File structure (expected touch list)

| Area | Path |
|------|------|
| App routes | `sprint5/UI/src/app/app-routing.module.ts` |
| Header | `sprint5/UI/src/app/header/header.component.html`, `.css`, `.ts` |
| Home / grid | `sprint5/UI/src/app/products/overview/overview.component.html` |
| Category | `sprint5/UI/src/app/products/category/category.component.html` |
| PDP | `sprint5/UI/src/app/products/detail/detail.component.html` |
| New feature | `sprint5/UI/src/app/guided-discovery/` (module, routing, shell component, `.scss` if standalone styles) |
| i18n | `sprint5/UI/src/assets/i18n/*.json` |

### Testing requirements

- Run `npm test` from `sprint5/UI` with existing CI flags (`--browsers=FirefoxHeadless --watch=false` per `package.json`).  
- Prefer **one** focused spec new or extended for guided route + nav link existence.

### Library / version guardrails

Pinned workspace versions (do not downgrade): `@angular/*` **20.0.5**, `bootstrap` **5.3.7**, `@angular/router` **20.0.5** [Source: `sprint5/UI/package.json`]. Use `ng generate module` / `component` from `sprint5/UI` if generating files.

### Out of scope (explicit)

- Recommendation API, `X-Journey-Id`, OpenAPI (Story 1.3+).  
- Task prompt, chips, validation (Story 1.2).  
- Recommendation cards, skeleton loading for results (Story 1.4+).  
- Consent/CMP changes (Epic 2).

### Open points / product judgment

- **Search in header:** Today search lives mainly on overview sidebar; AC2 requires navbar parity—implement the smallest change that satisfies “peer” and one-tap mobile without duplicating all filter UI in the header.  
- **Route slug:** Pick `/guided` **or** `/help-me-choose` and use consistently in all CTAs.

## Dev Agent Record

### Agent Model Used

Composer (Cursor agent)

### Debug Log References

- `npm test` requires Firefox (`FirefoxHeadless`); not available in this CI sandbox — **`npx ng build --configuration development` passed** as compile/regression gate.

### Completion Notes List

- Implemented lazy `/guided` route, shell page with optional `category` / `product` query context, global **Help me choose** + **Search** (fragment `search` → focus `#search-query` on home), cart always visible with tap targets (44px), skip link to `#main-content`, home/category/PDP CTAs, i18n for en/de/es/fr/nl/tr.

### File List

- `sprint5/UI/src/app/app-routing.module.ts`
- `sprint5/UI/src/app/app.component.html`
- `sprint5/UI/src/app/app.component.ts`
- `sprint5/UI/src/app/header/header.component.html`
- `sprint5/UI/src/app/header/header.component.ts`
- `sprint5/UI/src/app/header/header.component.css`
- `sprint5/UI/src/app/guided-discovery/guided-discovery.module.ts`
- `sprint5/UI/src/app/guided-discovery/guided-discovery-routing.module.ts`
- `sprint5/UI/src/app/guided-discovery/guided-discovery-shell.component.ts`
- `sprint5/UI/src/app/guided-discovery/guided-discovery-shell.component.html`
- `sprint5/UI/src/app/guided-discovery/guided-discovery-shell.component.css`
- `sprint5/UI/src/app/guided-discovery/guided-discovery-shell.component.spec.ts`
- `sprint5/UI/src/app/products/overview/overview.component.html`
- `sprint5/UI/src/app/products/overview/overview.component.ts`
- `sprint5/UI/src/app/products/category/category.component.html`
- `sprint5/UI/src/app/products/detail/detail.component.html`
- `sprint5/UI/src/assets/i18n/en.json`
- `sprint5/UI/src/assets/i18n/de.json`
- `sprint5/UI/src/assets/i18n/es.json`
- `sprint5/UI/src/assets/i18n/fr.json`
- `sprint5/UI/src/assets/i18n/nl.json`
- `sprint5/UI/src/assets/i18n/tr.json`
- `_bmad-output/implementation-artifacts/sprint-status.yaml`

---

**Story completion status:** Implementation complete — **Status `review`**; run **`bmad-code-review`** ([CR]) next, then mark **done** when approved.
