review-story---
stepsCompleted:
  - 1
  - 2
  - 3
  - 4
workflowType: epics
status: complete
lastStep: 4
completedAt: 2026-04-19
inputDocuments:
  - _bmad-output/planning-artifacts/prd.md
  - _bmad-output/planning-artifacts/architecture.md
  - _bmad-output/planning-artifacts/ux-design-specification.md
---

# practice-software-testing - Epic Breakdown

## Overview

This document provides the complete epic and story breakdown for **practice-software-testing**, decomposing requirements from the PRD, Architecture, and UX Design Specification into implementable stories. **Epic list** and **FR coverage map** are drafted in Step 2 pending your approval, then stories in Step 3.

## Requirements Inventory

### Functional Requirements

- FR1: Shopper can start an AI-guided discovery session from key storefront entry points.
- FR2: Shopper can describe a task or job-to-be-done in natural language to receive guidance.
- FR3: Shopper can provide or refine contextual inputs (for example, use case, material, constraints, budget) during discovery.
- FR4: System can generate product recommendations based on task context and available user signals.
- FR5: System can present a ranked set of recommended products for user evaluation.
- FR6: Shopper can request alternative recommendations when initial suggestions are not satisfactory.
- FR7: Shopper can continue discovery using non-AI methods without losing session continuity.
- FR8: System can support recommendation behavior for users with no prior history (cold-start scenarios).
- FR9: Shopper can view a plain-language explanation for why each recommended product is suggested.
- FR10: Shopper can view confidence cues associated with recommended products.
- FR11: Shopper can understand whether recommendations are AI-derived, rule-based, or manually curated.
- FR12: Shopper can access clear disclosure when a decision aid is AI-driven.
- FR13: Shopper can provide explicit helpfulness feedback on recommendation quality.
- FR14: System can capture implicit feedback signals from user interaction behavior.
- FR15: Support and operations users can review recommendation rationale for a historical session.
- FR16: Shopper can compare recommended products on key decision attributes.
- FR17: Shopper can inspect compatibility and suitability information relevant to the stated task.
- FR18: Shopper can review availability status of recommended products before selection.
- FR19: Shopper can move from recommendation to product detail without losing discovery context.
- FR20: Shopper can add recommended products to cart directly from discovery surfaces.
- FR21: Shopper can adjust selection after recommendation review before checkout initiation.
- FR22: Shopper can complete checkout after AI-guided selection using standard purchase flow.
- FR23: System can attribute conversion outcomes to recommendation source context.
- FR24: System can support completion of purchases that originated from fallback (non-AI) discovery paths.
- FR25: Shopper can complete purchase without mandatory AI interaction.
- FR26: System can preserve journey continuity from discovery to checkout for analytics and support review.
- FR27: Returning shopper can receive recommendations informed by historical behavior and purchase activity.
- FR28: Shopper can use recommendation functionality as a guest or authenticated user according to policy.
- FR29: Shopper can provide and manage consent choices related to recommendation and analytics data usage.
- FR30: System can enforce consent state before collecting recommendation and analytics signals.
- FR31: Shopper can request deletion of personal recommendation-related data.
- FR32: System can execute data minimization rules for recommendation and telemetry processing.
- FR33: Operations users can configure and maintain baseline recommendation rules.
- FR34: Operations users can prioritize, demote, include, or exclude products from recommendation outcomes.
- FR35: Operations users can stage recommendation logic changes before broad release.
- FR36: Operations users can control release exposure for recommendation features.
- FR37: Operations users can revert recommendation configuration changes when negative impact is detected.
- FR38: Operations users can monitor core recommendation business and quality indicators.
- FR39: Operations users can review recommendation-impact outcomes across experiment variants.
- FR40: Product and operations users can run controlled experiments comparing AI-guided and traditional discovery journeys.
- FR41: System can assign and persist experiment variants for eligible sessions.
- FR42: System can measure recommendation engagement and conversion outcomes by variant.
- FR43: Product users can evaluate experiment results to guide prioritization decisions.
- FR44: System can support phased rollout progression based on defined decision gates.
- FR45: Support users can retrieve a user and session recommendation interaction history for issue handling.
- FR46: Support users can inspect inputs and outputs used in recommendation generation for a reported case.
- FR47: Support users can classify recommendation issues by root-cause category.
- FR48: Support users can escalate systemic recommendation issues to responsible teams with attached evidence.
- FR49: System can maintain an auditable history of recommendation-logic and privileged administrative actions.
- FR50: Operations users can access diagnostic signals needed to investigate recommendation service incidents.
- FR51: System can capture recommendation impression, selection, and downstream conversion events.
- FR52: Product users can analyze funnel progression from discovery entry through checkout completion.
- FR53: Product users can compare AI-guided and traditional journey performance.
- FR54: Product users can analyze outcomes by user segment (for example, new vs returning).
- FR55: Product users can analyze outcomes by recommendation source and confidence context.
- FR56: System can provide analytics outputs needed to evaluate time-to-decision and conversion uplift objectives.
- FR57: Internal or partner clients can request recommendations through a governed service interface.
- FR58: Recommendation consumers can receive ranked outputs with decision-support metadata.
- FR59: Integrated clients can submit telemetry events associated with recommendation interactions.
- FR60: Integrated clients can rely on backward-compatible service evolution according to version policy.
- FR61: Integration consumers can authenticate and access recommendation capabilities according to access policy.
- FR62: System can enforce usage governance controls for integration traffic.
- FR63: System can enforce retention and deletion policies for recommendation and telemetry data classes.
- FR64: System can maintain separation between personal data processing and analytics processing contexts.
- FR65: Authorized users can access audit evidence supporting regulatory and control obligations.
- FR66: System can provide behavior continuity through predefined fallback modes when recommendation components degrade.
- FR67: System can support post-incident analysis with sufficient operational and decision-trace evidence.
- FR68: Governance users can evaluate recommendation behavior for bias and trust risk indicators and initiate corrective action.

### NonFunctional Requirements

