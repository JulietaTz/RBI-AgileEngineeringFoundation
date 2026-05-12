---
stepsCompleted:
  - 1
  - 2
  - 3
  - 4
  - 5
  - 6
  - 7
  - 8
inputDocuments:
  - _bmad-output/planning-artifacts/prd.md
  - _bmad-output/planning-artifacts/ux-design-specification.md
workflowType: architecture
project_name: practice-software-testing
user_name: Rudolfgroetz
date: 2026-04-19
lastStep: 8
status: complete
completedAt: 2026-04-19
---

# Architecture Decision Document

_This document builds collaboratively through step-by-step discovery. Sections are appended as we work through each architectural decision together._

## Project Context Analysis

### Requirements Overview

**Functional Requirements**

The PRD defines **68 FRs** in ten capability areas. Architecturally, they imply:

- **Shopper path:** session-scoped AI discovery (NL task + context chips), ranked recommendations with explanations and source disclosure (AI vs rule vs curated), cold-start behavior, compare-on-task-attributes, cart/checkout handoff with **journey continuity** and **conversion attribution** (FR1–FR8, FR9–FR15, FR16–FR21, FR22–FR26).
- **Trust & data subject rights:** consent before telemetry, minimization, deletion, guest vs authenticated behavior (FR27–FR32, FR63–FR64).
- **Operations & product:** configurable rules, staging, feature exposure, revert, KPIs, experiments and variant assignment (FR33–FR44).
- **Support & audit:** session replay of inputs/outputs, root-cause taxonomy, escalation, immutable admin/rule change history, incident diagnostics (FR45–FR50, FR49, FR65, FR67).
- **Analytics:** impression/click/conversion pipeline, funnels, segment and source breakdowns, time-to-decision support (FR51–FR56).
- **Platform:** governed recommendation **API**, metadata-rich responses, telemetry ingestion, auth, rate limits, backward-compatible versioning (FR57–FR62).
- **Resilience & governance:** degradation modes, bias/trust risk evaluation hooks (FR66, FR68).

**Non-Functional Requirements**

Drivers for system design include: **sub-500 ms** user-facing recommendation latency; **near–real-time** stock at decision points; **99.5%** monthly availability with checkout-adjacent protection; **WCAG 2.1 AA** and full keyboard/screen-reader coverage on storefront, recommendation, and checkout; **encryption** in transit and at rest; **least-privilege RBAC** for config, diagnostics, and audit; **consent-gated** analytics; **GDPR** and **DORA**-aligned monitoring, incident process, and evidence; **versioned** integrations with **gateway**-style governance; **separation** of PII vs analytics contexts; **retention/deletion** SLAs and right-to-erasure propagation; **graceful fallback** when dependencies or recommendation components fail.

**Scale & complexity**

- **Primary domain:** Full-stack **web commerce** (Angular SPA/SSR as stated in PRD) plus **recommendation/intelligence services**, **telemetry/analytics**, and **internal tooling** (ops, support, experiments).
- **Complexity level:** **High** — matches PRD `classification.complexity` and multi-framework compliance (GDPR, DORA, ISO/SOC2/NIST expectations in domain section).
- **Estimated architectural components (coarse):** storefront/UI extensions; recommendation orchestration; ranking/rules engine; model or external LLM adapter (future); product/catalog integration; inventory/pricing; session/journey store; telemetry pipeline; consent/CMP integration; admin & support UIs or modules; API gateway concerns; audit log pipeline; experiment/flag service; analytics warehouse or lake path (exact topology TBD in later steps).

### Technical Constraints & Dependencies

