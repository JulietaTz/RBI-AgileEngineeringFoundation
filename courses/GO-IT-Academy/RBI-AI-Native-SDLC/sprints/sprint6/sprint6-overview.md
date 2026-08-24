# Sprint 6 — Course Overview for Participants

## What is this course?

**Holistic Testing with GenAI** is a hands-on workshop that takes you through all 8 stages of the **Holistic Testing Model** — Discover, Plan, Understand, Build, Deploy, Release, Observe, and Learn — applied to a realistic sprint scenario.

You work as part of a product team delivering **Sprint 6** of the ToolShop webshop. The sprint has two business goals:

1. **Go to Czech market** — enable Czech language, Czech product content, local payment via PayU, and new delivery options.
2. **Security hardening** — replace vulnerable password hashing (MD5) and a deprecated authentication library.

GenAI supports you throughout: for risk analysis, test design, log analysis, and automation strategy decisions.

---

## What you will learn

- How to apply the **Holistic Testing Model** across an entire sprint lifecycle
- **Risk-based test planning** using MoSCoW prioritization with AI assistance
- The **T1T5 test design technique** for business use cases and production validation
- How to design test situations for a **password strength feature** with multi-language character sets
- How to **read and triage automated test execution logs** with a quality engineering lens
- How to **design tests for production** when non-production coverage is limited
- How to **analyze production monitoring logs** and write a Test Summary Report with a Go/No-Go recommendation
- How to **continuously improve a Test Automation Strategy** based on real findings, defects, and risks

---

## The 8 Transfer Tasks

Each task maps to one stage of the Holistic Testing Model. They build on each other — the outputs of earlier tasks feed into later ones.

---

### 1 — Discover: Test the Idea (Go to Czech Market)

**Stage:** Discover  
**What you do:**  
You evaluate whether ToolShop should expand into the Czech market. Using publicly available data, you research Czech competitors, customer needs, and localization challenges (language, payment, logistics, legal). You deliver a structured recommendation: expand, pilot, or hold.

**What you learn:**  
How to apply a structured market evaluation as a testing activity — before a single line of code is written.

---

### 2 — Plan: Risk-Based Test Planning with GenAI

**Stage:** Plan  
**What you do:**  
You analyze all Sprint 6 user stories for business impact, technical complexity, integration risks, and security relevance. Using different GenAI models, you apply **MoSCoW prioritization** (3 MUST / 1 SHOULD / 2 COULD / 4 WON'T) and compare how different models reason about testing risk.

**What you learn:**  
How AI can support and accelerate risk-based test planning, and how to critically evaluate and compare AI-generated prioritization decisions.

---

### 3 — Understand: T1T5 Test Design for Business Use Cases (ATDD)

**Stage:** Understand  
**What you do:**  
Using the official ToolShop feature list, you identify 3 business use cases influenced by Sprint 6 stories (e.g., checkout, registration, language switching). You apply the **T1T5 test design technique** to define a complete test set — at minimum one test per T1–T5 category — per use case.

**What you learn:**  
How to derive meaningful acceptance tests from business requirements using a structured design technique, and how this supports ATDD practices.

---

### 4 — Build: Test the Password Strength Feature

**Stage:** Build  
**What you do:**  
You analyze a defined password policy and design a set of **test situations** for the password strength feature. You create passwords covering German, Czech, and English character sets — both valid and invalid — and document them in a table with expected strength level and pass/fail status. You compare outputs from multiple LLMs.

**What you learn:**  
How to systematically derive test situations from a business rule, how character encoding affects test design, and how different AI models produce different test coverage.

---

### 5 — Deploy: Analyze Automated Test Execution Logs

**Stage:** Deploy  
**What you do:**  
You analyze 20 automated test execution log files (`ta-*.log`) from a ToolShop pipeline run. You identify quality patterns: repeated failures, order dependence, vague diagnostics, missing assertions, slow tests, and coverage gaps. You produce a prioritized improvement backlog — each item with an observation, a suspected root cause, and a concrete next step.

**What you learn:**  
How to read automation logs with a "test quality" lens, and how to translate log patterns into actionable improvements for a test suite.

---

### 6 — Release: T1T5 Test Design for Production Payment Validation

**Stage:** Release  
**What you do:**  
PayU was integrated with only a dummy implementation in non-production environments. You design a **T1T5 test set for testing in production** covering payment processing, checkout integration, error handling, security, and reliability. Each team is assigned a specific T1T5 type (T1–T5) and delivers a prioritized table of test cases.

**What you learn:**  
How to design safe, risk-oriented production validation tests for business-critical integrations — and why testing in production requires a different mindset than pre-production testing.

---

### 7 — Observe: Production Payment Log Analysis

**Stage:** Observe  
**What you do:**  
You analyze 5 production test log files (`test_*.log`) from the PayU deployment. You identify passed/failed tests, critical failure patterns, recurring errors, currency issues, and timeout anomalies. You write a full **Test Summary Report** including an Executive Summary, Key Findings, Risk Assessment, and a **Go / Go with Risks / No-Go** release recommendation.

**What you learn:**  
How to structure a professional test summary for stakeholders, how to assess production risk from monitoring data, and what a Go/No-Go decision requires as evidence.

---

### 8 — Learn: Improve the Test Automation Strategy

**Stage:** Learn  
**What you do:**  
Combining all findings from the previous 7 tasks — defects, log patterns, production risks, test designs — you reflect on what worked, what created risk, and what areas need stronger automation. You produce a **prioritized list of the 10 most important T1T5 tests to automate**, each justified by business value and risk mitigation.

**What you learn:**  
How the full sprint cycle feeds back into automation strategy decisions — and how continuous learning drives engineering quality forward.

---

## Sprint 6 User Stories (Input Material)

| Story | Title | Domain |
|-------|-------|--------|
| US2300 | Czech Language Support | Localization |
| US2350 | Czech Product Content | Localization |
| US2450 | Delivery Costs | Logistics |
| US3100 | PayU Payment Integration | Payment |
| US3150 | New Delivery Options | Logistics |
| US3206 | Secure Password Hashing | Security |
| US3409 | Replace Auth Library (Auth-X32) | Security |
| US4700 | Login Step in Checkout Flow | UX / Auth |
| US5003 | Replace ORM Mapper | Technical Debt |

---

## How the tasks connect

```
1-Discover  →  2-Plan  →  3-Understand  →  4-Build
                                               ↓
                                           5-Deploy
                                               ↓
                                  6-Release → 7-Observe
                                               ↓
                                           8-Learn
                                    (updates Automation Strategy)
```

Each stage produces an artifact that the next stage can reference. The final deliverable — the improved **Test Automation Strategy** — is grounded in real evidence from all 8 stages.