- NFR1 (Performance): The system shall deliver user-facing recommendation responses within 500 ms under normal operating conditions.
- NFR2 (Performance): The system shall preserve responsive product discovery interactions during expected peak traffic periods.
- NFR3 (Performance): The system shall reflect stock and availability changes near real-time at decision-critical points in the shopping flow.
- NFR4 (Performance): The system shall degrade gracefully under peak load without blocking core purchase actions.
- NFR5 (Security): The system shall encrypt sensitive data in transit and at rest.
- NFR6 (Security): The system shall enforce least-privilege and role-based access for recommendation configuration, diagnostics, and audit data.
- NFR7 (Security): The system shall log privileged and administrative actions in an auditable, tamper-evident manner.
- NFR8 (Security): The system shall enforce consent state before collecting recommendation telemetry and analytics events.
- NFR9 (Security): The system shall support GDPR-aligned data minimization, lawful processing, and deletion rights.
- NFR10 (Security): The system shall support DORA-aligned monitoring, alerting, and incident response readiness.
- NFR11 (Scalability): The system shall support planned launch and 12-month growth without material degradation of recommendation and checkout journeys.
- NFR12 (Scalability): The system shall handle campaign and event traffic spikes above baseline demand without interruption to critical buyer flows.
- NFR13 (Scalability): The system shall support phased capacity expansion aligned with MVP, growth, and expansion roadmap stages.
- NFR14 (Scalability): The system shall process increasing recommendation and analytics event volumes while preserving data quality and processing continuity.
- NFR15 (Accessibility): The system shall meet WCAG 2.1 AA for public storefront, recommendation, and checkout journeys.
- NFR16 (Accessibility): The system shall provide full keyboard operability and screen-reader compatibility for critical user flows.
- NFR17 (Accessibility): The system shall provide accessible labeling and semantics for AI-driven recommendation elements.
- NFR18 (Accessibility): The system shall maintain accessibility conformance across supported devices, breakpoints, and browser targets.
- NFR19 (Integration): The system shall maintain reliable integration behavior for critical dependencies including IdP, CMP, API gateway, SIEM, and recommendation service interfaces.
- NFR20 (Integration): The system shall provide versioned integration contracts that avoid breaking changes for dependent consumers.
- NFR21 (Integration): The system shall trigger controlled fallback behavior on critical integration failure rather than failing end-to-end user journeys.
- NFR22 (Integration): The system shall provide integration observability telemetry sufficient for fault isolation and operational triage.
- NFR23 (Reliability): The system shall maintain at least 99.5% monthly availability, with stricter protection of checkout-adjacent critical paths.
- NFR24 (Reliability): The system shall provide approved fallback discovery modes when recommendation components are degraded or unavailable.
- NFR25 (Reliability): The system shall enforce incident MTTD, response, recovery, and post-incident review obligations defined in domain requirements.
- NFR26 (Reliability): The system shall preserve transaction continuity and minimize customer-facing disruption during partial failures.
- NFR27 (Reliability): The system shall retain audit and operational evidence required for resilience validation and post-incident analysis.
- NFR28 (Data lifecycle): The system shall enforce retention windows for recommendation telemetry, anonymized analytics, and user-identifiable data as defined in domain requirements.
- NFR29 (Data lifecycle): The system shall complete right-to-erasure propagation across dependent systems within the agreed deletion SLA.
- NFR30 (Data lifecycle): The system shall maintain logical separation between personal data processing and analytics processing contexts.
- NFR31 (Data lifecycle): The system shall use pseudonymized or anonymized model-training inputs with controlled refresh cycles.

### Additional Requirements

_From `architecture.md` — technical and process constraints that affect epics/stories._

- Use canonical codebase roots **`sprint5/UI`** (Angular 20 + Bootstrap 5) and **`sprint5/API`** (Laravel 12, PHP 8.3, MySQL, Redis) for new AI/recommendation work unless explicitly superseded.
- Expose recommendation and telemetry through **versioned REST** under `/v1/...` (or agreed prefix), with **OpenAPI** updated for every new route.
- **New** public JSON responses use **`camelCase`** keys; legacy endpoints keep existing field names until a versioned migration.
- Propagate **`X-Journey-Id`** (or agreed header pair) from Angular on recommendation- and attribution-related calls; document in OpenAPI.
- Implement **consent-gated telemetry** on client (CMP) and server (reject or no-op without valid consent context).
- Implement recommendation core in **Laravel services** first, with **Redis** caching and **queues** for non-latency-critical work; synchronous shopper path stays within **500 ms** budget where architected.
- **RBAC** and **immutable audit** for recommendation rules, privileged admin actions, and support read APIs (align to FR49, FR65).
- **PII vs analytics** separation in storage and service access paths; retention purge jobs per PRD domain windows.
- **CI gates:** OpenAPI diff, lint, tests; extend with **a11y** checks for touched Angular recommendation surfaces when pipeline allows.
- **Observability:** structured logs with `journey_id`, latency, route; traces/metrics for recommendation p95 and error budgets (99.5% SLO).
- **Feature flags:** DB/Redis-backed or agreed vendor; staged rollout and revert (FR35–FR37, FR44).
- **Angular:** lazy `recommendation/` feature area, single **`RecommendationApiService`**, CDK for overlays/focus; optional **`ng add @angular/ssr`** when SEO scope is fixed.
- **Naming:** plural `snake_case` MySQL tables; kebab-case Angular files; `app-ai-*` selectors for AI-owned components.
- Close architecture **gaps** in early stories: API gateway choice, CMP integration schema, OIDC for admin/support, optional LLM adapter, SSR scope, warehouse export.

### UX Design Requirements

_Actionable UX-derived requirements (testable in UI)._