- **Brownfield:** Existing **Angular** applications (e.g. sprint-based `UI/` trees) with established patterns (routing, `HttpClient`, interceptors, guards, i18n). New features should align with current bootstrap and module boundaries unless a deliberate strangler migration is chosen later.
- **PRD-stated stack direction:** **Angular SPA + SSR (Universal)** for SEO entry and rich in-session recommendation UX.
- **External dependencies called out in PRD:** OAuth2/OIDC IdPs; **CMP** (e.g. OneTrust/Usercentrics); **API gateway**; **SIEM**; recommendation consumers internal/partner.
- **Performance budget:** recommendation path must stay within **500 ms** normal conditions—implies caching, regional placement, async enrichment limits, or edge/BFF patterns to be decided.
- **Data:** strict **PII vs analytics** separation; retention windows (e.g. 90–180 days interaction telemetry per PRD domain section).

### Cross-Cutting Concerns Identified

- **Observability & audit:** traces/logs/metrics for latency SLOs; immutable audit for rule changes and admin actions; support-facing decision traces.
- **Security & privacy:** consent, minimization, erasure, encryption, RBAC, gateway authZ/rate limits.
- **Feature delivery:** flags and staged rollout for recommendation behavior (FR35–FR37, FR44).
- **Resilience:** degraded recommendation modes without blocking checkout (FR66, NFR reliability).
- **Accessibility & UX performance:** WCAG AA and sub-500 ms *perceived* interaction (UX spec + NFR)—frontend architecture must avoid layout thrash and support SSR/hydration strategy.
- **Experimentation integrity:** variant stickiness and unbiased measurement (FR41–FR43).

*Step 2 saved on stakeholder **Continue (`c`)**.*

## Starter Template Evaluation

### Primary Technology Domain

**Full-stack web application** — Angular storefront (PRD + UX + repo) with backend commerce and platform services (Laravel API in repo) to be extended for recommendations, telemetry, and governance.

### Starter Options Considered