- UX-DR1: Implement **Task prompt bar** with label, text/textarea, contextual chips (material, environment, budget), submit, and “Use search instead”; states empty, typing, submitting, success handoff, error with retry, disabled when policy blocks; compact (header) and expanded (landing) variants; **no focus trap** in header variant; Enter submits, Escape returns focus; `aria-describedby` for hints.
- UX-DR2: Implement **Recommendation card** for 2–3 visible picks with thumbnail, title, price, stock, one-line “why,” source hint (rule vs model), primary Add to cart, secondary Compare and Why details; skeleton, ready, out-of-stock, error states; horizontal vs stacked responsive variants; `aria-describedby` / region semantics for rationale.
- UX-DR3: Implement **Explain panel / accordion** with collapsed default, expanded state, optional technical tier; `aria-expanded` and focus move into panel when opened.
- UX-DR4: Implement **Compact compare tray** for 2–3 SKUs, sticky desktop, offcanvas/sheet on small screens, Bootstrap table, max-SKU guard with message; scoped `th` and keyboard navigation; closing does not clear cart.
- UX-DR5: Implement **Feedback chip** (“Not helpful?”) with idle, submitted, optional “Tell us more”; toast confirmation; accessible button/toggle naming.
- UX-DR6: Implement **Consent / instrumentation shell** so GA4/internal events on AI surfaces only fire per CMP state (blocked / partial / full).
- UX-DR7: Implement **internal support session trace viewer** layout using Bootstrap tables/badges/accordions (read-only timeline: inputs, outputs, actions).
- UX-DR8: Map **design tokens** (brand + single **AI accent**) to CSS variables over Bootstrap theme variables; validate **WCAG 2.1 AA** contrast for primary, AI accent, and semantic colors.
- UX-DR9: Document owned AI components in **Storybook** (or team equivalent) with keyboard and screen-reader notes.
- UX-DR10: Apply **button hierarchy**: one primary per view (`btn-primary` for Add to cart / checkout continue); secondary outline for Compare / Why / Refine; `btn-link` for search escape; never `btn-danger` for dismiss AI.
- UX-DR11: Apply **feedback patterns**: skeleton cards for AI slots; toasts `role="status"`; errors `role="alert"` without incorrect focus steal; empty “no matches” with Refine, Search, Browse; API failure order retry → refine → search.
- UX-DR12: Apply **form patterns** to task input: validate on submit; inline errors with `aria-invalid` / `aria-describedby`; optional single-line expand to textarea on focus without collapsing while error visible.
- UX-DR13: Implement **navigation dual rail**: global **Help me choose** peer to search/categories/cart; split layout ≥ `md`, stacked mobile with task block first; skip link and landmarks; sticky compare subheader when active.
- UX-DR14: **Modals:** Bootstrap modal + CDK focus trap; restore focus on dismiss; CMP/cookie first layer must not trap focus; `aria-modal="true"`; Escape closes except legal block.
- UX-DR15: **Empty/loading discovery:** action-led copy; always three recovery paths (refine, search, bestsellers/category).
- UX-DR16: **Search/filter parity:** search does not hide guided entry; clearing filters does not clear cart; accessible search name and filter group labels.
- UX-DR17: **Responsive:** mobile-first; cart/search one tap on mobile; bottom safe-area for fixed bars; workshop-friendly contrast and **44×44px** touch targets on primary actions.
- UX-DR18: Use **Bootstrap 5 breakpoints** (`sm`–`xxl`) for layout switches; compare offcanvas below `md`.
- UX-DR19: **Keyboard:** full path task → recommendations → compare → add to cart without traps; visible `:focus-visible` rings.
- UX-DR20: **Screen readers:** recommendation set as list/region with count; stock/price blocking actions use polite live region updates where specified.
- UX-DR21: Honor **`prefers-reduced-motion`** (no layout-shift animation on recommendation load).
- UX-DR22: When **SSR** is enabled, hydrate without breaking CMP; avoid CLS (fixed-height skeletons, lazy images with dimensions).

### FR Coverage Map

| FR | Epic | Brief |
|----|------|--------|
| FR1 | Epic 1 | Start AI-guided session from storefront entry points |
| FR2 | Epic 1 | Describe task / JTBD in natural language |
| FR3 | Epic 1 | Refine contextual inputs (chips, constraints) |
| FR4 | Epic 1 | Generate recommendations from context + signals |
| FR5 | Epic 1 | Present ranked set for evaluation |
| FR6 | Epic 1 | Request alternative recommendations |
| FR7 | Epic 1 | Continue with non-AI discovery without losing session |
| FR8 | Epic 1 | Cold-start recommendation behavior |
| FR25 | Epic 1 | Complete purchase path without mandatory AI |
| FR27 | Epic 1 | Returning shopper signals inform recommendations |
| FR28 | Epic 1 | Guest vs authenticated recommendation use |
| FR9 | Epic 2 | Plain-language “why” per recommendation |
| FR10 | Epic 2 | Confidence cues on recommendations |
| FR11 | Epic 2 | Source disclosure (AI vs rule vs curated) |
| FR12 | Epic 2 | Clear AI decision-aid disclosure |
| FR13 | Epic 2 | Explicit helpfulness feedback |
| FR14 | Epic 2 | Implicit behavioral feedback capture |
| FR29 | Epic 2 | Consent choices for recommendation/analytics |
| FR30 | Epic 2 | Enforce consent before telemetry |
| FR31 | Epic 2 | Request deletion of recommendation-related personal data |
| FR32 | Epic 2 | Data minimization for telemetry/processing |
| FR16 | Epic 3 | Compare on key decision attributes |
| FR17 | Epic 3 | Compatibility / suitability for stated task |
| FR18 | Epic 3 | Availability before selection |
| FR19 | Epic 3 | PDP without losing discovery context |
| FR20 | Epic 3 | Add to cart from discovery surfaces |
| FR21 | Epic 3 | Adjust selection after review before checkout |
| FR22 | Epic 4 | Checkout after AI-guided selection |
| FR23 | Epic 4 | Attribute conversion to recommendation context |
| FR24 | Epic 4 | Purchases from fallback discovery paths |
| FR26 | Epic 4 | Journey continuity discovery → checkout |
| FR57 | Epic 5 | Governed recommendation service for clients |
| FR58 | Epic 5 | Ranked outputs + decision-support metadata |
| FR59 | Epic 5 | Submit interaction telemetry |
| FR60 | Epic 5 | Backward-compatible API evolution |
| FR61 | Epic 5 | Authenticate integration consumers |
| FR62 | Epic 5 | Usage governance on integration traffic |
| FR51 | Epic 6 | Capture impression, selection, conversion events |
| FR52 | Epic 6 | Funnel analytics discovery → checkout |
| FR53 | Epic 6 | AI-guided vs traditional journey comparison |
| FR54 | Epic 6 | Segment-level outcomes |
| FR55 | Epic 6 | Outcomes by source and confidence |
| FR56 | Epic 6 | Outputs for time-to-decision and uplift goals |
| FR33 | Epic 7 | Configure baseline recommendation rules |
| FR34 | Epic 7 | Prioritize / demote / include / exclude products |
| FR35 | Epic 7 | Stage logic changes before broad release |
| FR36 | Epic 7 | Control exposure of recommendation features |
| FR37 | Epic 7 | Revert configuration on negative impact |
| FR38 | Epic 7 | Monitor business and quality indicators |
| FR39 | Epic 7 | Review outcomes across experiment variants |
| FR49 | Epic 7 | Auditable admin and rule-change history |
| FR40 | Epic 8 | Run controlled experiments (AI vs traditional) |
| FR41 | Epic 8 | Assign and persist experiment variants |
| FR42 | Epic 8 | Measure engagement and conversion by variant |
| FR43 | Epic 8 | Evaluate experiments for prioritization |
| FR44 | Epic 8 | Phased rollout with decision gates |
| FR15 | Epic 9 | Ops/support review rationale for historical session |
| FR45 | Epic 9 | Support retrieves session interaction history |
| FR46 | Epic 9 | Support inspects recommendation inputs/outputs |
| FR47 | Epic 9 | Root-cause classification |
| FR48 | Epic 9 | Escalate systemic issues with evidence |
| FR50 | Epic 9 | Ops diagnostic signals for incidents |
| FR63 | Epic 10 | Retention and deletion policies |
| FR64 | Epic 10 | Separate personal vs analytics processing |
| FR65 | Epic 10 | Audit evidence for controls |
| FR66 | Epic 10 | Fallback when recommendation components degrade |
| FR67 | Epic 10 | Post-incident analysis evidence |
| FR68 | Epic 10 | Bias/trust risk evaluation and corrective action |

**NFRs (NFR1–NFR31):** Cross-cutting—each story that touches performance, security, accessibility, integration, reliability, or data lifecycle shall cite the relevant NFR IDs in acceptance criteria (no separate epic layer).

## Epic List

### Epic 1: Confident AI-guided product discovery

**Goal:** Shoppers start from intent, describe or refine a real job, get a short ranked list—including cold start and returning-user signals—and can always fall back to classic browse or checkout **without** being forced through AI.

**FRs covered:** FR1, FR2, FR3, FR4, FR5, FR6, FR7, FR8, FR25, FR27, FR28

**Notes:** Aligns with UX-DR1–UX-DR3, UX-DR11–UX-DR13, UX-DR15–UX-DR18, architecture canonical `sprint5` + journey header stub.

### Epic 2: Trust, consent, and data control

**Goal:** Shoppers see **why** picks exist, how confident the system is, and what kind of logic produced them; they can give feedback and control consent, deletion, and minimization for recommendation-related data.

**FRs covered:** FR9, FR10, FR11, FR12, FR13, FR14, FR29, FR30, FR31, FR32

**Notes:** UX-DR2–UX-DR6, UX-DR8–UX-DR12, CMP/consent architecture rules.

### Epic 3: Decide and buy from recommendations

**Goal:** From a short list, shoppers compare on task-relevant axes, see stock and fit, open PDP without losing context, add to cart, and adjust choices before checkout.

**FRs covered:** FR16, FR17, FR18, FR19, FR20, FR21

**Notes:** UX-DR4, UX-DR10, compare tray and button hierarchy.

### Epic 4: Checkout with journey attribution

**Goal:** Shoppers complete standard checkout after AI or fallback discovery; the system attributes conversions and preserves **journey continuity** for analytics and support.

**FRs covered:** FR22, FR23, FR24, FR26

**Notes:** Ties to architecture journey ID propagation and existing checkout flows.

### Epic 5: Integration-ready recommendation APIs

**Goal:** Internal or partner systems obtain ranked, explained recommendations and post telemetry through **versioned, authenticated, governed** APIs.

**FRs covered:** FR57, FR58, FR59, FR60, FR61, FR62

**Notes:** OpenAPI, `/v1/...`, camelCase new contracts per architecture.

### Epic 6: Product and funnel analytics

**Goal:** Product and operations users measure funnels, variants, segments, and sources to prove uplift and time-to-decision goals.

**FRs covered:** FR51, FR52, FR53, FR54, FR55, FR56

**Notes:** Consent-gated event pipeline (overlap FR30—stories reference both).

### Epic 7: Merchandising and safe operations

**Goal:** Operations users tune rules, staging, exposure, and rollback while monitoring quality—and leave an **auditable** trail of privileged changes.

**FRs covered:** FR33, FR34, FR35, FR36, FR37, FR38, FR39, FR49

**Notes:** Admin UI patterns (Bootstrap), Redis/DB flags per architecture.

### Epic 8: Experiments and phased rollout

**Goal:** Product and operations run **A/B** and phased rollouts with sticky variants and measurable outcomes to de-risk releases.

**FRs covered:** FR40, FR41, FR42, FR43, FR44

**Notes:** Links to Epic 7 monitoring; variant assignment early in session.

### Epic 9: Support and diagnostics

**Goal:** Support and operations can **replay** a session, inspect inputs/outputs, classify and escalate issues; ops has diagnostics for incidents.

**FRs covered:** FR15, FR45, FR46, FR47, FR48, FR50

**Notes:** UX-DR7 internal viewer; read-only APIs.

### Epic 10: Resilience, compliance, and governance

**Goal:** The platform enforces retention/separation, survives degradation with approved fallbacks, and supports audit, incident review, and **bias/trust** governance.

**FRs covered:** FR63, FR64, FR65, FR66, FR67, FR68

**Notes:** Jobs for purge/erasure; NFR alignment for GDPR/DORA posture.

---

## Epic 1: Confident AI-guided product discovery

Shoppers start from intent, describe or refine a real job, get a short ranked list—including cold start and returning-user signals—and can always fall back to classic browse or checkout **without** being forced through AI.

### Story 1.1: Storefront entry and dual-rail navigation for guided discovery

As a **shopper**,  
I want **clear entry points to “Help me choose” alongside search and categories**,  
So that **I can start AI-guided discovery without losing classic shop parity**.

**Acceptance Criteria:**