| Option | Assessment |
|--------|----------------|
| **Angular CLI greenfield** (`ng new`, optional `--ssr`) | Official baseline for *new* frontends; SSR via `@angular/ssr` per [Angular SSR guide](https://angular.dev/guide/ssr). Not a whole-repo replacement given **brownfield** classification. |
| **Nx / monorepo** | Optional if multiple deployable apps need shared tooling; adds ceremony—**deferred** by default. |
| **Third-party Angular boilerplates** | Conflicts with existing **Bootstrap 5** stack and UX tokens—**rejected** as primary. |
| **Brownfield continuation + CLI schematics (selected)** | Extend the **existing Angular workspace** (e.g. `sprint5/UI`: **@angular/\* 20.0.5**, **@angular/cli 20.0.4** per `package.json`). Add routes/components with **`ng generate`**. Enable SSR when ready with **`ng add @angular/ssr`**. Plan major upgrades using **`ng update`** and [Angular releases](https://angular.dev/reference/releases). |

### UX alignment

Bootstrap-first UI, WCAG 2.1 AA, keyboard/list semantics, skeleton-first loading, and **<500 ms** perceived recommendation steps favor **official Angular + minimal add-ons** over animation-heavy or alternate-CSS starters.

### Selected starter: Brownfield Angular app (official CLI toolchain)

**Rationale:** PRD brownfield context, existing interceptors/guards/i18n, and UX-mandated Bootstrap/CDK patterns are already embodied in sprint UI trees; greenfield starters would duplicate decisions and risk drift.

**Reference commands**

Greenfield (illustrative only):

```bash
ng new shop-ssr --ssr --routing --style=scss
```

Brownfield SSR (run from the **canonical** UI root you adopt, e.g.):

```bash
cd sprint5/UI
ng add @angular/ssr
```

**Architectural decisions already implied by this baseline**

- **Language & runtime:** TypeScript; Zone.js as in current app unless you later ADR a zoneless migration.
- **Styling:** Bootstrap 5 + tokens; not the CLI default bare CSS stack.
- **Build:** `@angular-devkit/build-angular`; SSR adds server/prerender targets when the schematic is applied.
- **Testing:** Karma + Jasmine (present); changing runner is a separate decision.
- **Structure:** Match **one** paradigm in the chosen app (e.g. standalone `bootstrapApplication` in sprint5 vs NgModule in older sprints) for new recommendation surfaces.
- **DX:** `ng serve`, ESLint, existing HTTP/auth patterns.

**Note:** First implementation milestone should be “nominate canonical `UI/` workspace + SSR schematic (if required) + version alignment,” not importing an external starter repo.

*Step 3 saved on stakeholder **Continue (`c`)**.*

## Core Architectural Decisions

### Decision priority analysis

**Critical (block wrong implementation)**

- Canonical **sprint roots** for shop vertical: **`sprint5/UI`** + **`sprint5/API`** unless you explicitly fork a new generation.
- **REST + OpenAPI** as the public integration style (existing `api-docs.json` OpenAPI 3.0); recommendation and telemetry endpoints extend this with **versioned** paths or headers (FR60, FR62).
- **Consent-gated telemetry** end-to-end: browser only fires GA4/recommendation analytics after CMP allows; server rejects or no-ops PII analytics without consent context (FR29–FR30, FR51).
- **Journey/session correlation** ID propagated from Angular through checkout for attribution and support (FR26, FR45–FR46).

**Important (shape the system)**

- **Recommendation service boundary:** implement first as **Laravel modules/services** + queues calling a **ranking engine** (rules + optional external model); extract to dedicated microservice only if latency or team boundaries force it (NFR latency).
- **Redis** for cache, rate limiting, and hot read models (already via Predis); **MySQL** default OLTP per `.env.example` (`DB_CONNECTION=mysql`); target a supported **MySQL 8.4.x** LTS train in production (verify patch with ops—Oracle release notes as of early 2026).
- **AuthN:** keep **JWT** for API consumers and SPA pattern in use; add **OAuth2/OIDC** for enterprise admin/support paths per PRD via Laravel **Socialite** or gateway broker—exact IdP wiring in env-specific config.
- **AuthZ:** **RBAC** in Laravel for operations, support, governance APIs; audit every privileged change (FR49, FR65).

**Deferred (post-MVP / ADR later)**

- Dedicated **data warehouse** vs warehouse-lite in MySQL for analytics (start with event tables + export).
- **Nx** monorepo or splitting Angular apps.
- **Zoneless** Angular and major version jump (20 → current Angular release train) as a scheduled upgrade epic, not mixed into first AI slice.

### Data architecture

- **Primary OLTP:** **MySQL** (brownfield default); use **Laravel migrations** for all new recommendation, experiment, audit, and telemetry tables; enforce **retention** jobs (FR63) and **erasure** propagation (FR31, FR64).
- **Cache / ephemeral:** **Redis** for session-scoped recommendation context, feature-flag reads, and throttling counters; TTL aligned to PRD interaction retention (90–180 days guidance for raw events—implement as rolling purge job).
- **PII vs analytics:** separate logical schemas or table prefixes + strict service-layer access; no analytics pipeline reads raw PII without pseudonymization (FR64).
- **Model / training data:** store **pseudonymized** feature exports outside hot OLTP when ML expands (PRD domain section).

### Authentication & security

- **Transport:** TLS everywhere; HSTS at edge; Laravel encryption for secrets at rest.
- **API security:** gateway-level (PRD) **authN**, **rate limits**, request size limits; internal service accounts for batch jobs.
- **Admin/support surfaces:** MFA where available (Google2FA already in dependencies); SIEM-forwarded security logs for auth failures and admin actions.
- **CMP:** first-party consent string stored server-side where needed to justify downstream processing.

### API & communication patterns

- **External and SPA-facing:** **REST JSON**, **OpenAPI** published (`darkaonline/l5-swagger` / `zircote/swagger-php` pattern); problem+json or Laravel validation error shape as standard error envelope.
- **Versioning:** URL prefix (`/v1/...`) or explicit `Accept-Version`; breaking changes require new major version and deprecation window (FR60).
- **Async:** Laravel **queues** for non-latency-critical scoring refresh, bulk exports, and long audit writes; **synchronous** path for shopper recommendation request under **500 ms** budget (cache + precomputed ranks).
- **Idempotency:** telemetry POST endpoints accept client keys for dedupe where retries occur.

### Frontend architecture

- **Angular** feature area for AI: lazy route(s) under main shell; **services** for recommendation API; minimal global state—prefer **session + route state** + `BehaviorSubject` only where multiple siblings need sync.
- **SSR:** enable with **`ng add @angular/ssr`** on canonical app when SEO routes need it; hydrate carefully around **CMP** and third-party scripts.
- **Components:** align with UX spec—Bootstrap + small set of **owned** AI components; **Angular CDK** for overlays/focus only.
- **Performance:** route-level code split; fixed-height skeletons; avoid `*ngFor` on large catalog in AI panel (virtual scroll if lists grow).

### Infrastructure & deployment

- **Hosting:** not fixed in PRD—assume **containerized** Laravel + PHP-FPM and static/SSR Angular build behind reverse proxy or CDN; **horizontal scale** stateless API + sticky sessions only if required (prefer JWT/stateless).
- **CI/CD:** lint, unit, contract (Pact present)—extend with **a11y** and **OpenAPI diff** gates for recommendation routes.
- **Observability:** OpenTelemetry-compatible traces from Laravel middleware; correlate with **journey ID**; metrics for p95 recommendation latency and error budget (99.5% NFR).
- **Feature flags:** start with **DB + Redis** backed flags (FR36) or integrate LaunchDarkly/Unleash later—document provider in first ops story.

### Decision impact analysis

**Implementation sequence (suggested)**

1. Nominate **canonical** UI/API roots and enable **SSR** only if needed for entry pages.  
2. Add **recommendation** REST endpoints + OpenAPI + Laravel domain services + Redis cache.  
3. Wire **Angular** task UI to API with skeleton UX and **journey** header.  
4. **Telemetry** + consent checks (client + server).  
5. **Operations** rules CRUD + audit log.  
6. **Support** session inspection read API.  
7. Hardening: rate limits, experiments, erasure jobs.

**Cross-component dependencies**

- CMP and consent state **block** analytics payloads and some personalization headers.  
- **JWT/session** choices affect recommendation **guest vs authenticated** flows (FR28).  
- **Experiment variant** must be assigned early and stored with **journey** for unbiased funnels (FR41–FR42).  
- **Stock/pricing** integration must stay on the hot path—cache invalidation rules tie catalog DB to recommendation ranking.

*Step 4 saved on stakeholder **Continue (`c`)**.*

## Implementation Patterns & Consistency Rules

### Pattern categories defined

**Critical conflict points (12):** DB vs DTO naming, JSON casing, REST path pluralization, error envelope, date/time format, journey/correlation headers, event names, Angular file vs symbol names, test location, OpenAPI operationIds, log field keys, loading/error UI contract between AI components and shared layout.

### Naming patterns

**Database (Laravel / MySQL)**

- Tables: **plural**, `snake_case` (e.g. `recommendation_events`, `experiment_assignments`).
- Columns: **`snake_case`**; primary key `id` BIGINT UNSIGNED unless UUID chosen by ADR.
- Foreign keys: `{table_singular}_id`; indexes `idx_{table}_{columns}`.

**REST API**

- Paths: **plural** resource nouns, kebab-case multiwords if needed (`/v1/recommendation-sessions`).
- Path params: `:uuid` or `:id` consistent per resource type; document in OpenAPI.
- Headers: `X-Journey-Id` (or `X-Request-Id` + `X-Journey-Id` pair)—**one chosen set** per environment; never mix casings (`X-Journey-ID` vs `X-Journey-Id`).
- Query params: **`snake_case`** for public API consistency with Laravel query string conventions.

**Laravel code**

- Classes: `PascalCase`; methods: `camelCase`; config keys `snake_case`.

**Angular code**

- **Files:** `kebab-case.*` (e.g. `task-prompt-bar.component.ts`).
- **Components / directives:** `PascalCase` symbols matching default export.
- **Selectors:** `app-kebab-case` for app-wide; feature prefix for AI (`app-ai-...`) to avoid collisions.
- **TS members:** `camelCase`; **template** refs `camelCase`.

### Structure patterns

**Laravel (`sprint5/API`)**

- New recommendation domain: `app/Models/Recommendation/`, `app/Services/Recommendation/`, `app/Http/Controllers/Api/V1/Recommendation/`, Form Requests under `app/Http/Requests/Api/V1/`.
- Migrations: `database/migrations/` with timestamp prefix; never hand-edit applied migrations.
- Feature tests: `tests/Feature/Api/V1/`; unit tests for pure ranking: `tests/Unit/Recommendation/`.

**Angular (`sprint5/UI`)**

- AI feature: under `src/app/recommendation/` (or agreed feature folder)—**components**, **services**, **models** (interfaces), **routes** co-located by feature.
- Shared UI: existing shared module/pattern only; do not duplicate `HttpClient` wrappers—extend one `RecommendationApiService`.

**Documentation**

- OpenAPI annotations live **next to controllers** (existing swagger-php style); export regenerates `storage/api-docs/api-docs.json` in CI.

### Format patterns

**API JSON**

- **New** recommendation and telemetry endpoints: **`camelCase`** keys in JSON bodies and success payloads to align with TypeScript and reduce mapping bugs; document in OpenAPI.
- **Legacy** endpoints: **do not rename fields** until explicit versioning/migration story.
- Dates: **ISO 8601** UTC strings (`2026-04-19T14:32:01Z`) in JSON; never epoch-only in externally visible APIs.
- Errors: Laravel-style **`{ "message": string, "errors"?: { field: string[] } }`** for validation; use **HTTP problem** shape (`type`, `title`, `status`, `detail`) only if introduced consistently with a shared exception handler (pick one per API version).

**Booleans / null**

- JSON `true`/`false`; omit optional fields vs explicit `null`—choose per DTO and document; default **omit** unknown optional.

### Communication patterns

**Domain events (Laravel)**

- Class names: `RecommendationRanked` (past tense outcome) or `RecommendationRequested`—pick **past tense for completed** events only; use one convention.
- Queue payload: serializable models by id, not full ORM graphs unless snapshot intentional.

**Angular state**

- **Immutable** updates for shared observables; no mutating arrays in place when using `async` pipe consumers.
- **Single** `RecommendationState` service per lazy route tree unless ADR splits read vs write.

**Logging**

- Structured JSON logs: **`level`**, **`message`**, **`journey_id`**, **`user_id`** (pseudonymous), **`route`**, **`latency_ms`**; never log full recommendation model or raw PII fields.

### Process patterns

**Errors**

- Angular: central `HttpErrorHandler` maps 401/403/429/5xx to toasts vs inline per UX spec; **always** offer retry/refine/search triad on recommendation 5xx.
- Laravel: map domain failures to typed HTTP exceptions; no raw stack traces to clients.

**Loading**

- Named flags: `isRecommendationsLoading` (boolean), not `loading` alone; pair with skeleton slot IDs for a11y.

### Enforcement guidelines

**All AI agents MUST**

- Follow naming and folder rules above for **new** files; do not introduce a second JSON casing style on the same API version.
- Add or update **OpenAPI** for every new public route before merge.
- Propagate **`X-Journey-Id`** (or chosen header) from Angular on recommendation and checkout-related calls when present.
- Run **Pint** / **ESLint** and existing test targets for touched packages.

**Pattern enforcement**

- CI: OpenAPI diff, PHPStan/Psalm if enabled, Angular lint, unit tests for ranking pure functions.
- PR template checklist: naming, journey header, consent gate, no PII in logs.

### Pattern examples

**Good**

- `GET /v1/recommendations` returns `{ "items": [ { "productId": 1, "score": 0.92, "rationale": "..." } ] }` with ISO timestamps.
- Migration `2026_04_19_000000_create_recommendation_events_table.php` with plural table name.

**Anti-patterns**

- Mixing `product_id` and `productId` in the same new endpoint.
- Logging full task description containing unchecked user PII in production log level `info`.
- Second ad-hoc HTTP client for recommendations beside the shared service.

*Step 5 saved on stakeholder **Continue (`c`)**.*

## Project Structure & Boundaries

### Complete project directory structure

High-signal layout for **practice-software-testing** (canonical **sprint5**). Older `sprint1`–`sprint4` trees remain reference-only unless explicitly adopted.

```
practice-software-testing/
├── _bmad-output/planning-artifacts/
│   ├── prd.md
│   ├── ux-design-specification.md
│   └── architecture.md
├── sprint5/
│   ├── API/                                 # Laravel 12 + PHP 8.3
│   │   ├── app/
│   │   │   ├── Http/Controllers/            # existing resource controllers
│   │   │   ├── Http/Controllers/Api/V1/Recommendation/   # (new)
│   │   │   ├── Http/Requests/Api/V1/        # (new) Form requests
│   │   │   ├── Http/Middleware/
│   │   │   ├── Models/                      # Product, User, Cart, …
│   │   │   ├── Models/Recommendation/       # (new)
│   │   │   ├── Services/                    # existing domain services
│   │   │   ├── Services/Recommendation/     # (new)
│   │   │   ├── Jobs/
│   │   │   ├── Console/
│   │   │   ├── Providers/
│   │   │   └── Policies/
│   │   ├── routes/api.php                   # add /v1/recommendation-* group (new)
│   │   ├── database/migrations/             # (new) recommendation, audit, experiment tables
│   │   ├── tests/Feature/Api/V1/            # (new)
│   │   ├── tests/Unit/Recommendation/       # (new)
│   │   ├── storage/api-docs/                # generated OpenAPI
│   │   ├── composer.json
│   │   └── .env.example
│   └── UI/                                  # Angular 20 + Bootstrap 5
│       ├── angular.json
│       ├── package.json
│       └── src/
│           └── app/
│               ├── app.component.*
│               ├── app-routing.module.ts
│               ├── header/ footer/ checkout/ products/ auth/ account/ admin/
│               ├── chat-widget/             # existing NL-style UI
│               ├── recommendation/          # (new) task flow, cards, compare
│               ├── _services/               # + recommendation-api.service (new)
│               ├── _helpers/                # interceptors (+ journey header) (modify)
│               └── models/                  # + recommendation DTOs (new)
│           ├── assets/
│           └── environments/
└── docs/                                    # project knowledge (optional; empty today)
```

### Architectural boundaries

**API boundaries**

- **Public REST:** Laravel `routes/api.php` with versioned groups. Existing catalog/cart/checkout unchanged; **new** `/v1/recommendation-*` behind same auth/rate-limit stack.
- **Internal:** Queues/workers call `Services\Recommendation\*` through service layer; no ad-hoc SQL from UI.
- **Auth:** JWT for shopper API; admin/support routes use guards + policies (extend existing).

**Component boundaries**

- **Angular:** `RecommendationApiService` is the sole HTTP client for recommendation JSON; components are presentational + local UI state.
- **Laravel:** Controllers thin; **FormRequest** validates; **Service** owns rules; **Model** for persistence.

**Service boundaries**

- **Catalog/stock:** existing product/cart services remain source of truth; recommendation **reads** via them or cached projections.
- **Telemetry:** append-only writer module; no circular dependency on ranking core.

**Data boundaries**

- **MySQL:** transactional + new recommendation/audit tables.
- **Redis:** namespaced keys, explicit TTLs.
- **PII:** MySQL under GDPR controls; analytics use pseudonymous identifiers.

### Requirements to structure mapping

| PRD FR group | Primary location |
|--------------|------------------|
| AI-guided discovery (FR1–FR8) | `UI/.../recommendation/` + `RecommendationApiService` + `API/.../Recommendation/` |
| Transparency & trust (FR9–FR15) | UI explain surfaces + API DTOs + support read APIs |
| Evaluation & decision (FR16–FR21) | UI compare + PDP links; Laravel read models for suitability |
| Checkout & conversion (FR22–FR26) | `checkout/` + attribution middleware / headers |
| Identity & consent (FR27–FR32) | `auth/`, `account/` + consent middleware + CMP gating in UI |
| Operations & governance (FR33–FR39) | `admin/` + `Services/Recommendation/Rules` + audit migrations |
| Experimentation (FR40–FR44) | Laravel assignment service + Redis/DB variant store |
| Support & diagnostics (FR45–FR50) | Admin/support controllers + read-only query services |
| Analytics (FR51–FR56) | Event writer + scheduled rollups (warehouse optional later) |
| API & integration (FR57–FR62) | `Api/V1/` + OpenAPI + gateway config |
| Compliance & resilience (FR63–FR68) | Jobs (purge), config, fallback rankers in `Services/Recommendation` |

### Integration points

**Internal:** Angular → HTTPS → API gateway (if deployed) → Laravel → MySQL/Redis; queues → workers.

**External:** OIDC IdP, CMP, SIEM, optional model provider—configured in Laravel `config/` + secrets; no secrets in Angular bundles.

**Data flow (recommendation):** UI task → `POST` session/rank → Laravel service (rules + catalog cache) → ranked DTOs → UI cards → consent-gated telemetry events.

### File organization patterns

- **Config:** Laravel `config/`; Angular `environment*.ts` for API base URL and build-time flags.
- **Tests:** Laravel Feature for HTTP, Unit for pure rankers; Angular `*.spec.ts` beside components for shallow tests.

### Development workflow integration

- **Local:** `php artisan serve` or Sail + `ng serve`; align ports in `.env` and `environment.ts`.
- **CI:** regenerate OpenAPI, `ng lint`, `php artisan test`, contract tests where present.
- **Deploy:** PHP-FPM + static `dist/` (+ optional SSR container if `@angular/ssr` is added).

*Step 6 saved on stakeholder **Continue (`c`)**.*

## Architecture Validation Results

### Coherence validation

**Decision compatibility**

- **Angular 20 + Laravel 12 + MySQL + Redis** are a common, compatible brownfield pairing; no conflicting stacks (for example no second SPA framework).
- **JWT + Laravel Sanctum/Socialite** (existing direction) coexists with PRD OIDC for admin—resolve per-environment IdP in configuration, not dual auth on one route.
- **New JSON `camelCase`** on new endpoints alongside **legacy snake_case** is explicitly bounded by versioning and the rule not to rename legacy fields until a version bump—acceptable if agents follow the split.

**Pattern consistency**

- Naming, folder, OpenAPI, journey header, and logging rules align with decisions on REST, telemetry, and support traceability.
- Error and loading patterns match the UX recovery triad (retry / refine / search).

**Structure alignment**

- Mapped FR groups to concrete `sprint5/API` and `sprint5/UI` paths; boundaries keep ranking logic server-side and HTTP access single-path from Angular.

### Requirements coverage validation

**Epics / stories**

- No epics document was in `inputDocuments`; coverage was assessed by **PRD FR categories** (68 FRs). Each row in the requirements-to-structure mapping has a primary home.

**Functional requirements**

- Shopper discovery through checkout, consent, operations, experiments, support, analytics, integration APIs, and compliance behaviors are assigned to **Laravel services**, the **Angular feature area**, **DB/Redis**, or **external integrations**—no orphan FR block was identified.

**Non-functional requirements**

- Latency (500 ms), availability (99.5%), WCAG AA, encryption, RBAC/audit, consent-gated telemetry, versioned APIs, fallback modes, and retention/erasure are covered in core decisions and patterns; SLO dashboards and incident runbooks remain **operational** follow-on work.

### Implementation readiness validation

**Decision completeness**

- Critical stack and boundary choices are documented; **vendor-specific** gateway, CMP, IdP, and optional LLM choices are called out in gap analysis for environment ADRs.

**Structure completeness**

- Canonical roots and **(new)** insertion points are explicit; not every legacy file is listed to avoid noise.

**Pattern completeness**

- High-risk agent conflict areas (casing, headers, logs, tests) have rules and examples.

### Gap analysis results

| Priority | Gap | Suggested follow-up |
|----------|-----|---------------------|
| Important | API **gateway** product and policies (rate limit, WAF, mTLS) not selected | ADR per environment (cloud-native vs Kong/NGINX, etc.). |
| Important | **CMP** integration and consent string schema | Privacy + frontend task; wire to Angular and Laravel middleware. |
| Important | **OIDC** IdP(s) for admin/support | Configuration plus Socialite or enterprise SSO package choice. |
| Nice | **NLU / LLM** provider for advanced task parsing (post-MVP) | Adapter behind `Recommendation` service interface. |
| Nice | **Angular SSR** rollout scope (which routes first) | Story after `ng add @angular/ssr` spike. |
| Nice | **Data warehouse** for long-range analytics | Event export when in-product aggregates are insufficient. |

### Validation issues addressed

- **Legacy vs new JSON casing:** resolved by the pattern “new endpoints use camelCase; legacy untouched until a versioned migration.”
- **Multiple sprint folders:** resolved by declaring **sprint5** canonical for new AI work.

### Architecture completeness checklist

**Requirements analysis**

- [x] Project context analyzed  
- [x] Scale and complexity assessed  
- [x] Technical constraints identified  
- [x] Cross-cutting concerns mapped  

**Architectural decisions**

- [x] Critical decisions documented (versions verified where researched: Laravel 12 support policy, MySQL 8.4 LTS line, Angular release train note)  
- [x] Integration patterns defined  
- [x] Performance and resilience considered  

**Implementation patterns**

- [x] Naming, structure, format, communication, and process patterns documented  

**Project structure**

- [x] Directory boundaries and FR mapping documented  
- [x] Integration points described  

### Architecture readiness assessment

**Overall status:** **READY FOR IMPLEMENTATION** (close vendor and env ADRs in early milestones).

**Confidence level:** **Medium–high** — strong brownfield fit; medium reflects external integration and SSR scope not yet fixed.

**Key strengths**

- Single canonical UI/API pair; clear server/client split; consent, audit, and journey correlation in the architecture early.

**Areas for future enhancement**

- Optional extraction of ranking to a dedicated service, analytics warehouse, streaming (SSE) recommendations, expanded fairness monitoring.

### Implementation handoff

**AI agent guidelines**

- Treat this document as the source of truth for boundaries, patterns, and structure.  
- Do not add parallel HTTP clients or alternate JSON conventions on the same API version.  
- Every new route: OpenAPI update, tests, and journey header behavior where applicable.

**First implementation priority**

1. Confirm **canonical `sprint5/UI` + `sprint5/API`**.  
2. Add stub **`/v1/recommendation-*`** routes + OpenAPI + Laravel Feature test.  
3. Add Angular **`RecommendationApiService`** and a **`recommendation/`** route shell with skeleton UX.

*Steps 7–8 completed on stakeholder **Continue (`c`)** — **Create Architecture** workflow finished.*