**Given** I am on home, category, PDP, or global header  
**When** I look at the primary navigation  
**Then** I see a **Help me choose** affordance peer to search and categories (and cart remains one tap on mobile)  
**And** choosing it navigates to the guided flow route without clearing my cart (FR1, FR7, FR25, UX-DR13, NFR15–NFR18).

**Given** I am on a small viewport  
**When** I open the nav pattern used by the app  
**Then** search and cart remain reachable within one tap per UX breakpoints (UX-DR17, UX-DR18).

### Story 1.2: Task prompt bar and contextual chips

As a **shopper**,  
I want **to describe my job and refine it with chips (material, environment, budget)**,  
So that **the system can narrow recommendations to my situation**.

**Acceptance Criteria:**

**Given** the guided flow route is active  
**When** I enter free text and optional chips and submit  
**Then** the client validates on submit, shows inline errors with `aria-invalid` / `aria-describedby`, and calls the recommendation API with the payload (FR2, FR3, UX-DR1, UX-DR12, NFR15–NFR17).  
**And** I can use **Use search instead** as a tertiary escape without losing session (FR7).

### Story 1.3: Journey identifier and recommendation session API

As a **shopper**,  
I want **a stable journey identifier across my session**,  
So that **later checkout and support can correlate my discovery path**.

**Acceptance Criteria:**

**Given** I start or resume guided discovery  
**When** the client requests or creates a session  
**Then** the API returns a **journeyId** and the Angular client sends `X-Journey-Id` on subsequent recommendation calls per architecture patterns (FR26 partial foundation, architecture).  
**And** OpenAPI documents the header and session resource (FR60 alignment).

### Story 1.4: Ranked recommendations with cold-start behavior

As a **shopper**,  
I want **a short ranked list of products for my task**,  
So that **I can decide quickly even with no prior history**.

**Acceptance Criteria:**

**Given** a valid task payload (new or returning user, guest or authenticated per policy)  
**When** I request recommendations  
**Then** I receive **≤3** ranked items within the **500 ms** budget under normal load with skeleton placeholders during fetch (FR4, FR5, FR8, FR27, FR28, NFR1, NFR4, UX-DR11).  
**And** cold-start uses documented safe defaults (rules + popularity) without blocking on external ML (FR8).

### Story 1.5: Alternative recommendations and refine loop

As a **shopper**,  
I want **to ask for alternatives or refine my task after the first set**,  
So that **I can recover from a weak first match without leaving the shop**.

**Acceptance Criteria:**

**Given** an initial ranked set is shown  
**When** I choose **Refine** or **More like this** / alternatives action  
**Then** the client re-submits context and replaces cards with a new ranked set (FR6).  
**And** errors offer **retry → refine → search** ordering per UX patterns (UX-DR11).

### Story 1.6: Fallback to classic discovery without session loss

As a **shopper**,  
I want **to switch to search, categories, or bestsellers at any time**,  
So that **I never feel trapped in AI** (FR7, FR25).

**Acceptance Criteria:**

**Given** I am in guided flow  
**When** I activate search or category browse  
**Then** my cart and journey context remain intact and I can return to guided flow (FR7, FR25, UX-DR16).  
**And** no mandatory AI step exists on the path to checkout (FR25).

---

## Epic 2: Trust, consent, and data control

Shoppers see **why** picks exist, how confident the system is, and what kind of logic produced them; they can give feedback and control consent, deletion, and minimization for recommendation-related data.

### Story 2.1: Rationale, confidence, and source metadata on recommendations

As a **shopper**,  
I want **plain-language “why,” confidence cues, and source type (AI / rule / curated)**,  
So that **I can trust or challenge suggestions**.

**Acceptance Criteria:**

**Given** ranked items are returned  
**When** cards render  
**Then** each item shows **rationale**, **confidence** cue, and **source** label and meets disclosure for AI-driven aid (FR9, FR10, FR11, FR12, UX-DR2, NFR17).  
**And** one-line rationale is visible before marketing fluff per UX guidelines (UX-DR2).

### Story 2.2: Explainability accordion and AI disclosure surfaces

As a **shopper**,  
I want **optional deeper explanation without cluttering the card**,  
So that **I can dig in only when needed**.

**Acceptance Criteria:**

**Given** a recommendation card  
**When** I expand “Why details”  
**Then** accordion follows `aria-expanded` and moves focus into the panel (FR9, UX-DR3, NFR16).  
**And** collapsed state is default (UX-DR3).

### Story 2.3: Explicit and implicit feedback capture

As a **shopper**,  
I want **to mark “Not helpful” and have my clicks inform quality**,  
So that **the product improves without heavy forms**.

**Acceptance Criteria:**

**Given** recommendations are visible  
**When** I use the feedback chip and optional “Tell us more”  
**Then** events are recorded with **consent state** enforced server-side (FR13, FR14, FR30, UX-DR5).  
**And** confirmation uses polite toast `role="status"` (UX-DR11).

### Story 2.4: Consent and instrumentation shell (client + server)

As a **shopper**,  
I want **analytics and personalization hooks to respect my CMP choices**,  
So that **tracking only happens when allowed**.

**Acceptance Criteria:**

**Given** CMP states blocked / partial / full  
**When** recommendation impressions or clicks would fire  
**Then** the shell blocks or gates GA4/internal events accordingly (FR29, FR30, UX-DR6, NFR8).  
**And** server rejects telemetry writes without valid consent context (FR30).

### Story 2.5: Data subject deletion and minimization for recommendation data

As a **shopper**,  
I want **to request deletion and rely on minimized retention**,  
So that **my data rights are honored**.

**Acceptance Criteria:**

**Given** an authenticated user requests deletion of recommendation-related personal data  
**When** the request is processed  
**Then** propagation runs per SLA across recommendation tables and derived views (FR31, FR32, NFR9, NFR28–NFR30).  
**And** audit log records the request outcome without storing unnecessary PII (FR49 alignment).

### Story 2.6: Design tokens and AI accent theme

As a **shopper**,  
I want **consistent visual treatment of AI vs primary commerce actions**,  
So that **the UI feels coherent and accessible**.

**Acceptance Criteria:**

**Given** Bootstrap theme variables  
**When** tokens are applied to guided surfaces  
**Then** primary, secondary, and **AI accent** meet **WCAG 2.1 AA** contrast checks documented in the story evidence (UX-DR8, NFR15).  
**And** AI accent is not applied to every CTA (UX-DR10 scope).

---

## Epic 3: Decide and buy from recommendations

From a short list, shoppers compare on task-relevant axes, see stock and fit, open PDP without losing context, add to cart, and adjust choices before checkout.

### Story 3.1: Compact compare tray (2–3 SKUs)

As a **shopper**,  
I want **to compare up to three recommendations on task-relevant attributes**,  
So that **I can choose without opening many tabs**.

**Acceptance Criteria:**

**Given** I select Compare on one or two items  
**When** the tray opens (sticky desktop, offcanvas mobile)  
**Then** I can lock at most **three** SKUs and keyboard-navigate the comparison table (FR16, UX-DR4, NFR16).  
**And** closing the tray does not empty my cart (UX-DR4).

### Story 3.2: Suitability and compatibility information

As a **shopper**,  
I want **compatibility and suitability details for my stated task**,  
So that **I avoid wrong purchases**.

**Acceptance Criteria:**

**Given** a task context and SKU  
**When** I view compare or PDP from guided flow  
**Then** suitability fields requested by UX are populated from catalog/service reads (FR17).  
**And** PDP link preserves discovery context query params where defined (FR19).

### Story 3.3: Stock and availability on recommendation surfaces

As a **shopper**,  
I want **accurate stock near decision time**,  
So that **I do not commit to unavailable items**.

**Acceptance Criteria:**

**Given** recommendations are shown  
**When** inventory changes for a visible SKU  
**Then** cards update before add-to-cart per near–real-time NFR (FR18, NFR3).  
**And** out-of-stock disables primary add with alternate CTA per UX card states (UX-DR2).

### Story 3.4: Add to cart and adjust selection from guided surfaces

As a **shopper**,  
I want **to add from cards and adjust quantity before checkout**,  
So that **I can finalize my basket efficiently**.

**Acceptance Criteria:**

**Given** in-stock recommendations  
**When** I click **Add to cart** (primary)  
**Then** cart service updates and confirmation matches existing shop behavior (FR20, FR21, UX-DR10).  
**And** only one primary CTA dominates the card region (UX-DR10).

---

## Epic 4: Checkout with journey attribution

Shoppers complete standard checkout after AI or fallback discovery; the system attributes conversions and preserves **journey continuity** for analytics and support.

### Story 4.1: Journey propagation through checkout requests

As a **shopper**,  
I want **my journey ID to persist through checkout API calls**,  
So that **support and analytics can reconstruct my path**.

**Acceptance Criteria:**

**Given** I have an active `journeyId` from guided discovery  
**When** I proceed through checkout steps  
**Then** the client attaches the agreed header or metadata on checkout-related API calls (FR26, architecture).  
**And** behavior is documented in OpenAPI where externally visible.

### Story 4.2: Conversion attribution to recommendation context

As a **product owner**,  
I want **orders to record recommendation source context**,  
So that **we can measure uplift**.

**Acceptance Criteria:**

**Given** a completed purchase that originated from guided or fallback paths  
**When** the order is persisted  
**Then** attribution fields store experiment/recommendation source per FR23 schema (FR23, FR24).  
**And** missing AI context does not block order completion (FR24, FR25).

### Story 4.3: Standard checkout completion after guided selection

As a **shopper**,  
I want **to use the existing checkout flow after AI picks**,  
So that **I trust payment and delivery steps**.

**Acceptance Criteria:**

**Given** items in cart from guided surfaces  
**When** I complete checkout  
**Then** the standard checkout UI and APIs succeed without mandatory AI steps (FR22, FR25, NFR23).  
**And** errors use existing recovery patterns without dead ends (NFR21).

---

## Epic 5: Integration-ready recommendation APIs

Internal or partner systems obtain ranked, explained recommendations and post telemetry through **versioned, authenticated, governed** APIs.

### Story 5.1: Versioned REST contract and OpenAPI for recommendations

As an **integration developer**,  
I want **documented `/v1` recommendation endpoints**,  
So that **I can integrate safely**.

**Acceptance Criteria:**

**Given** the Laravel API  
**When** consumers call recommendation endpoints  
**Then** responses use **camelCase** fields on new contracts and OpenAPI is regenerated in CI (FR57, FR58, FR60, architecture).  
**And** breaking changes require a new major version path (FR60).

### Story 5.2: Authenticated access for integration consumers

As a **platform engineer**,  
I want **strong authentication for integration traffic**,  
So that **only authorized callers use ranking**.

**Acceptance Criteria:**

**Given** a client credential or token per policy  
**When** they call recommendation APIs  
**Then** unauthenticated calls receive **401** and policy is documented (FR61, NFR5–NFR7).  
**And** least-privilege scopes separate read vs telemetry write (FR61, FR62).

### Story 5.3: Telemetry ingestion with idempotency and governance

As an **integration developer**,  
I want **to post interaction events with dedupe keys**,  
So that **retries do not distort metrics**.

**Acceptance Criteria:**

**Given** a telemetry POST with client event key  
**When** the same key is retried  
**Then** the server dedupes and returns success without double counting (FR59, architecture idempotency).  
**And** rate limits and payload size limits enforce governance (FR62, NFR22).

### Story 5.4: Usage quotas and abuse protection

As a **platform engineer**,  
I want **usage governance on integration traffic**,  
So that **latency SLOs hold**.

**Acceptance Criteria:**

**Given** sustained over-quota traffic  
**When** thresholds are exceeded  
**Then** the gateway or application returns **429** with retry guidance without impacting core shoppers (FR62, NFR1, NFR4).  
**And** alerts fire per observability plan (NFR10, NFR22).

---

## Epic 6: Product and funnel analytics

Product and operations users measure funnels, variants, segments, and sources to prove uplift and time-to-decision goals.

### Story 6.1: Recommendation event schema and writer

As a **product analyst**,  
I want **impression, click, and conversion events stored reliably**,  
So that **I can build funnels**.

**Acceptance Criteria:**

**Given** consent allows collection  
**When** events arrive from web or integrations  
**Then** they persist with journey and variant identifiers where applicable (FR51, FR30, FR41).  
**And** PII stays out of analytics-only stores (FR64, NFR30).

### Story 6.2: Funnel and journey comparison dashboards

As a **product user**,  
I want **to see funnels from discovery through checkout by AI vs traditional paths**,  
So that **I can validate uplift hypotheses**.

**Acceptance Criteria:**

**Given** populated events  
**When** I open the analytics views  
**Then** I can compare AI-guided vs traditional journeys and funnel drop-offs (FR52, FR53, FR56).  
**And** exports use pseudonymous identifiers (FR64).

### Story 6.3: Segment and source/confidence breakdowns

As a **product user**,  
I want **breakdowns by segment and recommendation source**,  
So that **I can tune quality**.

**Acceptance Criteria:**

**Given** event data  
**When** I filter by new vs returning and by source/confidence bucket  
**Then** charts match definitions in the analytics spec attached to this story (FR54, FR55).  
**And** query performance stays within agreed dashboard SLAs (NFR1, NFR11).

---

## Epic 7: Merchandising and safe operations

Operations users tune rules, staging, exposure, and rollback while monitoring quality—and leave an **auditable** trail of privileged changes.

### Story 7.1: Baseline recommendation rules configuration

As an **operations user**,  
I want **to maintain baseline ranking rules**,  
So that **recommendations match merchandising strategy**.

**Acceptance Criteria:**

**Given** admin auth with RBAC  
**When** I create or update rules  
**Then** changes are validated, versioned, and written with actor + timestamp audit (FR33, FR49, NFR6, NFR7).  
**And** OpenAPI/admin routes are protected from anonymous access (NFR6).

### Story 7.2: Product boost, demote, include, exclude lists

As an **operations user**,  
I want **to steer specific SKUs in or out of outcomes**,  
So that **stock and commercial priorities are respected**.

**Acceptance Criteria:**

**Given** catalog SKUs  
**When** I apply boost/demote/include/exclude  
**Then** ranking honors these constraints in the next recommendation requests (FR34).  
**And** audit entries describe before/after snapshots (FR49).

### Story 7.3: Staged logic changes and controlled exposure

As an **operations user**,  
I want **to stage drafts and roll out gradually**,  
So that **bad deploys do not tank conversion**.

**Acceptance Criteria:**

**Given** a draft ruleset  
**When** I publish to a percentage or internal cohort flag  
**Then** only targeted traffic sees the draft; others remain on stable logic (FR35, FR36, FR44).  
**And** feature flag state is readable from diagnostics (FR50).

### Story 7.4: Revert and negative-impact safeguards

As an **operations user**,  
I want **one-click revert when metrics degrade**,  
So that **I can recover quickly**.

**Acceptance Criteria:**

**Given** elevated error rate or conversion drop against guardrails  
**When** I trigger revert  
**Then** previous stable configuration becomes active for all traffic (FR37, NFR24).  
**And** incident notes attach to the audit bundle (FR49, NFR27).

### Story 7.5: KPI monitoring and experiment outcome review

As an **operations user**,  
I want **live KPIs and experiment impact views**,  
So that **I can decide promotions or rollbacks**.

**Acceptance Criteria:**

**Given** telemetry and order attribution data  
**When** I open operations dashboards  
**Then** I see core quality and business indicators and cross-experiment comparisons (FR38, FR39, FR42).  
**And** data respects RBAC roles (NFR6).

---

## Epic 8: Experiments and phased rollout

Product and operations run **A/B** and phased rollouts with sticky variants and measurable outcomes to de-risk releases.

### Story 8.1: Experiment definition and lifecycle

As a **product user**,  
I want **to define experiments comparing AI-guided vs traditional journeys**,  
So that **we learn safely**.

**Acceptance Criteria:**

**Given** admin UI  
**When** I create an experiment with hypothesis and metrics  
**Then** it stores variant definitions and success criteria (FR40).  
**And** experiments cannot start without monitoring hooks enabled (FR38, NFR10).

### Story 8.2: Sticky variant assignment per session

As a **shopper**,  
I want **a consistent experience within my session**,  
So that **experiments are statistically valid**.

**Acceptance Criteria:**

**Given** an eligible session  
**When** the first discovery page loads  
**Then** variant assignment persists for the session cookie/journey record (FR41).  
**And** reassignment rules are documented to avoid bias (FR41, FR68).

### Story 8.3: Outcome measurement by variant

As a **product user**,  
I want **engagement and conversion metrics by variant**,  
So that **I can judge winners**.

**Acceptance Criteria:**

**Given** running experiment  
**When** events and orders include variant id  
**Then** dashboards show metrics per variant with statistical notes (FR42, FR43).  
**And** consent gating still applies to event capture (FR30).

### Story 8.4: Phased rollout decision gates

As an **operations user**,  
I want **phase gates before 100% rollout**,  
So that **risk stays bounded**.

**Acceptance Criteria:**

**Given** a phased plan with gate criteria  
**When** a phase completes  
**Then** system enforces approval or auto-promotion rules per configuration (FR44).  
**And** rollback path exists to prior phase (FR37, FR44).

---

## Epic 9: Support and diagnostics

Support and operations can **replay** a session, inspect inputs/outputs, classify and escalate issues; ops has diagnostics for incidents.

### Story 9.1: Session interaction history API

As a **support agent**,  
I want **to retrieve a user/session timeline of recommendation interactions**,  
So that **I can answer tickets quickly**.

**Acceptance Criteria:**

**Given** authorized support role  
**When** I query by user or journey id  
**Then** I receive ordered events with inputs and shown outputs (FR45, FR46, NFR6).  
**And** responses exclude unrelated PII fields (FR64).

### Story 9.2: Support console session viewer UI

As a **support agent**,  
I want **a read-only viewer with tables and accordions**,  
So that **I can explain what the customer saw**.

**Acceptance Criteria:**

**Given** the timeline API  
**When** I load a case  
**Then** the Bootstrap-based viewer renders per UX-DR7 without custom visual language drift (FR15, UX-DR7, NFR15).  
**And** export-to-PDF or copy bundle is optional follow-on if already in admin patterns.

### Story 9.3: Issue classification and escalation with evidence

As a **support agent**,  
I want **to tag root cause and escalate with a bundle**,  
So that **product/data teams can act**.

**Acceptance Criteria:**

**Given** a case  
**When** I choose category and escalate  
**Then** ticket includes session id, variant, rules version, and model metadata if applicable (FR47, FR48).  
**And** escalation is audit logged (FR49).

### Story 9.4: Operations diagnostic signals for incidents

As an **operations user**,  
I want **latency, error rate, and dependency health for recommendation services**,  
So that **I can triage incidents**.

**Acceptance Criteria:**

**Given** observability stack  
**When** an incident occurs  
**Then** diagnostics page surfaces traces/metrics/logs with `journey_id` correlation where present (FR50, NFR22, NFR27).  
**And** links to runbook documents are attached in story notes (NFR10).

---

## Epic 10: Resilience, compliance, and governance

The platform enforces retention/separation, survives degradation with approved fallbacks, and supports audit, incident review, and **bias/trust** governance.

### Story 10.1: Retention and deletion jobs for telemetry classes

As a **compliance owner**,  
I want **automated retention enforcement**,  
So that **we meet policy windows**.

**Acceptance Criteria:**

**Given** retention policies per data class  
**When** the scheduled job runs  
**Then** rows past TTL are purged or anonymized per rules (FR63, NFR28).  
**And** job outcomes are logged immutably (NFR7, FR65).

### Story 10.2: Logical separation of PII and analytics processing

As a **data engineer**,  
I want **enforced separation between PII processing and analytics paths**,  
So that **we reduce leakage risk**.

**Acceptance Criteria:**

**Given** application services  
**When** analytics queries run  
**Then** they cannot access raw PII tables without elevated audited role (FR64, NFR30).  
**And** automated tests assert role boundaries (NFR6).

### Story 10.3: Auditor-facing audit evidence export

As an **auditor**,  
I want **evidence packs for controls**,  
So that **reviews succeed**.

**Acceptance Criteria:**

**Given** authorized auditor role  
**When** I request an evidence export  
**Then** I receive signed/timestamped bundles covering consent, changes, and incidents (FR65, NFR7).  
**And** exports omit unrelated customer content (FR64).

### Story 10.4: Recommendation graceful degradation modes

As a **shopper**,  
I want **the shop to keep working when ranking fails**,  
So that **I can still buy**.

**Acceptance Criteria:**

**Given** recommendation dependency failure  
**When** I request recommendations  
**Then** the system serves approved fallback (rules/static/bestseller) without blocking checkout (FR66, NFR21, NFR24).  
**And** UI messaging is neutral with retry path (UX-DR11).

### Story 10.5: Post-incident analysis evidence package

As an **incident responder**,  
I want **a collated evidence pack after incidents**,  
So that **post-incident review completes on time**.

**Acceptance Criteria:**

**Given** a declared incident  
**When** the pack is generated  
**Then** it includes traces, config versions, feature flags, and sample journeys per domain obligations (FR67, NFR25–NFR27).  
**And** access is restricted and audited (NFR6, FR65).

### Story 10.6: Bias and trust risk review workflow

As a **governance user**,  
I want **to evaluate recommendation behavior for bias/trust risks**,  
So that **we can open corrective actions**.

**Acceptance Criteria:**

**Given** periodic review cadence  
**When** I complete a checklist with metrics and samples  
**Then** I can record decisions and link follow-up tickets (FR68).  
**And** outcomes feed back into Epic 7 rule changes (FR33–FR34).

---

### UX-DR coverage (cross-check)

| UX-DR range | Addressed in stories |
|-------------|---------------------|
| UX-DR1–UX-DR7 | 1.2–1.3, 2.2–2.4, 3.1, 9.2 |
| UX-DR8–UX-DR12 | 2.6, 2.1–2.2, 2.4, 1.2 |
| UX-DR13–UX-DR22 | 1.1, 1.5–1.6, 2.4, 3.x, 4.x (SSR noted in deployment follow-on) |

_SSR-specific UX-DR22 is satisfied when SSR story is executed; until then document as known gap in sprint planning._

---

## Final validation report

**Date:** 2026-04-19

### FR coverage (Step 4 §1)

- **FR1–FR68:** Each appears in the **FR Coverage Map** and in at least one story acceptance block or epic FR list; no orphan FRs detected on review.
- **NFR1–NFR31:** Woven into story AC where relevant; cross-cutting security, accessibility, and data lifecycle reinforced in Epics 2, 5, 6, 10.

### Architecture alignment (Step 4 §2)

- **Starter / brownfield:** `architecture.md` specifies **brownfield continuation** on `sprint5/UI` + `sprint5/API` — **Epic 1 Story 1.1** correctly starts with storefront/dual-rail entry, not a greenfield scaffold story. **Pass.**
- **Incremental data/model work:** Stories introduce persistence (sessions, events, rules) in the story that first needs them — no “create all tables” story. **Pass.**

### Story quality (Step 4 §3)

- Stories use **As a / I want / So that** and **Given / When / Then / And** with **testable** outcomes.
- **Sizing:** Scoped for a single dev agent per story; split further in sprint planning if estimates exceed one iteration.
- **Known gap:** **UX-DR22** (SSR + CMP hydration) remains explicit follow-on until an SSR enablement story is added (documented above).

### Epic structure (Step 4 §4)

- Epics are **user-value** oriented (shopper, ops, support, integration, governance), not layer cakes. **Pass.**

### Dependencies (Step 4 §5)

- **Within-epic:** Stories are ordered so later stories build on earlier ones in the same epic (e.g. 1.2 → 1.3 → 1.4).
- **Cross-epic:** **Epic 2** trust UI is most meaningful when **Epic 1** ranking exists; use **API mocks / stub cards** to implement Epic 2 in parallel if needed. **Recommended delivery order:** Epic **1 → 2 → 3 → 4** (shopper vertical slice), then **6** (events) before heavy **7–8** analytics overlap, **5** integrations, **9** support, **10** compliance jobs — adjust per team capacity.

### Outcome

**Validation:** **PASS** — document ready for **sprint planning** and **`bmad-dev-story`** / implementation.

*Create Epics and Stories workflow **complete** (Steps 1–4).*
