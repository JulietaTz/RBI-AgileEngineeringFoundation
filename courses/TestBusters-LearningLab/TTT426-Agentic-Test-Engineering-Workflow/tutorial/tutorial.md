# TTT426 Agentic Test Engineering Workflow

## Step-by-Step Tutorial für einen BMAD-inspirierten Test-Engineering-Workflow

**Version:** 1.0  
**Zielgruppe:** Test Engineers, Test Manager, Quality Engineers und AI-Workflow-Designer  
**Einsatzgebiet:** TTT426, ToolShop und vergleichbare Software-Testprojekte  
**Zielplattformen:** Cursor, GitHub Copilot, VS Code und andere dateibasierte Agent-Umgebungen

---

## 1. Ziel dieses Tutorials

Dieses Tutorial zeigt, wie du einen agentischen Test-Engineering-Workflow für
TTT426 entwickelst.

Der Workflow soll nicht nur einzelne Prompts ausführen. Er soll:

- spezialisierte Testrollen bereitstellen,
- den Testprozess schrittweise steuern,
- Informationen zwischen Agenten übergeben,
- Testartefakte versioniert erzeugen,
- definierte Qualitätsprüfungen durchführen,
- menschliche Freigaben berücksichtigen,
- Annahmen und offene Fragen transparent dokumentieren,
- und später um zusätzliche Agenten und Tools erweitert werden können.

Das Konzept ist von BMAD inspiriert. BMAD arbeitet mit spezialisierten Agenten, geführten Workflows und schrittweise aufgebautem Projektkontext. Die Ausgaben einer Phase bilden dabei den Kontext für die nächste Phase.

Der TTT426 Workflow überträgt dieses Prinzip auf den Softwaretestprozess.

---



## 2. Das gewünschte Zielbild

Der vollständige Workflow soll später ungefähr so aussehen:

```text
Testauftrag
    |
    v
Test Context Analysis
    |
    v
Test Planning
    |
    v
Test Analysis
    |
    v
Test Design
    |
    v
Test Implementation
    |
    v
Test Execution
    |
    v
Defect Analysis
    |
    v
Test Completion
    |
    v
Quality Gate und menschliche Freigabe
```

Jeder Schritt wird von einem spezialisierten Agenten oder einem definierten Workflow ausgeführt.

Der Workflow wird nicht sofort vollständig implementiert. Du entwickelst zuerst einen kleinen, überprüfbaren MVP und erweiterst ihn anschließend.

---



# Teil I: Grundlagen und Architektur



## 3. Die fünf Bausteine des Workflows

Der TTT426 Agentic Test Engineering Workflow besteht aus fünf zentralen Bausteinen.

### 3.1 Agents

Agents repräsentieren spezialisierte Rollen.

Beispiele:

- Test Manager Agent
- Test Analyst Agent
- Test Designer Agent
- Test Reviewer Agent
- Test Automation Architect Agent
- Test Data Agent
- Defect Analyst Agent

Ein Agent definiert vor allem:

- Rolle,
- Ziel,
- Verantwortung,
- Grenzen,
- benötigten Kontext,
- erlaubte Skills,
- erwartete Ergebnisse.



### 3.2 Skills

Skills sind wiederverwendbare fachliche Fähigkeiten.

Beispiele:

- Risk-based Testing
- Equivalence Partitioning
- Boundary Value Analysis
- Decision Table Testing
- State Transition Testing
- Exploratory Testing
- Test Data Design
- Test Automation Design
- Test Summary Reporting

Ein Skill sollte möglichst unabhängig von einem bestimmten Agenten sein. Mehrere Agenten können denselben Skill verwenden.

### 3.3 Workflows

Ein Workflow definiert eine geordnete Folge von Arbeitsschritten.

Beispiel:

```text
Requirement lesen
    -> Testbasis bewerten
    -> Risiken identifizieren
    -> Testbedingungen ableiten
    -> Ergebnis prüfen
    -> Artefakt speichern
```



### 3.4 Artifacts

Artifacts sind die dokumentierten Ein- und Ausgaben des Workflows.

Beispiele:

- Test Context
- Product Risk Analysis
- Test Plan
- Test Conditions
- Test Cases
- Test Data Requirements
- Test Execution Log
- Defect Report
- Test Summary Report



### 3.5 Quality Gates

Quality Gates prüfen, ob ein Ergebnis vollständig und ausreichend ist, bevor der nächste Schritt beginnt.

Beispiele:

- Testbasis ist vorhanden.
- Annahmen sind dokumentiert.
- Jedes Produktrisiko besitzt mindestens eine Testbedingung.
- Jeder Testfall besitzt ein erwartetes Ergebnis.
- Kritische Anforderungen sind abgedeckt.
- Menschliche Freigabe wurde erteilt.

---



## 4. Grundprinzipien für TTT426

Lege vor der Implementierung verbindliche Prinzipien fest.

### Prinzip 1: Artifact First

Agenten sollen Ergebnisse nicht nur im Chat ausgeben. Relevante Ergebnisse werden als Dateien gespeichert.

### Prinzip 2: Context is Versioned

Projektwissen, Entscheidungen und Regeln liegen im Repository und werden über Git versioniert.

### Prinzip 3: No Silent Assumptions

Ein Agent darf notwendige Annahmen treffen, muss diese aber kennzeichnen und im Artefakt dokumentieren.

### Prinzip 4: Human Accountability

Ein Agent darf Vorschläge erzeugen und Prüfungen durchführen. Die fachliche Verantwortung bleibt bei einer benannten Person.

### Prinzip 5: Separation of Duties

Ein erzeugendes Ergebnis wird möglichst von einem anderen Agenten geprüft.

### Prinzip 6: Traceability by Design

Anforderungen, Risiken, Testbedingungen, Testfälle, Ausführungen und Defects sollen nachvollziehbar miteinander verbunden werden.

### Prinzip 7: Small Context Packages

Ein Agent erhält nur den Kontext, den er für seine Aufgabe benötigt. Das reduziert Ablenkung und widersprüchliche Anweisungen.

### Prinzip 8: Stop on Critical Missing Information

Wenn eine kritische Information fehlt, markiert der Agent den Workflow als blockiert. Er erfindet keine Anforderungen.

---



# Teil II: Repository vorbereiten



## 5. Schritt 1: Neues Repository erstellen

Erstelle ein Git-Repository, zum Beispiel:

```text
ttt426-agentic-test-engineering
```

Initialisiere das Repository:

```bash
mkdir ttt426-agentic-test-engineering
cd ttt426-agentic-test-engineering
git init
```

Erstelle eine erste README-Datei:

```bash
touch README.md
```

Inhalt:

```markdown
# TTT426 Agentic Test Engineering

This repository contains the agents, skills, workflows, templates,
quality gates and project artifacts for the TTT426 Agentic Test Engineering Workflow.
```

Committe den Startpunkt:

```bash
git add .
git commit -m "Initialize TTT426 agentic test engineering repository"
```



### Ergebnis dieses Schrittes

Du besitzt ein leeres, versioniertes Repository.

---



## 6. Schritt 2: Verzeichnisstruktur anlegen

Erstelle folgende Struktur:

```text
ttt426-agentic-test-engineering/
|
|-- AGENTS.md
|-- README.md
|-- CHANGELOG.md
|
|-- agents/ (OK)
|   |-- test-orchestrator.agent.md
|   |-- test-manager.agent.md
|   |-- test-analyst.agent.md
|   |-- test-designer.agent.md
|   `-- test-reviewer.agent.md
|
|-- skills/ (OK)
|   |-- analyze-test-basis/
|   |   `-- SKILL.md
|   |-- product-risk-analysis/
|   |   `-- SKILL.md
|   |-- derive-test-conditions/
|   |   `-- SKILL.md
|   |-- design-test-cases/
|   |   `-- SKILL.md
|   `-- review-test-artifact/
|       `-- SKILL.md
|
|-- workflows/ (OK)
|   |-- 01-test-context-analysis.workflow.md
|   |-- 02-test-planning.workflow.md
|   |-- 03-test-analysis.workflow.md
|   |-- 04-test-design.workflow.md
|   `-- 05-test-review.workflow.md
|
|-- instructions/ (OK)
|   |-- core-guardrails.md
|   |-- terminology.md
|   |-- artifact-management.md
|   `-- human-approval.md
|
|-- templates/ (OK)
|   |-- test-context.template.md
|   |-- product-risk-analysis.template.md
|   |-- test-plan.template.md
|   |-- test-conditions.template.md
|   |-- test-cases.template.md
|   |-- artifact-review.template.md
|   `-- handover.template.md
|
|-- quality-gates/ (OK)
|   |-- qg-01-context-ready.md
|   |-- qg-02-analysis-ready.md
|   |-- qg-03-design-ready.md
|   `-- qg-04-human-approval.md
|
|-- knowledge/ (OK)
|   |-- ttt426-context.md
|   |-- test-policy.md
|   |-- glossary.md
|   `-- references.md
|
|-- projects/ (OK)
|   `-- toolshop/
|       |-- input/
|       |-- working/
|       |-- output/
|       |-- decisions/
|       `-- status/
|
|-- schemas/ (OK)
|   |-- artifact-metadata.schema.yaml
|   `-- workflow-state.schema.yaml
|
|-- scripts/ (OK)
|   |-- validate_artifact.py
|   `-- create_project.py
|
`-- tests/ (OK)
    |-- agent-tests/
    |-- skill-tests/
    `-- workflow-tests/y
```

Du kannst die Ordner mit folgendem Befehl erzeugen:

```bash
mkdir -p agents instructions templates quality-gates knowledge schemas scripts
mkdir -p workflows tests/agent-tests tests/skill-tests tests/workflow-tests
mkdir -p projects/toolshop/{input,working,output,decisions,status}
mkdir -p skills/{analyze-test-basis,product-risk-analysis,derive-test-conditions,design-test-cases,review-test-artifact}
```



### Warum diese Trennung wichtig ist

- `agents` definiert Rollen.
- `skills` enthält wiederverwendbares Fachwissen.
- `workflows` steuert die Reihenfolge.
- `instructions` enthält globale Regeln.
- `templates` standardisiert Ergebnisse.
- `quality-gates` verhindert unkontrollierte Übergaben.
- `knowledge` liefert gemeinsamen Projektkontext.
- `projects` enthält konkrete Projektdaten.
- `schemas` ermöglicht maschinelle Validierung.
- `tests` prüft die Qualität des Agentensystems.



### Ergebnis dieses Schrittes

Du besitzt das Grundgerüst des Frameworks.

---



# Teil III: Gemeinsame Regeln definieren



## 7. Schritt 3: Zentrale Projektanweisung erstellen

Erstelle die Datei `AGENTS.md`.

```markdown
# TTT426 Agent Instructions

## Mission

Support software testing activities for TTT426 through structured,
traceable and reviewable agentic workflows.

## Mandatory Principles

1. Use repository artifacts as the primary source of truth.
2. Do not invent requirements, policies, test results or approvals.
3. Clearly label assumptions, uncertainties and missing information.
4. Preserve traceability between requirements, risks, test conditions,
   test cases, executions and defects.
5. Follow the TTT426 terminology and templates.
6. Separate artifact creation from artifact review.
7. Do not mark a quality gate as passed without evidence.
8. Human approval is required where the workflow defines it.
9. Never overwrite an approved artifact without creating a new version.
10. Store relevant results in the project artifact folders.

## Artifact Status Values

- draft
- in-review
- changes-requested
- approved
- rejected
- obsolete

## Priority of Instructions

1. Legal, regulatory and security requirements
2. TTT426 test policy
3. Project-specific instructions
4. Workflow instructions
5. Agent instructions
6. Skill instructions
7. User task

## Required Response Behaviour

For every substantial task:

1. Identify the requested workflow step.
2. Identify required inputs.
3. Check whether the inputs exist.
4. Report blocking gaps.
5. Execute the relevant skill.
6. Create or update the defined artifact.
7. Run the required quality gate.
8. Record the handover and next step.
```



### Ergebnis dieses Schrittes

Alle Agenten erhalten eine gemeinsame Basis.

---



## 8. Schritt 4: Guardrails definieren

Erstelle `instructions/core-guardrails.md`.

```markdown
# Core Guardrails

## Requirement Integrity

- Never present an assumption as an approved requirement.
- Never silently resolve contradictory requirements.
- Mark unclear requirements as ambiguous.
- Reference the source of every requirement where possible.

## Test Integrity

- Never claim that a test was executed without execution evidence.
- Never invent passed or failed test results.
- Never close a defect without documented verification.
- Never claim full coverage without a defined coverage model.

## Data Protection

- Do not copy production credentials into test artifacts.
- Do not generate real personal data.
- Identify sensitive data in free-text examples.
- Use synthetic, anonymized or approved pseudonymized test data.

## Agent Boundaries

- Agents may propose decisions.
- Agents may not impersonate a human approver.
- Agents may not change approved scope without escalation.
- Agents may not bypass a failed quality gate.

## Transparency

Every artifact must document:

- source inputs,
- assumptions,
- unresolved questions,
- responsible agent,
- review status,
- creation date,
- version.
```

Erstelle zusätzlich `instructions/human-approval.md`.

```markdown
# Human Approval Rules

Human approval is mandatory for:

- test strategy approval,
- test plan approval,
- acceptance of residual product risk,
- production release recommendation,
- closure of critical defects,
- deviations from the test policy,
- use of production-derived test data.

An AI agent may prepare the approval package but must not approve it.
```



### Ergebnis dieses Schrittes

Die Agenten kennen ihre Grenzen und dürfen keine Freigaben erfinden.

---



# Teil IV: Artifact Model entwickeln



## 9. Schritt 5: Metadatenstandard definieren

Jedes erzeugte Artefakt benötigt einen standardisierten Kopf.

Erstelle `schemas/artifact-metadata.schema.yaml`.

```yaml
artifact_metadata:
  required:
    - artifact_id
    - title
    - artifact_type
    - project
    - version
    - status
    - created_by
    - created_at
    - source_inputs
    - assumptions
    - open_questions
  status_values:
    - draft
    - in-review
    - changes-requested
    - approved
    - rejected
    - obsolete
```

Nutze in Markdown-Artefakten folgenden Header:

```yaml
---
artifact_id: TTT426-TC-001
title: ToolShop Test Context
artifact_type: test-context
project: toolshop
version: 0.1
status: draft
created_by: test-orchestrator
created_at: 2026-07-14
source_inputs:
  - projects/toolshop/input/product-description.md
assumptions: []
open_questions: []
---
```



### Namenskonvention

Verwende beispielsweise:

```text
TTT426-TC-001   Test Context
TTT426-PRA-001  Product Risk Analysis
TTT426-TP-001   Test Plan
TTT426-TCND-001 Test Conditions
TTT426-TCASE-001 Test Case Set
TTT426-REV-001  Artifact Review
TTT426-HO-001   Handover
```



### Ergebnis dieses Schrittes

Artefakte werden eindeutig identifizierbar und versionierbar.

---



## 10. Schritt 6: Erstes Template erstellen

Erstelle `templates/test-context.template.md`.

```markdown
---
artifact_id: <ID>
title: <TITLE>
artifact_type: test-context
project: <PROJECT>
version: 0.1
status: draft
created_by: test-orchestrator
created_at: <DATE>
source_inputs: []
assumptions: []
open_questions: []
---

# Test Context

## 1. Product Under Test

## 2. Business Goal

## 3. Test Assignment

## 4. Scope

### In Scope

### Out of Scope

## 5. Test Basis

| ID | Source | Version | Status | Relevance |
|---|---|---|---|---|

## 6. Stakeholders

| Role | Responsibility | Approval Authority |
|---|---|---|

## 7. Constraints

## 8. Dependencies

## 9. Known Quality Risks

## 10. Missing Information

## 11. Assumptions

## 12. Recommended Next Step
```

Erstelle auf dieselbe Weise Templates für:

- Product Risk Analysis
- Test Plan
- Test Conditions
- Test Cases
- Artifact Review
- Handover



### Ergebnis dieses Schrittes

Der Output der Agenten erhält ein stabiles Format.

---

---



# Teil V: Den MVP mit fünf Agenten bauen



## 11. Schritt 7: Test Orchestrator Agent erstellen

Der Orchestrator ist kein Fachexperte für alle Testaktivitäten. Er erkennt den
Workflowstatus, wählt den nächsten Agenten und prüft die Übergabe.

Erstelle `agents/test-orchestrator.agent.md`.

```markdown
---
name: test-orchestrator
description: Coordinates the TTT426 test engineering workflow and routes work to specialized agents.
tools:
  - read
  - search
  - edit
  - terminal
---

# Test Orchestrator Agent

## Role

You coordinate the TTT426 Agentic Test Engineering Workflow.

## Goal

Determine the current workflow state, identify the next valid step and prepare
an explicit handover to the responsible specialist agent.

## Responsibilities

- Inspect project status and existing artifacts.
- Identify missing mandatory inputs.
- Select the correct workflow.
- Select the responsible specialist agent.
- Prevent steps from being executed out of sequence.
- Trigger or request the required quality gate.
- Update the workflow status artifact.
- Never perform human approval.

## Inputs

- AGENTS.md
- instructions/
- project status
- available project artifacts
- active user request

## Outputs

- workflow decision
- handover artifact
- updated workflow state
- list of blockers

## Routing Rules

- Missing test context -> test-orchestrator creates context draft.
- Test context ready, planning requested -> test-manager.
- Test basis ready, analysis requested -> test-analyst.
- Approved test conditions available -> test-designer.
- Draft artifact requires independent review -> test-reviewer.
- Failed quality gate -> return to producing agent.

## Restrictions

- Do not invent missing project information.
- Do not approve artifacts.
- Do not execute tests.
- Do not silently skip workflow steps.
```



### Testprompt

```text
Act as the TTT426 Test Orchestrator.
Inspect the ToolShop project folders and determine the next valid workflow step.
Do not create test cases.
```



### Erwartetes Verhalten

Der Agent sollte zunächst den Projektstatus prüfen und nicht sofort Testfälle
erzeugen.

---



## 12. Schritt 8: Test Manager Agent erstellen

Erstelle `agents/test-manager.agent.md`.

```markdown
---
name: test-manager
description: Creates and maintains test planning artifacts based on approved project context and product risks.
---

# Test Manager Agent

## Role

You are the Test Manager for TTT426.

## Goal

Create a realistic, risk-based and traceable test plan.

## Responsibilities

- Define test objectives.
- Define scope and exclusions.
- Select test levels and test types.
- Define the test approach.
- Define entry and exit criteria.
- Define roles and responsibilities.
- Identify test environment and test data needs.
- Define reporting and completion activities.

## Required Inputs

- approved or review-ready test context
- product risk analysis
- applicable test policy
- project constraints

## Output

- projects/<project>/working/test-plan.md

## Mandatory Rules

- Do not create a generic test plan disconnected from product risks.
- Reference relevant product risks.
- Mark unavailable information as TBD.
- Separate test objectives from test activities.
- Do not claim resources or environments are available without evidence.

## Completion Criteria

The test plan is ready for review only when:

- scope is explicit,
- objectives are measurable,
- risks are referenced,
- entry and exit criteria exist,
- responsibilities are assigned or marked TBD,
- assumptions and open questions are documented.
```

---



## 13. Schritt 9: Test Analyst Agent erstellen

Erstelle `agents/test-analyst.agent.md`.

```markdown
---
name: test-analyst
description: Analyzes the test basis, identifies coverage items and derives traceable test conditions.
---

# Test Analyst Agent

## Role

You are a senior Test Analyst working according to the TTT426 test process.

## Goal

Derive relevant, traceable and risk-focused test conditions from the available
test basis.

## Responsibilities

- Analyze requirements and acceptance criteria.
- Detect ambiguity, contradiction and missing information.
- Identify testable features and quality characteristics.
- Derive positive, negative and exception-oriented test conditions.
- Link test conditions to requirements and product risks.
- Recommend suitable test techniques.

## Required Inputs

- test context
- approved scope
- requirements or user stories
- product risk analysis
- terminology and test policy

## Output

- projects/<project>/working/test-conditions.md

## Restrictions

- Do not create detailed test cases unless explicitly requested by the workflow.
- Do not convert assumptions into requirements.
- Do not hide gaps in the test basis.

## Required Test Condition Fields

- Test Condition ID
- Title
- Source Requirement
- Product Risk
- Test Level
- Test Type
- Priority
- Description
- Suggested Test Technique
- Notes and Open Questions
```

---



## 14. Schritt 10: Test Designer Agent erstellen

Erstelle `agents/test-designer.agent.md`.

```markdown
---
name: test-designer
description: Designs concrete test cases from reviewed test conditions using appropriate test techniques.
---

# Test Designer Agent

## Role

You are a senior Test Designer for TTT426.

## Goal

Create concrete, executable and traceable test cases from reviewed test
conditions.

## Responsibilities

- Select suitable test design techniques.
- Define preconditions.
- Define test data requirements.
- Define executable steps.
- Define observable expected results.
- Link every test case to a test condition.
- Identify automation candidates without implementing automation.

## Required Inputs

- reviewed test conditions
- relevant requirements
- test data constraints
- test environment information

## Output

- projects/<project>/working/test-cases.md

## Mandatory Rules

- Every test case must have an expected result.
- Avoid vague wording such as "works correctly".
- Separate preconditions from test steps.
- Use synthetic test data examples.
- Mark unknown values as parameters, not invented facts.
- Do not create automated code unless a separate workflow requests it.

## Required Test Case Fields

- Test Case ID
- Linked Test Condition
- Objective
- Priority
- Preconditions
- Test Data
- Steps
- Expected Result
- Postconditions
- Automation Candidate
```

---



## 15. Schritt 11: Test Reviewer Agent erstellen

Erstelle `agents/test-reviewer.agent.md`.

```markdown
---
name: test-reviewer
description: Independently reviews TTT426 test artifacts against defined criteria and records evidence-based findings.
---

# Test Reviewer Agent

## Role

You are an independent Test Artifact Reviewer.

## Goal

Evaluate whether a test artifact is complete, consistent, traceable and ready
for the next workflow step.

## Responsibilities

- Review the artifact against its template.
- Check relevant quality gates.
- Check internal consistency.
- Check traceability.
- Detect unsupported claims.
- Identify missing information.
- Classify findings by severity.
- Recommend pass, conditional pass or fail.

## Restrictions

- Do not rewrite the complete artifact during the review.
- Do not approve on behalf of a human.
- Do not mark a criterion as passed without evidence.

## Finding Severity

- Critical: workflow must stop
- Major: correction required before handover
- Minor: improvement recommended
- Observation: no immediate correction required

## Output

- projects/<project>/working/reviews/<artifact-id>-review.md
```



### Ergebnis dieses Schrittes

Der MVP besitzt einen Orchestrator sowie vier klar getrennte Testrollen.

---



# Teil VI: Skills entwickeln



## DONE 16. Schritt 12: Skill für Testbasisanalyse erstellen

Erstelle `skills/analyze-test-basis/SKILL.md`.

```markdown
---
name: analyze-test-basis
description: Analyze requirements and other test basis documents for testability, ambiguity, contradiction and missing information.
---

# Analyze Test Basis

## Use This Skill When

- a new requirement or user story enters the workflow,
- test conditions must be derived,
- the quality of the test basis is unknown,
- requirements changed.

## Required Inputs

- requirement or user story
- acceptance criteria
- relevant business rules
- known constraints

## Procedure

1. Identify each explicit requirement statement.
2. Assign or preserve a unique requirement identifier.
3. Identify actors, triggers, conditions, actions and outcomes.
4. Check whether the expected behaviour is observable.
5. Check for ambiguous wording.
6. Check for missing boundaries and error behaviour.
7. Check for contradictions with other sources.
8. Identify assumptions required for testing.
9. Separate facts, assumptions and questions.
10. Produce an analysis table.

## Output Format

| Requirement ID | Finding Type | Finding | Severity | Test Impact | Proposed Clarification |
|---|---|---|---|---|---|

## Quality Criteria

- No invented requirement details.
- Every finding references its source.
- Ambiguities are explained, not only labelled.
- Test impact is stated.
```

---



## DONE 17. Schritt 13: Skill für Produktrisikoanalyse erstellen

Erstelle `skills/product-risk-analysis/SKILL.md`.

```markdown
---
name: product-risk-analysis
description: Identify and assess product quality risks to support risk-based testing decisions.
---

# Product Risk Analysis

## Risk Model

Risk Exposure = Likelihood x Impact

Use a scale from 1 to 5 for likelihood and impact.

## Procedure

1. Identify product features and quality characteristics.
2. Formulate concrete failure events.
3. Describe possible consequences.
4. Estimate likelihood from 1 to 5.
5. Estimate impact from 1 to 5.
6. Calculate the risk exposure.
7. Assign a risk class.
8. Recommend a test response.
9. Record assumptions and evidence.

## Risk Classes

- 16 to 25: Critical
- 10 to 15: High
- 5 to 9: Medium
- 1 to 4: Low

## Output Format

| Risk ID | Failure Event | Consequence | Likelihood | Impact | Exposure | Class | Test Response |
|---|---|---|---:|---:|---:|---|---|

## Guardrails

- Do not confuse project risks with product risks.
- Do not assign scores without explaining the rationale.
- Do not present estimates as measured facts.
```

---



## DONE 18. Schritt 14: Skill für Testbedingungen erstellen

Erstelle `skills/derive-test-conditions/SKILL.md`.

```markdown
---
name: derive-test-conditions
description: Derive traceable and risk-focused test conditions from the test basis and product risks.
---

# Derive Test Conditions

## Procedure

1. Read the reviewed test basis analysis.
2. Read relevant product risks.
3. Identify testable behaviours and quality attributes.
4. Derive positive test conditions.
5. Derive negative test conditions.
6. Derive boundary and exception conditions.
7. Consider relevant non-functional conditions.
8. Assign priority based on risk.
9. Link every condition to its source.
10. Recommend a suitable test technique.

## Rules

- A test condition describes what should be tested, not detailed steps.
- Test conditions must be independently understandable.
- Duplicate conditions should be consolidated.
- Critical risks require explicit coverage.
```

---



## DONE 19. Schritt 15: Skill für Testfalldesign erstellen

Erstelle `skills/design-test-cases/SKILL.md`.

```markdown
---
name: design-test-cases
description: Design concrete test cases using suitable black-box, experience-based and collaboration-based test techniques.
---

# Design Test Cases

## Technique Selection

Use equivalence partitioning when:

- inputs can be divided into groups with equivalent behaviour.

Use boundary value analysis when:

- ordered ranges, limits or thresholds exist.

Use decision table testing when:

- combinations of conditions determine outcomes.

Use state transition testing when:

- behaviour depends on current state and events.

Use exploratory charters when:

- learning, investigation and adaptability are important.

## Procedure

1. Select one reviewed test condition.
2. Identify the most suitable test technique.
3. Define coverage items.
4. Create the smallest meaningful test set.
5. Define preconditions and test data.
6. Define steps and expected results.
7. Link each case to its coverage item.
8. Mark automation suitability.
9. Review for redundant cases.
```

---



## DONE 20. Schritt 16: Review-Skill erstellen

Erstelle `skills/review-test-artifact/SKILL.md`.

```markdown
---
name: review-test-artifact
description: Review a test artifact against its template, quality gate, traceability rules and source inputs.
---

# Review Test Artifact

## Procedure

1. Identify artifact type and version.
2. Load the matching template.
3. Load the applicable quality gate.
4. Check mandatory metadata.
5. Check completeness.
6. Check consistency with source inputs.
7. Check traceability.
8. Check for unsupported statements.
9. Classify findings.
10. Produce a review recommendation.

## Recommendation Values

- PASS
- CONDITIONAL PASS
- FAIL

## Mandatory Evidence

Every passed criterion must reference:

- a section,
- a table row,
- an identifier,
- or another explicit artifact location.
```



### Ergebnis dieses Schrittes

Fachlogik ist nicht mehr fest in einzelne Agenten eingebaut. Sie liegt in
wiederverwendbaren Skills.

---



# Teil VII: Workflows definieren



## DONE 21. Schritt 17: Workflow für Test Context Analysis erstellen

Erstelle `workflows/01-test-context-analysis.workflow.md`.

```markdown
# Workflow 01: Test Context Analysis

## Purpose

Create a shared and reviewable understanding of the test assignment before
planning or test design starts.

## Trigger

- new project,
- new release,
- major scope change,
- missing test context.

## Responsible Agent

- test-orchestrator

## Required Inputs

- product description or user request
- available requirements
- known stakeholders
- known constraints

## Steps

1. Inventory available input documents.
2. Create a source list.
3. Identify product and business goal.
4. Define preliminary scope.
5. Identify stakeholders and approval roles.
6. Identify constraints and dependencies.
7. Record known quality risks.
8. Record missing information.
9. Create the test context artifact.
10. Run Quality Gate QG-01.
11. Create handover to Test Manager or Test Analyst.

## Output

- projects/<project>/working/test-context.md
- projects/<project>/working/handovers/context-handover.md

## Stop Conditions

Stop the workflow when:

- the product under test cannot be identified,
- no test assignment exists,
- scope cannot be distinguished from assumptions,
- required legal or security constraints are unknown.
```

---



## DONE 22. Schritt 18: Workflow für Test Analysis erstellen

Erstelle `workflows/03-test-analysis.workflow.md`.

```markdown
# Workflow 03: Test Analysis

## Purpose

Transform the test basis and product risks into reviewed test conditions.

## Responsible Agent

- test-analyst

## Required Inputs

- test context
- in-scope requirements
- product risk analysis
- applicable policies

## Skills

- analyze-test-basis
- derive-test-conditions

## Steps

1. Validate required inputs.
2. Analyze the test basis.
3. Record ambiguities and contradictions.
4. Derive candidate test conditions.
5. Link conditions to requirements and risks.
6. Assign priority.
7. Recommend test techniques.
8. Save the test conditions artifact.
9. Request independent review.
10. Apply required corrections.
11. Run Quality Gate QG-02.
12. Create handover to Test Designer.

## Output

- test-basis-analysis.md
- test-conditions.md
- test-conditions-review.md
- analysis-to-design-handover.md
```

---



## 23. Schritt 19: Workflow für Test Design erstellen

Erstelle `workflows/04-test-design.workflow.md`.

```markdown
# Workflow 04: Test Design

## Purpose

Create executable, traceable and risk-focused test cases from reviewed test
conditions.

## Responsible Agent

- test-designer

## Required Inputs

- reviewed test conditions
- relevant requirements
- test data constraints
- environment information

## Skills

- design-test-cases

## Steps

1. Validate that test conditions are review-ready or approved.
2. Group conditions by feature, risk and technique.
3. Select appropriate test design techniques.
4. Define coverage items.
5. Create test cases.
6. Define test data requirements.
7. Mark automation candidates.
8. Check traceability.
9. Save the test case artifact.
10. Request independent review.
11. Apply corrections.
12. Run Quality Gate QG-03.
13. Prepare handover to implementation or execution.
```



### Ergebnis dieses Schrittes

Die Reihenfolge der Agentenarbeit ist jetzt explizit beschrieben.

---



# Teil VIII: Handover und Workflow State



## DONE 24. Schritt 20: Handover-Template erstellen

Erstelle `templates/handover.template.md`.

```markdown
---
artifact_id: <ID>
artifact_type: handover
project: <PROJECT>
version: 0.1
status: draft
created_by: <AGENT>
created_at: <DATE>
---

# Handover

## From

## To

## Completed Workflow Step

## Artifacts Delivered

| Artifact ID | File | Version | Status |
|---|---|---|---|

## Quality Gate Result

## Important Decisions

## Assumptions

## Open Questions

## Known Limitations

## Required Next Action

## Human Approval Required

- [ ] Yes
- [ ] No

## Handover Acceptance

- Status: pending
- Accepted by:
- Date:
- Comments:
```



### Warum ein Handover notwendig ist

Ein Dateiname allein erklärt nicht:

- was abgeschlossen wurde,
- welche Annahmen gelten,
- welche Probleme offen sind,
- welche Qualität geprüft wurde,
- und was der nächste Agent tun soll.

Das Handover ist daher ein eigenes Artefakt.

---



## DONE 25. Schritt 21: Workflowstatus definieren

Erstelle `schemas/workflow-state.schema.yaml`.

```yaml
project: string
current_phase: string
current_step: string
status:
  allowed:
    - not-started
    - ready
    - in-progress
    - blocked
    - in-review
    - awaiting-human-approval
    - completed
active_agent: string
required_inputs: array
available_artifacts: array
blockers: array
next_agent: string
next_action: string
last_updated: date-time
```

Erstelle für ToolShop:

`projects/toolshop/status/workflow-state.yaml`

```yaml
project: toolshop
current_phase: context-analysis
current_step: inventory-inputs
status: ready
active_agent: test-orchestrator
required_inputs:
  - product-description
available_artifacts: []
blockers: []
next_agent: test-orchestrator
next_action: create-test-context
last_updated: 2026-07-14T14:00:00+02:00
```



### OK DONE - Ergebnis dieses Schrittes

Der Orchestrator muss den Projektstatus nicht aus Chatverläufen erraten.

---



# Teil IX: Quality Gates entwickeln



## DONE 26. Schritt 22: Quality Gate für Test Context erstellen

Erstelle `quality-gates/qg-01-context-ready.md`.

```markdown
# QG-01: Test Context Ready

## Purpose

Confirm that sufficient context exists before planning or test analysis starts.

## Criteria

- [ ] Product under test is identified.
- [ ] Business goal is documented.
- [ ] Test assignment is documented.
- [ ] In-scope items are listed.
- [ ] Out-of-scope items are listed or explicitly unknown.
- [ ] Test basis sources are listed.
- [ ] Relevant stakeholders are identified.
- [ ] Constraints are documented.
- [ ] Assumptions are explicitly labelled.
- [ ] Open questions are documented.
- [ ] No human approval is falsely claimed.

## Decision Rule

- PASS: all mandatory criteria are satisfied.
- CONDITIONAL PASS: no critical gap exists and remaining gaps are explicitly
  accepted for the next step.
- FAIL: a critical input is missing or scope cannot be determined.
```

---



## DONE 27. Schritt 23: Quality Gate für Test Analysis erstellen

Erstelle `quality-gates/qg-02-analysis-ready.md`.

```markdown
# QG-02: Test Analysis Ready

## Criteria

- [ ] Every test condition has a unique ID.
- [ ] Every test condition references a source.
- [ ] Critical and high product risks have explicit coverage.
- [ ] Positive and negative conditions are considered.
- [ ] Relevant boundaries and exceptions are considered.
- [ ] Priorities are assigned.
- [ ] Suggested techniques are reasonable.
- [ ] Duplicates are removed or justified.
- [ ] Ambiguities are documented.
- [ ] The artifact received an independent review.

## Fail Conditions

The gate fails when:

- critical risks have no test condition,
- conditions are not traceable,
- invented requirements are used,
- or the independent review reports unresolved critical findings.
```

---



## 28. Schritt 24: Quality Gate für Test Design erstellen

Erstelle `quality-gates/qg-03-design-ready.md`.

```markdown
# QG-03: Test Design Ready

## Criteria

- [ ] Every test case has a unique ID.
- [ ] Every test case links to a reviewed test condition.
- [ ] Every test case has an observable expected result.
- [ ] Preconditions are separated from steps.
- [ ] Test data is synthetic, approved or parameterized.
- [ ] Coverage items from the selected technique are represented.
- [ ] Critical tests are clearly prioritized.
- [ ] Redundant test cases are removed or justified.
- [ ] Automation candidates are marked.
- [ ] An independent review has been completed.

## Decision Rule

A PASS means that the test design is ready for implementation or execution.
It does not mean that tests were executed successfully.
```



### Ergebnis dieses Schrittes

Agenten können nicht allein aufgrund einer überzeugend klingenden Antwort zum
nächsten Schritt wechseln.

---



# Teil X: Den ersten End-to-End-MVP ausführen



## 29. Schritt 25: Ein kleines ToolShop-Beispiel vorbereiten

Erstelle:

`projects/toolshop/input/registration-user-story.md`

```markdown
# User Story: User Registration

As a visitor,
I want to create a customer account,
so that I can order products from the ToolShop.

## Acceptance Criteria

1. The user can register with first name, last name, email and password.
2. The email address must be unique.
3. The password must contain at least eight characters.
4. After successful registration, the user can log in.
5. Invalid input is rejected and an error message is displayed.
```

Erstelle:

`projects/toolshop/input/product-description.md`

```markdown
# ToolShop Product Description

ToolShop is a web application that allows customers to browse tools, create an
account, log in and place orders.

The first TTT426 workflow increment focuses on customer registration.
```

---



## 30. Schritt 26: Orchestrator starten

Verwende in deiner Agent-Umgebung folgenden Auftrag:

```text
Use the TTT426 Test Orchestrator.

Project: toolshop

Inspect:
- AGENTS.md
- instructions/
- projects/toolshop/input/
- projects/toolshop/status/workflow-state.yaml

Execute only Workflow 01: Test Context Analysis.
Create the required artifacts in the project working folder.
Run QG-01 and record the evidence.
Do not create test conditions or test cases yet.
```



### Manuelle Prüfung

Prüfe anschließend:

- Wurde ein Test Context erzeugt?
- Sind Quellen genannt?
- Sind Annahmen erkennbar?
- Wurde kein Testfall erzeugt?
- Ist QG-01 nachvollziehbar bewertet?
- Existiert ein Handover?

Committe das Ergebnis erst nach deiner Prüfung.

```bash
git add projects/toolshop
git commit -m "Create ToolShop test context through orchestrated workflow"
```

---



## 31. Schritt 27: Test Analysis ausführen

Verwende folgenden Auftrag:

```text
Use the TTT426 Test Analyst.

Project: toolshop

Read the current workflow state, the reviewed test context, the registration
user story and applicable instructions.

Execute Workflow 03: Test Analysis.
Use the analyze-test-basis and derive-test-conditions skills.
Create:
- test-basis-analysis.md
- test-conditions.md
- analysis-to-design-handover.md

Do not create detailed test cases.


```

17.7 Artifacts created
projects/toolshop/working/test-basis-analysis.md — ambiguities documented (email syntax, password rules beyond length 8, undefined “invalid input”, error-message expectations)
projects/toolshop/working/test-conditions.md — TC-REG-001–009, traced to ACs and risks; email uniqueness + password BVA covered; no invented UI/format rules
projects/toolshop/working/handovers/analysis-to-design-handover.md
projects/toolshop/status/workflow-state.yaml updated → awaiting review

Danach startet der Reviewer:

```text
Use the TTT426 Test Reviewer.
Review the ToolShop test conditions against:
- the source user story,
- the test conditions template,
- QG-02,
- the core guardrails.

Record evidence for every passed criterion.
Do not rewrite the test conditions.
```



### Manuelle Prüfung

Kontrolliere insbesondere:

- Deckt der Agent die E-Mail-Eindeutigkeit ab?
- Erkennt er unklare Regeln zur E-Mail-Syntax?
- Erkennt er unklare Passwortregeln?
- Vermeidet er erfundene UI-Details?
- Verknüpft er Bedingungen mit den Acceptance Criteria?

---



## 32. Schritt 28: Test Design ausführen

Nach erfolgreichem Review:

```text
Use the TTT426 Test Designer.

Execute Workflow 04 for the reviewed ToolShop registration test conditions.
Use appropriate test design techniques.
Create concrete manual test cases only.
Do not create Playwright code.
Mark suitable automation candidates.
Run the defined review and quality gate process.
```



### Erwartete Techniken

Für die Beispielstory sind wahrscheinlich geeignet:

- Equivalence Partitioning für gültige und ungültige Eingaben
- Boundary Value Analysis für die Passwortlänge
- Decision Table Testing für Kombinationen von Eingabegültigkeit
- Error Guessing für bereits registrierte E-Mail-Adressen

Der Agent muss seine Technikentscheidung begründen.

---



# Teil XI: Agentensystem testen



## 33. Schritt 29: Agent Tests definieren

Agenten benötigen Tests wie andere Softwarekomponenten.

Erstelle `tests/agent-tests/test-analyst-tests.md`.

```markdown
# Test Analyst Agent Tests

## AT-TA-001: Missing Requirement

### Input

A user request without an identifiable requirement source.

### Expected Behaviour

- Agent reports the missing source.
- Agent does not invent requirements.
- Workflow is marked blocked or conditional.

## AT-TA-002: Ambiguous Password Rule

### Input

"The password must be secure."

### Expected Behaviour

- Agent identifies "secure" as ambiguous.
- Agent requests measurable criteria.
- Agent may derive a provisional condition labelled as assumption-dependent.

## AT-TA-003: Contradictory Rules

### Input

Source A: Password requires at least 8 characters.
Source B: Password requires at least 12 characters.

### Expected Behaviour

- Agent reports the contradiction.
- Agent does not silently select one value.
- Detailed test design is blocked until clarified or explicitly parameterized.
```

---



## 34. Schritt 30: Skill Tests definieren

Erstelle `tests/skill-tests/design-test-cases-tests.md`.

```markdown
# Design Test Cases Skill Tests

## ST-DTC-001: Boundary Value Analysis

### Input

Password length must be at least 8 characters.

### Expected Coverage

At minimum:

- 7 characters
- 8 characters
- 9 characters

The result must not claim additional maximum length rules.

## ST-DTC-002: Unsupported UI Detail

### Input

A requirement defines an error message but not its visual location.

### Expected Behaviour

The test case checks that an error message is displayed but does not invent
colour, position or component type.
```

---



## 35. Schritt 31: Workflow Tests definieren

Erstelle `tests/workflow-tests/context-to-design-e2e.md`.

```markdown
# E2E Workflow Test: Context to Design

## Preconditions

- ToolShop input files exist.
- Workflow state is set to context-analysis.
- No output artifacts exist.

## Execution

1. Run Test Context Analysis.
2. Review QG-01.
3. Run Test Analysis.
4. Review QG-02.
5. Run Test Design.
6. Review QG-03.

## Expected Results

- Steps execute in the defined order.
- Each step creates a handover.
- No detailed test cases exist before test analysis is complete.
- The reviewer is different from the producing role.
- Failed gates prevent the next workflow transition.
- Every test case links to a test condition.
```



### Ergebnis dieses Schrittes

Du kannst Änderungen an Agenten und Skills regressiv prüfen.

---



# Teil XII: Einfache Automatisierung hinzufügen



## 36. Schritt 32: Artifact Validator bauen

Erstelle `scripts/validate_artifact.py`.

```python
from __future__ import annotations

import sys
from pathlib import Path

import yaml


REQUIRED_FIELDS = {
    "artifact_id",
    "title",
    "artifact_type",
    "project",
    "version",
    "status",
    "created_by",
    "created_at",
    "source_inputs",
    "assumptions",
    "open_questions",
}


def extract_frontmatter(content: str) -> dict:
    if not content.startswith("---\n"):
        raise ValueError("Artifact has no YAML frontmatter.")

    parts = content.split("---", 2)
    if len(parts) < 3:
        raise ValueError("Artifact frontmatter is not closed.")

    metadata = yaml.safe_load(parts[1])
    if not isinstance(metadata, dict):
        raise ValueError("Artifact frontmatter must contain a YAML object.")

    return metadata


def validate(path: Path) -> list[str]:
    errors: list[str] = []

    try:
        content = path.read_text(encoding="utf-8")
        metadata = extract_frontmatter(content)
    except (OSError, ValueError, yaml.YAMLError) as exc:
        return [str(exc)]

    missing = REQUIRED_FIELDS.difference(metadata.keys())
    if missing:
        errors.append(f"Missing metadata fields: {sorted(missing)}")

    if metadata.get("status") not in {
        "draft",
        "in-review",
        "changes-requested",
        "approved",
        "rejected",
        "obsolete",
    }:
        errors.append("Invalid artifact status.")

    return errors


def main() -> int:
    if len(sys.argv) != 2:
        print("Usage: python validate_artifact.py <artifact.md>")
        return 2

    artifact_path = Path(sys.argv[1])
    errors = validate(artifact_path)

    if errors:
        print(f"Validation failed for {artifact_path}:")
        for error in errors:
            print(f"- {error}")
        return 1

    print(f"Validation passed for {artifact_path}")
    return 0


if __name__ == "__main__":
    raise SystemExit(main())
```

Installiere PyYAML:

```bash
python -m pip install pyyaml
```

Führe die Prüfung aus:

```bash
python scripts/validate_artifact.py projects/toolshop/working/test-context.md
```



### Spätere Erweiterungen

Der Validator kann später prüfen:

- eindeutige IDs,
- erlaubte Statusübergänge,
- vorhandene Source Inputs,
- Traceability Links,
- Pflichtabschnitte,
- offene kritische Findings,
- menschliche Freigaben.

---



## 37. Schritt 33: GitHub Action für Artifact Validation hinzufügen

Erstelle `.github/workflows/validate-artifacts.yml`.

```yaml
name: Validate TTT426 Artifacts

on:
  pull_request:
    paths:
      - "projects/**/*.md"
      - "schemas/**"
      - "scripts/validate_artifact.py"

jobs:
  validate:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.12"

      - name: Install dependencies
        run: python -m pip install pyyaml

      - name: Validate Markdown artifacts
        run: |
          find projects -name "*.md" -print0 | while IFS= read -r -d '' file; do
            if head -n 1 "$file" | grep -q '^---$'; then
              python scripts/validate_artifact.py "$file"
            fi
          done
```



### Ergebnis dieses Schrittes

Formale Qualitätsregeln werden nicht nur dem LLM überlassen.

---



# Teil XIII: Human-in-the-Loop integrieren



## 38. Schritt 34: Freigabepunkte festlegen

Definiere zunächst nur wenige, aber wichtige Freigaben.

### Human Gate H1: Test Context Acceptance

Verantwortlich:

- Product Owner oder Auftraggeber
- Test Lead

Prüft:

- Scope
- Testauftrag
- Annahmen
- fehlende Informationen



### Human Gate H2: Test Plan Approval

Verantwortlich:

- Test Lead
- Projektleitung oder Product Owner, abhängig vom Projekt



### Human Gate H3: Test Design Acceptance

Verantwortlich:

- Test Lead oder Senior Test Engineer



### Human Gate H4: Release Recommendation

Verantwortlich:

- benannte menschliche Entscheidungsrolle

Ein Agent darf für jedes Gate ein Entscheidungspaket vorbereiten:

```markdown
# Approval Package

## Decision Requested

## Recommended Decision

## Evidence

## Open Risks

## Deviations

## Consequence of Approval

## Consequence of Rejection
```

Der Agent darf das Feld `approved_by` nicht selbst befüllen.

---



# Teil XIV: TTT426-spezifisches Wissen integrieren



## 39. Schritt 35: TTT426 Context File erstellen

Erstelle `knowledge/ttt426-context.md`.

```markdown
# TTT426 Context

## Purpose

TTT426 is a hands-on software testing project in which participants apply the
complete software test process to a realistic product increment.

## Learning Principles

- Practice before theory-only documentation.
- Participants take responsibility for concrete test roles.
- Test artifacts must be usable in a real project.
- Missing and changing information is part of the learning scenario.
- Results must be reviewable and traceable.

## Product Under Test

The default training product is ToolShop, based on the Practice Software
Testing application.

## Test Management

Testiny may be used for test management and result reporting.

## Test Automation

Playwright is the preferred UI automation technology for the current learning
context.

## Important Boundary

The Agentic Test Engineering Workflow supports the participants. It must not
replace their learning, reflection, review and accountability.
```

---



## 40. Schritt 36: Terminologie definieren

Erstelle `instructions/terminology.md`.

Nutze verbindliche Definitionen für mindestens:

- Test Basis
- Test Object
- Test Condition
- Test Case
- Test Procedure
- Test Level
- Test Type
- Test Approach
- Product Risk
- Project Risk
- Test Coverage
- Entry Criteria
- Exit Criteria
- Test Completion

Beispiel:

```markdown
## Test Condition

A testable aspect of a component or system identified as a basis for testing.

TTT426 usage rule:

A test condition describes what must be tested. It does not contain detailed
execution steps.
```

Die Terminologiedatei verhindert, dass verschiedene Agenten Begriffe
unterschiedlich verwenden.

---



# Teil XV: Erweiterung nach dem MVP



## 41. Schritt 37: Weitere Agenten priorisieren

Füge nicht alle Agenten gleichzeitig hinzu.

Empfohlene Reihenfolge:

### Ausbaustufe 1

- Test Orchestrator
- Test Manager
- Test Analyst
- Test Designer
- Test Reviewer



### Ausbaustufe 2

- Test Data Agent
- Exploratory Testing Agent
- Defect Analyst Agent
- Test Execution Coordinator



### Ausbaustufe 3

- Test Automation Architect Agent
- Playwright Automation Agent
- Automation Reviewer Agent
- CI/CD Integration Agent



### Ausbaustufe 4

- Test Summary Report Agent
- Quality Gate Agent
- Compliance Review Agent
- Learning Coach Agent

Jeder neue Agent benötigt:

1. klaren Zweck,
2. klar definierte Inputs,
3. klar definierte Outputs,
4. mindestens einen Skill,
5. Guardrails,
6. Agent Tests,
7. einen Platz im Workflow,
8. eine definierte Handover-Beziehung.

---



## 42. Schritt 38: Test Data Agent ergänzen

Der Test Data Agent sollte später unter anderem:

- Testdatenanforderungen aus Testfällen ableiten,
- sensible Daten erkennen,
- synthetische Daten vorschlagen,
- Datenabhängigkeiten dokumentieren,
- Testdatenkollisionen zwischen manuellen und automatisierten Tests verhindern,
- API-basierte Datenerzeugung vorbereiten,
- und Data Cleanup berücksichtigen.

Er sollte nicht:

- echte Credentials erzeugen,
- Produktionsdaten ungeprüft kopieren,
- oder irreversible Datenbankänderungen durchführen.

---



## 43. Schritt 39: Test Automation Workflow ergänzen

Der spätere Automation Workflow könnte so aussehen:

```text
Reviewed Manual Test Case
    |
    v
Automation Suitability Analysis
    |
    v
Automation Design
    |
    v
Test Data and Environment Check
    |
    v
Playwright Implementation
    |
    v
Static Review
    |
    v
Execution
    |
    v
Result Import into Testiny
```

Trenne mindestens:

- Automation Architect
- Automation Implementer
- Automation Reviewer

Der implementierende Agent sollte seinen eigenen Code nicht allein freigeben.

---



# Teil XVI: Plattformintegration



## 44. Schritt 40: Nutzung in Cursor

Für Cursor kannst du die gleichen Grundbausteine verwenden:

- Repository Rules oder zentrale Anweisungsdateien für globale Regeln
- Custom Agents oder Subagents für spezialisierte Rollen
- Agent Skills mit `SKILL.md` für wiederverwendbare Fähigkeiten
- MCP für kontrollierten Zugriff auf externe Systeme
- versionierte Markdown-Workflows für die Ablaufsteuerung

Prüfe nach jeder Cursor-Aktualisierung die aktuelle offizielle Dokumentation,
da sich Dateipfade und Funktionsumfang ändern können.

### Empfohlene Zuordnung


| TTT426 Element          | Cursor-Konzept                            |
| ----------------------- | ----------------------------------------- |
| AGENTS.md               | Projektkontext und gemeinsame Anweisungen |
| agents/*.agent.md       | Custom Agents oder Subagents              |
| skills/*/SKILL.md       | Agent Skills                              |
| workflows/*.workflow.md | explizite Workflow-Anweisungen            |
| quality-gates/*.md      | Review Rules und Checklisten              |
| scripts/                | deterministische Validierung              |


---



## 45. Schritt 41: Nutzung mit GitHub Copilot

GitHub Copilot und VS Code unterstützen repository-basierte Custom
Instructions, Custom Agents und Agent Skills. Skills werden als Ordner mit
einer `SKILL.md` und optionalen Ressourcen oder Skripten organisiert.

### Empfohlene Zuordnung


| TTT426 Element        | GitHub Copilot-Konzept                |
| --------------------- | ------------------------------------- |
| globale Regeln        | repository custom instructions        |
| spezialisierte Rollen | custom agents                         |
| Fachmethoden          | agent skills                          |
| technische Checks     | scripts und GitHub Actions            |
| Review des Outputs    | Pull Request Review und Quality Gates |


Halte die Kernlogik möglichst plattformneutral. Plattformabhängige Wrapper
sollten nur auf die gemeinsamen Dateien verweisen.

---



# Teil XVII: Governance und Sicherheit



## 46. Schritt 42: Agent Registry erstellen

Erstelle eine zentrale Übersicht, zum Beispiel `agents/registry.yaml`.

```yaml
agents:
  - id: test-orchestrator
    owner: TTT426 Test Lead
    status: active
    risk_level: medium
    allowed_write_paths:
      - projects/*/working
      - projects/*/status
    human_approval_required: false

  - id: test-manager
    owner: TTT426 Test Lead
    status: active
    risk_level: medium
    allowed_write_paths:
      - projects/*/working
    human_approval_required: true

  - id: test-reviewer
    owner: TTT426 Test Lead
    status: active
    risk_level: low
    allowed_write_paths:
      - projects/*/working/reviews
    human_approval_required: false
```

Dokumentiere pro Agent:

- Owner
- Version
- Modell oder Plattform
- erlaubte Tools
- erlaubte Schreibbereiche
- Datenzugriff
- Risikoeinstufung
- erforderliche Freigaben
- letzte Evaluierung

---



## 47. Schritt 43: Audit Trail definieren

Jeder relevante Agentenlauf sollte mindestens dokumentieren:

- Agentenname und Version
- verwendetes Modell, soweit verfügbar
- Zeitpunkt
- Auftrag
- gelesene Artefakte
- verwendete Skills
- erzeugte oder geänderte Dateien
- Annahmen
- Quality-Gate-Ergebnis
- menschliche Entscheidung

Ein einfaches Logformat:

```yaml
run_id: RUN-2026-07-14-001
agent: test-analyst
agent_version: 0.1
project: toolshop
workflow: 03-test-analysis
inputs:
  - TTT426-TC-001
  - US-REG-001
skills:
  - analyze-test-basis
  - derive-test-conditions
outputs:
  - TTT426-TCND-001
quality_gate: QG-02
result: conditional-pass
human_review: pending
```

---



# Teil XVIII: Erfolg messen



## 48. Schritt 44: Qualitätsmetriken definieren

Bewerte nicht nur Geschwindigkeit.

Geeignete Metriken:

### Artifact Quality

- Anteil der Artefakte, die das Review beim ersten Versuch bestehen
- durchschnittliche Anzahl Major Findings
- Anteil nicht belegter Aussagen
- Anteil fehlender Pflichtfelder



### Traceability

- Anteil der Testbedingungen mit Requirement-Link
- Anteil der Testfälle mit Test-Condition-Link
- Anteil kritischer Risiken mit Testabdeckung



### Workflow Reliability

- Anzahl übersprungener Quality Gates
- Anzahl fehlerhafter Statusübergänge
- Anzahl blockierter Läufe wegen fehlenden Kontexts
- Wiederholbarkeit bei identischem Input



### Human Effort

- Reviewzeit pro Artefakt
- Anzahl notwendiger Korrekturschleifen
- Anteil akzeptierter Agentenvorschläge



### Learning Value

- Können Teilnehmer Agentenergebnisse kritisch erklären?
- Können Teilnehmer Fehler im Agentenoutput erkennen?
- Können Teilnehmer Testtechniken selbst auswählen und begründen?
- Verbessert der Workflow das Verständnis des Testprozesses?

---



# Teil XIX: Empfohlener Implementierungsplan



## 49. Vier Iterationen zum ersten stabilen Workflow



### Iteration 1: Repository und Context Workflow

Implementiere:

- Verzeichnisstruktur
- AGENTS.md
- Guardrails
- Test Orchestrator
- Test Context Template
- Workflow 01
- QG-01
- Workflow State

Definition of Done:

- Der Orchestrator erzeugt reproduzierbar einen Test Context.
- Er erzeugt keine Testfälle.
- Fehlende Inputs werden sichtbar.
- Ein Handover wird erzeugt.



### Iteration 2: Test Analysis

Implementiere:

- Test Analyst
- Analyze Test Basis Skill
- Product Risk Analysis Skill
- Derive Test Conditions Skill
- Test Conditions Template
- Workflow 03
- QG-02
- Reviewer

Definition of Done:

- Anforderungen und Annahmen bleiben getrennt.
- Testbedingungen sind traceable.
- Kritische Risiken besitzen Abdeckung.
- Ein unabhängiger Reviewbericht existiert.



### Iteration 3: Test Design

Implementiere:

- Test Designer
- Design Test Cases Skill
- Test Cases Template
- Workflow 04
- QG-03
- Skill Tests

Definition of Done:

- Testfälle besitzen erwartete Ergebnisse.
- Testtechniken sind erkennbar angewandt.
- Testfälle verweisen auf Testbedingungen.
- Automatisierungskandidaten sind markiert.



### Iteration 4: Automation und Governance

Implementiere:

- Artifact Validator
- GitHub Action
- Agent Registry
- Audit Trail
- E2E Workflow Tests
- Human Approval Package

Definition of Done:

- Formale Artefaktfehler werden automatisch erkannt.
- Pull Requests führen Validierungen aus.
- Agentenbesitz und Zugriffsrechte sind dokumentiert.
- Menschliche Freigaben sind nachvollziehbar.

---



# Teil XX: Häufige Fehler



## 50. Vermeide diese Anti-Patterns



### Ein Agent für alles

Problem:

Der Agent vermischt Planung, Analyse, Design, Ausführung und Review.

Lösung:

Rollen und Übergaben trennen.

### Nur Chat, keine Artefakte

Problem:

Ergebnisse verschwinden im Verlauf und sind nicht versionierbar.

Lösung:

Artifact First anwenden.

### LLM als alleinige Quality Gate

Problem:

Das gleiche Modell erzeugt und genehmigt seine eigene Antwort.

Lösung:

Unabhängiger Review, deterministische Checks und menschliche Freigabe
kombinieren.

### Zu große Context Files

Problem:

Der Agent erhält zu viele irrelevante oder widersprüchliche Informationen.

Lösung:

Aufgabenspezifische Context Packages erstellen.

### Unklare Agentenautonomie

Problem:

Es ist nicht definiert, was ein Agent ändern oder ausführen darf.

Lösung:

Toolrechte, Schreibbereiche und Approval Gates dokumentieren.

### Keine Tests für Prompts und Skills

Problem:

Eine kleine Änderung verursacht unbemerkte Regressionen.

Lösung:

Agent Tests, Skill Tests und Workflow Tests versionieren.

### Übernahme überzeugender, aber unbelegter Aussagen

Problem:

Sprachliche Qualität wird mit fachlicher Richtigkeit verwechselt.

Lösung:

Evidence-based Reviews und Traceability verlangen.

---



# Teil XXI: Deine unmittelbare Start-Checkliste



## 51. Die ersten zehn konkreten Aktionen

1. Repository erstellen.
2. Verzeichnisstruktur anlegen.
3. `AGENTS.md` hinzufügen.
4. Core Guardrails hinzufügen.
5. ToolShop Input Story hinzufügen.
6. Test Context Template erstellen.
7. Test Orchestrator Agent erstellen.
8. Workflow 01 erstellen.
9. QG-01 erstellen.
10. Den ersten Context-Workflow ausführen und manuell reviewen.

Beginne erst danach mit Test Analyst und Test Designer.

---



# Teil XXII: Zielstruktur des ersten MVP

Nach den ersten drei Iterationen sollte dein Repository mindestens folgende
funktionsfähige Bestandteile enthalten:

```text
AGENTS.md
agents/
  test-orchestrator.agent.md
  test-analyst.agent.md
  test-designer.agent.md
  test-reviewer.agent.md
skills/
  analyze-test-basis/SKILL.md
  derive-test-conditions/SKILL.md
  design-test-cases/SKILL.md
  review-test-artifact/SKILL.md
workflows/
  01-test-context-analysis.workflow.md
  03-test-analysis.workflow.md
  04-test-design.workflow.md
templates/
  test-context.template.md
  test-conditions.template.md
  test-cases.template.md
  handover.template.md
quality-gates/
  qg-01-context-ready.md
  qg-02-analysis-ready.md
  qg-03-design-ready.md
projects/toolshop/
  input/
  working/
  output/
  decisions/
  status/
tests/
  agent-tests/
  skill-tests/
  workflow-tests/
```

Das ist noch kein vollautonomes Multi-Agent-System. Es ist aber bereits ein
kontrollierter, dateibasierter und überprüfbarer Agentic Test Engineering
Workflow.

Genau diese Basis ist notwendig, bevor du MCP-Tools, automatische Agentenketten,
Testiny, Playwright, GitHub Issues oder andere externe Systeme integrierst.

---



# Quellen und weiterführende Dokumentation

Die folgenden Quellen wurden für die strukturelle Einordnung verwendet. Prüfe
bei der technischen Umsetzung jeweils die aktuellste Version der offiziellen
Dokumentation.

- BMAD Method Repository: [https://github.com/bmad-code-org/BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD)
- BMAD Getting Started: [https://github.com/bmad-code-org/BMAD-METHOD/blob/main/docs/tutorials/getting-started.md](https://github.com/bmad-code-org/BMAD-METHOD/blob/main/docs/tutorials/getting-started.md)
- BMAD Workflow Map: [https://github.com/bmad-code-org/BMAD-METHOD/blob/main/docs/reference/workflow-map.md](https://github.com/bmad-code-org/BMAD-METHOD/blob/main/docs/reference/workflow-map.md)
- BMAD Documentation: [https://docs.bmad-method.org/](https://docs.bmad-method.org/)
- Cursor Documentation: [https://cursor.com/docs](https://cursor.com/docs)
- Cursor Agent Skills: [https://cursor.com/docs/skills](https://cursor.com/docs/skills)
- VS Code Agent Skills: [https://code.visualstudio.com/docs/copilot/customization/agent-skills](https://code.visualstudio.com/docs/copilot/customization/agent-skills)
- GitHub Copilot Agent Skills: [https://docs.github.com/en/copilot/concepts/agents/about-agent-skills](https://docs.github.com/en/copilot/concepts/agents/about-agent-skills)
- GitHub Copilot Custom Skills: [https://docs.github.com/en/copilot/how-tos/copilot-sdk/features/skills](https://docs.github.com/en/copilot/how-tos/copilot-sdk/features/skills)

---



# Abschluss

Der wichtigste Erfolgsfaktor ist nicht die Anzahl der Agenten. Entscheidend ist,
dass jeder Agent:

- eine begrenzte Rolle besitzt,
- auf kontrollierten Kontext zugreift,
- ein definiertes Artefakt erzeugt,
- seine Annahmen sichtbar macht,
- eine überprüfbare Übergabe erstellt,
- und einen Quality Gate nicht selbst umgehen kann.

Baue zuerst einen kleinen Workflow, der zuverlässig funktioniert. Erweitere ihn
erst, wenn Agent Tests, Artifact Reviews und menschliche Freigaben zeigen, dass
die bestehende Kette stabil ist.# TTT426 Agentic Test Engineering Workflow

## Step-by-Step Tutorial für einen BMAD-inspirierten Test-Engineering-Workflow

**Version:** 1.0  
**Zielgruppe:** Test Engineers, Test Manager, Quality Engineers und AI-Workflow-Designer  
**Einsatzgebiet:** TTT426, ToolShop und vergleichbare Software-Testprojekte  
**Zielplattformen:** Cursor, GitHub Copilot, VS Code und andere dateibasierte Agent-Umgebungen

---



## 1. Ziel dieses Tutorials

Dieses Tutorial zeigt, wie du einen agentischen Test-Engineering-Workflow für
TTT426 entwickelst.

Der Workflow soll nicht nur einzelne Prompts ausführen. Er soll:

- spezialisierte Testrollen bereitstellen,
- den Testprozess schrittweise steuern,
- Informationen zwischen Agenten übergeben,
- Testartefakte versioniert erzeugen,
- definierte Qualitätsprüfungen durchführen,
- menschliche Freigaben berücksichtigen,
- Annahmen und offene Fragen transparent dokumentieren,
- und später um zusätzliche Agenten und Tools erweitert werden können.

Das Konzept ist von BMAD inspiriert. BMAD arbeitet mit spezialisierten Agenten,
geführten Workflows und schrittweise aufgebautem Projektkontext. Die Ausgaben
einer Phase bilden dabei den Kontext für die nächste Phase.

Der TTT426 Workflow überträgt dieses Prinzip auf den Softwaretestprozess.

---



## 2. Das gewünschte Zielbild

Der vollständige Workflow soll später ungefähr so aussehen:

```text
Testauftrag
    |
    v
Test Context Analysis
    |
    v
Test Planning
    |
    v
Test Analysis
    |
    v
Test Design
    |
    v
Test Implementation
    |
    v
Test Execution
    |
    v
Defect Analysis
    |
    v
Test Completion
    |
    v
Quality Gate und menschliche Freigabe
```

Jeder Schritt wird von einem spezialisierten Agenten oder einem definierten
Workflow ausgeführt.

Der Workflow wird nicht sofort vollständig implementiert. Du entwickelst zuerst
einen kleinen, überprüfbaren MVP und erweiterst ihn anschließend.

---



# Teil I: Grundlagen und Architektur



## 3. Die fünf Bausteine des Workflows

Der TTT426 Agentic Test Engineering Workflow besteht aus fünf zentralen
Bausteinen.

### 3.1 Agents

Agents repräsentieren spezialisierte Rollen.

Beispiele:

- Test Manager Agent
- Test Analyst Agent
- Test Designer Agent
- Test Reviewer Agent
- Test Automation Architect Agent
- Test Data Agent
- Defect Analyst Agent

Ein Agent definiert vor allem:

- Rolle,
- Ziel,
- Verantwortung,
- Grenzen,
- benötigten Kontext,
- erlaubte Skills,
- erwartete Ergebnisse.



### 3.2 Skills

Skills sind wiederverwendbare fachliche Fähigkeiten.

Beispiele:

- Risk-based Testing
- Equivalence Partitioning
- Boundary Value Analysis
- Decision Table Testing
- State Transition Testing
- Exploratory Testing
- Test Data Design
- Test Automation Design
- Test Summary Reporting

Ein Skill sollte möglichst unabhängig von einem bestimmten Agenten sein.
Mehrere Agenten können denselben Skill verwenden.

### 3.3 Workflows

Ein Workflow definiert eine geordnete Folge von Arbeitsschritten.

Beispiel:

```text
Requirement lesen
    -> Testbasis bewerten
    -> Risiken identifizieren
    -> Testbedingungen ableiten
    -> Ergebnis prüfen
    -> Artefakt speichern
```



### 3.4 Artifacts

Artifacts sind die dokumentierten Ein- und Ausgaben des Workflows.

Beispiele:

- Test Context
- Product Risk Analysis
- Test Plan
- Test Conditions
- Test Cases
- Test Data Requirements
- Test Execution Log
- Defect Report
- Test Summary Report



### 3.5 Quality Gates

Quality Gates prüfen, ob ein Ergebnis vollständig und ausreichend ist, bevor
der nächste Schritt beginnt.

Beispiele:

- Testbasis ist vorhanden.
- Annahmen sind dokumentiert.
- Jedes Produktrisiko besitzt mindestens eine Testbedingung.
- Jeder Testfall besitzt ein erwartetes Ergebnis.
- Kritische Anforderungen sind abgedeckt.
- Menschliche Freigabe wurde erteilt.

---



## 4. Grundprinzipien für TTT426

Lege vor der Implementierung verbindliche Prinzipien fest.

### Prinzip 1: Artifact First

Agenten sollen Ergebnisse nicht nur im Chat ausgeben. Relevante Ergebnisse
werden als Dateien gespeichert.

### Prinzip 2: Context is Versioned

Projektwissen, Entscheidungen und Regeln liegen im Repository und werden über
Git versioniert.

### Prinzip 3: No Silent Assumptions

Ein Agent darf notwendige Annahmen treffen, muss diese aber kennzeichnen und im
Artefakt dokumentieren.

### Prinzip 4: Human Accountability

Ein Agent darf Vorschläge erzeugen und Prüfungen durchführen. Die fachliche
Verantwortung bleibt bei einer benannten Person.

### Prinzip 5: Separation of Duties

Ein erzeugendes Ergebnis wird möglichst von einem anderen Agenten geprüft.

### Prinzip 6: Traceability by Design

Anforderungen, Risiken, Testbedingungen, Testfälle, Ausführungen und Defects
sollen nachvollziehbar miteinander verbunden werden.

### Prinzip 7: Small Context Packages

Ein Agent erhält nur den Kontext, den er für seine Aufgabe benötigt. Das
reduziert Ablenkung und widersprüchliche Anweisungen.

### Prinzip 8: Stop on Critical Missing Information

Wenn eine kritische Information fehlt, markiert der Agent den Workflow als
blockiert. Er erfindet keine Anforderungen.

---



# Teil II: Repository vorbereiten



## 5. Schritt 1: Neues Repository erstellen

Erstelle ein Git-Repository, zum Beispiel:

```text
ttt426-agentic-test-engineering
```

Initialisiere das Repository:

```bash
mkdir ttt426-agentic-test-engineering
cd ttt426-agentic-test-engineering
git init
```

Erstelle eine erste README-Datei:

```bash
touch README.md
```

Inhalt:

```markdown
# TTT426 Agentic Test Engineering

This repository contains the agents, skills, workflows, templates,
quality gates and project artifacts for the TTT426 Agentic Test Engineering
Workflow.
```

Committe den Startpunkt:

```bash
git add .
git commit -m "Initialize TTT426 agentic test engineering repository"
```



### Ergebnis dieses Schrittes

Du besitzt ein leeres, versioniertes Repository.

---



## 6. Schritt 2: Verzeichnisstruktur anlegen

Erstelle folgende Struktur:

```text
ttt426-agentic-test-engineering/
|
|-- AGENTS.md
|-- README.md
|-- CHANGELOG.md
|
|-- agents/
|   |-- test-orchestrator.agent.md
|   |-- test-manager.agent.md
|   |-- test-analyst.agent.md
|   |-- test-designer.agent.md
|   `-- test-reviewer.agent.md
|
|-- skills/
|   |-- analyze-test-basis/
|   |   `-- SKILL.md
|   |-- product-risk-analysis/
|   |   `-- SKILL.md
|   |-- derive-test-conditions/
|   |   `-- SKILL.md
|   |-- design-test-cases/
|   |   `-- SKILL.md
|   `-- review-test-artifact/
|       `-- SKILL.md
|
|-- workflows/
|   |-- 01-test-context-analysis.workflow.md
|   |-- 02-test-planning.workflow.md
|   |-- 03-test-analysis.workflow.md
|   |-- 04-test-design.workflow.md
|   `-- 05-test-review.workflow.md
|
|-- instructions/
|   |-- core-guardrails.md
|   |-- terminology.md
|   |-- artifact-management.md
|   `-- human-approval.md
|
|-- templates/
|   |-- test-context.template.md
|   |-- product-risk-analysis.template.md
|   |-- test-plan.template.md
|   |-- test-conditions.template.md
|   |-- test-cases.template.md
|   |-- artifact-review.template.md
|   `-- handover.template.md
|
|-- quality-gates/
|   |-- qg-01-context-ready.md
|   |-- qg-02-analysis-ready.md
|   |-- qg-03-design-ready.md
|   `-- qg-04-human-approval.md
|
|-- knowledge/
|   |-- ttt426-context.md
|   |-- test-policy.md
|   |-- glossary.md
|   `-- references.md
|
|-- projects/
|   `-- toolshop/
|       |-- input/
|       |-- working/
|       |-- output/
|       |-- decisions/
|       `-- status/
|
|-- schemas/
|   |-- artifact-metadata.schema.yaml
|   `-- workflow-state.schema.yaml
|
|-- scripts/
|   |-- validate_artifact.py
|   `-- create_project.py
|
`-- tests/
    |-- agent-tests/
    |-- skill-tests/
    `-- workflow-tests/
```

Du kannst die Ordner mit folgendem Befehl erzeugen:

```bash
mkdir -p agents instructions templates quality-gates knowledge schemas scripts
mkdir -p workflows tests/agent-tests tests/skill-tests tests/workflow-tests
mkdir -p projects/toolshop/{input,working,output,decisions,status}
mkdir -p skills/{analyze-test-basis,product-risk-analysis,derive-test-conditions,design-test-cases,review-test-artifact}
```



### Warum diese Trennung wichtig ist

- `agents` definiert Rollen.
- `skills` enthält wiederverwendbares Fachwissen.
- `workflows` steuert die Reihenfolge.
- `instructions` enthält globale Regeln.
- `templates` standardisiert Ergebnisse.
- `quality-gates` verhindert unkontrollierte Übergaben.
- `knowledge` liefert gemeinsamen Projektkontext.
- `projects` enthält konkrete Projektdaten.
- `schemas` ermöglicht maschinelle Validierung.
- `tests` prüft die Qualität des Agentensystems.



### Ergebnis dieses Schrittes

Du besitzt das Grundgerüst des Frameworks.

---



# Teil III: Gemeinsame Regeln definieren



## 7. Schritt 3: Zentrale Projektanweisung erstellen

Erstelle die Datei `AGENTS.md`.

```markdown
# TTT426 Agent Instructions

## Mission

Support software testing activities for TTT426 through structured,
traceable and reviewable agentic workflows.

## Mandatory Principles

1. Use repository artifacts as the primary source of truth.
2. Do not invent requirements, policies, test results or approvals.
3. Clearly label assumptions, uncertainties and missing information.
4. Preserve traceability between requirements, risks, test conditions,
   test cases, executions and defects.
5. Follow the TTT426 terminology and templates.
6. Separate artifact creation from artifact review.
7. Do not mark a quality gate as passed without evidence.
8. Human approval is required where the workflow defines it.
9. Never overwrite an approved artifact without creating a new version.
10. Store relevant results in the project artifact folders.

## Artifact Status Values

- draft
- in-review
- changes-requested
- approved
- rejected
- obsolete

## Priority of Instructions

1. Legal, regulatory and security requirements
2. TTT426 test policy
3. Project-specific instructions
4. Workflow instructions
5. Agent instructions
6. Skill instructions
7. User task

## Required Response Behaviour

For every substantial task:

1. Identify the requested workflow step.
2. Identify required inputs.
3. Check whether the inputs exist.
4. Report blocking gaps.
5. Execute the relevant skill.
6. Create or update the defined artifact.
7. Run the required quality gate.
8. Record the handover and next step.
```



### Ergebnis dieses Schrittes

Alle Agenten erhalten eine gemeinsame Basis.

---



## 8. Schritt 4: Guardrails definieren

Erstelle `instructions/core-guardrails.md`.

```markdown
# Core Guardrails

## Requirement Integrity

- Never present an assumption as an approved requirement.
- Never silently resolve contradictory requirements.
- Mark unclear requirements as ambiguous.
- Reference the source of every requirement where possible.

## Test Integrity

- Never claim that a test was executed without execution evidence.
- Never invent passed or failed test results.
- Never close a defect without documented verification.
- Never claim full coverage without a defined coverage model.

## Data Protection

- Do not copy production credentials into test artifacts.
- Do not generate real personal data.
- Identify sensitive data in free-text examples.
- Use synthetic, anonymized or approved pseudonymized test data.

## Agent Boundaries

- Agents may propose decisions.
- Agents may not impersonate a human approver.
- Agents may not change approved scope without escalation.
- Agents may not bypass a failed quality gate.

## Transparency

Every artifact must document:

- source inputs,
- assumptions,
- unresolved questions,
- responsible agent,
- review status,
- creation date,
- version.
```

Erstelle zusätzlich `instructions/human-approval.md`.

```markdown
# Human Approval Rules

Human approval is mandatory for:

- test strategy approval,
- test plan approval,
- acceptance of residual product risk,
- production release recommendation,
- closure of critical defects,
- deviations from the test policy,
- use of production-derived test data.

An AI agent may prepare the approval package but must not approve it.
```



### Ergebnis dieses Schrittes

Die Agenten kennen ihre Grenzen und dürfen keine Freigaben erfinden.

---



# Teil IV: Artifact Model entwickeln



## 9. Schritt 5: Metadatenstandard definieren

Jedes erzeugte Artefakt benötigt einen standardisierten Kopf.

Erstelle `schemas/artifact-metadata.schema.yaml`.

```yaml
artifact_metadata:
  required:
    - artifact_id
    - title
    - artifact_type
    - project
    - version
    - status
    - created_by
    - created_at
    - source_inputs
    - assumptions
    - open_questions
  status_values:
    - draft
    - in-review
    - changes-requested
    - approved
    - rejected
    - obsolete
```

Nutze in Markdown-Artefakten folgenden Header:

```yaml
---
artifact_id: TTT426-TC-001
title: ToolShop Test Context
artifact_type: test-context
project: toolshop
version: 0.1
status: draft
created_by: test-orchestrator
created_at: 2026-07-14
source_inputs:
  - projects/toolshop/input/product-description.md
assumptions: []
open_questions: []
---
```



### Namenskonvention

Verwende beispielsweise:

```text
TTT426-TC-001   Test Context
TTT426-PRA-001  Product Risk Analysis
TTT426-TP-001   Test Plan
TTT426-TCND-001 Test Conditions
TTT426-TCASE-001 Test Case Set
TTT426-REV-001  Artifact Review
TTT426-HO-001   Handover
```



### Ergebnis dieses Schrittes

Artefakte werden eindeutig identifizierbar und versionierbar.

---



## 10. Schritt 6: Erstes Template erstellen

Erstelle `templates/test-context.template.md`.

```markdown
---
artifact_id: <ID>
title: <TITLE>
artifact_type: test-context
project: <PROJECT>
version: 0.1
status: draft
created_by: test-orchestrator
created_at: <DATE>
source_inputs: []
assumptions: []
open_questions: []
---

# Test Context

## 1. Product Under Test

## 2. Business Goal

## 3. Test Assignment

## 4. Scope

### In Scope

### Out of Scope

## 5. Test Basis

| ID | Source | Version | Status | Relevance |
|---|---|---|---|---|

## 6. Stakeholders

| Role | Responsibility | Approval Authority |
|---|---|---|

## 7. Constraints

## 8. Dependencies

## 9. Known Quality Risks

## 10. Missing Information

## 11. Assumptions

## 12. Recommended Next Step
```

Erstelle auf dieselbe Weise Templates für:

- Product Risk Analysis
- Test Plan
- Test Conditions
- Test Cases
- Artifact Review
- Handover



### Ergebnis dieses Schrittes

Der Output der Agenten erhält ein stabiles Format.

---



# Teil V: Den MVP mit fünf Agenten bauen



## 11. Schritt 7: Test Orchestrator Agent erstellen

Der Orchestrator ist kein Fachexperte für alle Testaktivitäten. Er erkennt den
Workflowstatus, wählt den nächsten Agenten und prüft die Übergabe.

Erstelle `agents/test-orchestrator.agent.md`.

```markdown
---
name: test-orchestrator
description: Coordinates the TTT426 test engineering workflow and routes work to specialized agents.
tools:
  - read
  - search
  - edit
  - terminal
---

# Test Orchestrator Agent

## Role

You coordinate the TTT426 Agentic Test Engineering Workflow.

## Goal

Determine the current workflow state, identify the next valid step and prepare
an explicit handover to the responsible specialist agent.

## Responsibilities

- Inspect project status and existing artifacts.
- Identify missing mandatory inputs.
- Select the correct workflow.
- Select the responsible specialist agent.
- Prevent steps from being executed out of sequence.
- Trigger or request the required quality gate.
- Update the workflow status artifact.
- Never perform human approval.

## Inputs

- AGENTS.md
- instructions/
- project status
- available project artifacts
- active user request

## Outputs

- workflow decision
- handover artifact
- updated workflow state
- list of blockers

## Routing Rules

- Missing test context -> test-orchestrator creates context draft.
- Test context ready, planning requested -> test-manager.
- Test basis ready, analysis requested -> test-analyst.
- Approved test conditions available -> test-designer.
- Draft artifact requires independent review -> test-reviewer.
- Failed quality gate -> return to producing agent.

## Restrictions

- Do not invent missing project information.
- Do not approve artifacts.
- Do not execute tests.
- Do not silently skip workflow steps.
```



### Testprompt

```text
Act as the TTT426 Test Orchestrator.
Inspect the ToolShop project folders and determine the next valid workflow step.
Do not create test cases.
```



### Erwartetes Verhalten

Der Agent sollte zunächst den Projektstatus prüfen und nicht sofort Testfälle
erzeugen.

---



## 12. Schritt 8: Test Manager Agent erstellen

Erstelle `agents/test-manager.agent.md`.

```markdown
---
name: test-manager
description: Creates and maintains test planning artifacts based on approved project context and product risks.
---

# Test Manager Agent

## Role

You are the Test Manager for TTT426.

## Goal

Create a realistic, risk-based and traceable test plan.

## Responsibilities

- Define test objectives.
- Define scope and exclusions.
- Select test levels and test types.
- Define the test approach.
- Define entry and exit criteria.
- Define roles and responsibilities.
- Identify test environment and test data needs.
- Define reporting and completion activities.

## Required Inputs

- approved or review-ready test context
- product risk analysis
- applicable test policy
- project constraints

## Output

- projects/<project>/working/test-plan.md

## Mandatory Rules

- Do not create a generic test plan disconnected from product risks.
- Reference relevant product risks.
- Mark unavailable information as TBD.
- Separate test objectives from test activities.
- Do not claim resources or environments are available without evidence.

## Completion Criteria

The test plan is ready for review only when:

- scope is explicit,
- objectives are measurable,
- risks are referenced,
- entry and exit criteria exist,
- responsibilities are assigned or marked TBD,
- assumptions and open questions are documented.
```

---



## 13. Schritt 9: Test Analyst Agent erstellen

Erstelle `agents/test-analyst.agent.md`.

```markdown
---
name: test-analyst
description: Analyzes the test basis, identifies coverage items and derives traceable test conditions.
---

# Test Analyst Agent

## Role

You are a senior Test Analyst working according to the TTT426 test process.

## Goal

Derive relevant, traceable and risk-focused test conditions from the available
test basis.

## Responsibilities

- Analyze requirements and acceptance criteria.
- Detect ambiguity, contradiction and missing information.
- Identify testable features and quality characteristics.
- Derive positive, negative and exception-oriented test conditions.
- Link test conditions to requirements and product risks.
- Recommend suitable test techniques.

## Required Inputs

- test context
- approved scope
- requirements or user stories
- product risk analysis
- terminology and test policy

## Output

- projects/<project>/working/test-conditions.md

## Restrictions

- Do not create detailed test cases unless explicitly requested by the workflow.
- Do not convert assumptions into requirements.
- Do not hide gaps in the test basis.

## Required Test Condition Fields

- Test Condition ID
- Title
- Source Requirement
- Product Risk
- Test Level
- Test Type
- Priority
- Description
- Suggested Test Technique
- Notes and Open Questions
```

---



## 14. Schritt 10: Test Designer Agent erstellen

Erstelle `agents/test-designer.agent.md`.

```markdown
---
name: test-designer
description: Designs concrete test cases from reviewed test conditions using appropriate test techniques.
---

# Test Designer Agent

## Role

You are a senior Test Designer for TTT426.

## Goal

Create concrete, executable and traceable test cases from reviewed test
conditions.

## Responsibilities

- Select suitable test design techniques.
- Define preconditions.
- Define test data requirements.
- Define executable steps.
- Define observable expected results.
- Link every test case to a test condition.
- Identify automation candidates without implementing automation.

## Required Inputs

- reviewed test conditions
- relevant requirements
- test data constraints
- test environment information

## Output

- projects/<project>/working/test-cases.md

## Mandatory Rules

- Every test case must have an expected result.
- Avoid vague wording such as "works correctly".
- Separate preconditions from test steps.
- Use synthetic test data examples.
- Mark unknown values as parameters, not invented facts.
- Do not create automated code unless a separate workflow requests it.

## Required Test Case Fields

- Test Case ID
- Linked Test Condition
- Objective
- Priority
- Preconditions
- Test Data
- Steps
- Expected Result
- Postconditions
- Automation Candidate
```

---



## 15. Schritt 11: Test Reviewer Agent erstellen

Erstelle `agents/test-reviewer.agent.md`.

```markdown
---
name: test-reviewer
description: Independently reviews TTT426 test artifacts against defined criteria and records evidence-based findings.
---

# Test Reviewer Agent

## Role

You are an independent Test Artifact Reviewer.

## Goal

Evaluate whether a test artifact is complete, consistent, traceable and ready
for the next workflow step.

## Responsibilities

- Review the artifact against its template.
- Check relevant quality gates.
- Check internal consistency.
- Check traceability.
- Detect unsupported claims.
- Identify missing information.
- Classify findings by severity.
- Recommend pass, conditional pass or fail.

## Restrictions

- Do not rewrite the complete artifact during the review.
- Do not approve on behalf of a human.
- Do not mark a criterion as passed without evidence.

## Finding Severity

- Critical: workflow must stop
- Major: correction required before handover
- Minor: improvement recommended
- Observation: no immediate correction required

## Output

- projects/<project>/working/reviews/<artifact-id>-review.md
```



### Ergebnis dieses Schrittes

Der MVP besitzt einen Orchestrator sowie vier klar getrennte Testrollen.

---



# Teil VI: Skills entwickeln



## 16. Schritt 12: Skill für Testbasisanalyse erstellen

Erstelle `skills/analyze-test-basis/SKILL.md`.

```markdown
---
name: analyze-test-basis
description: Analyze requirements and other test basis documents for testability, ambiguity, contradiction and missing information.
---

# Analyze Test Basis

## Use This Skill When

- a new requirement or user story enters the workflow,
- test conditions must be derived,
- the quality of the test basis is unknown,
- requirements changed.

## Required Inputs

- requirement or user story
- acceptance criteria
- relevant business rules
- known constraints

## Procedure

1. Identify each explicit requirement statement.
2. Assign or preserve a unique requirement identifier.
3. Identify actors, triggers, conditions, actions and outcomes.
4. Check whether the expected behaviour is observable.
5. Check for ambiguous wording.
6. Check for missing boundaries and error behaviour.
7. Check for contradictions with other sources.
8. Identify assumptions required for testing.
9. Separate facts, assumptions and questions.
10. Produce an analysis table.

## Output Format

| Requirement ID | Finding Type | Finding | Severity | Test Impact | Proposed Clarification |
|---|---|---|---|---|---|

## Quality Criteria

- No invented requirement details.
- Every finding references its source.
- Ambiguities are explained, not only labelled.
- Test impact is stated.
```

---



## 17. Schritt 13: Skill für Produktrisikoanalyse erstellen

Erstelle `skills/product-risk-analysis/SKILL.md`.

```markdown
---
name: product-risk-analysis
description: Identify and assess product quality risks to support risk-based testing decisions.
---

# Product Risk Analysis

## Risk Model

Risk Exposure = Likelihood x Impact

Use a scale from 1 to 5 for likelihood and impact.

## Procedure

1. Identify product features and quality characteristics.
2. Formulate concrete failure events.
3. Describe possible consequences.
4. Estimate likelihood from 1 to 5.
5. Estimate impact from 1 to 5.
6. Calculate the risk exposure.
7. Assign a risk class.
8. Recommend a test response.
9. Record assumptions and evidence.

## Risk Classes

- 16 to 25: Critical
- 10 to 15: High
- 5 to 9: Medium
- 1 to 4: Low

## Output Format

| Risk ID | Failure Event | Consequence | Likelihood | Impact | Exposure | Class | Test Response |
|---|---|---|---:|---:|---:|---|---|

## Guardrails

- Do not confuse project risks with product risks.
- Do not assign scores without explaining the rationale.
- Do not present estimates as measured facts.
```

---



## 18. Schritt 14: Skill für Testbedingungen erstellen

Erstelle `skills/derive-test-conditions/SKILL.md`.

```markdown
---
name: derive-test-conditions
description: Derive traceable and risk-focused test conditions from the test basis and product risks.
---

# Derive Test Conditions

## Procedure

1. Read the reviewed test basis analysis.
2. Read relevant product risks.
3. Identify testable behaviours and quality attributes.
4. Derive positive test conditions.
5. Derive negative test conditions.
6. Derive boundary and exception conditions.
7. Consider relevant non-functional conditions.
8. Assign priority based on risk.
9. Link every condition to its source.
10. Recommend a suitable test technique.

## Rules

- A test condition describes what should be tested, not detailed steps.
- Test conditions must be independently understandable.
- Duplicate conditions should be consolidated.
- Critical risks require explicit coverage.
```

---



## 19. Schritt 15: Skill für Testfalldesign erstellen

Erstelle `skills/design-test-cases/SKILL.md`.

```markdown
---
name: design-test-cases
description: Design concrete test cases using suitable black-box, experience-based and collaboration-based test techniques.
---

# Design Test Cases

## Technique Selection

Use equivalence partitioning when:

- inputs can be divided into groups with equivalent behaviour.

Use boundary value analysis when:

- ordered ranges, limits or thresholds exist.

Use decision table testing when:

- combinations of conditions determine outcomes.

Use state transition testing when:

- behaviour depends on current state and events.

Use exploratory charters when:

- learning, investigation and adaptability are important.

## Procedure

1. Select one reviewed test condition.
2. Identify the most suitable test technique.
3. Define coverage items.
4. Create the smallest meaningful test set.
5. Define preconditions and test data.
6. Define steps and expected results.
7. Link each case to its coverage item.
8. Mark automation suitability.
9. Review for redundant cases.
```

---



## 20. Schritt 16: Review-Skill erstellen

Erstelle `skills/review-test-artifact/SKILL.md`.

```markdown
---
name: review-test-artifact
description: Review a test artifact against its template, quality gate, traceability rules and source inputs.
---

# Review Test Artifact

## Procedure

1. Identify artifact type and version.
2. Load the matching template.
3. Load the applicable quality gate.
4. Check mandatory metadata.
5. Check completeness.
6. Check consistency with source inputs.
7. Check traceability.
8. Check for unsupported statements.
9. Classify findings.
10. Produce a review recommendation.

## Recommendation Values

- PASS
- CONDITIONAL PASS
- FAIL

## Mandatory Evidence

Every passed criterion must reference:

- a section,
- a table row,
- an identifier,
- or another explicit artifact location.
```



### Ergebnis dieses Schrittes

Fachlogik ist nicht mehr fest in einzelne Agenten eingebaut. Sie liegt in
wiederverwendbaren Skills.

---



# Teil VII: Workflows definieren



## 21. Schritt 17: Workflow für Test Context Analysis erstellen

Erstelle `workflows/01-test-context-analysis.workflow.md`.

```markdown
# Workflow 01: Test Context Analysis

## Purpose

Create a shared and reviewable understanding of the test assignment before
planning or test design starts.

## Trigger

- new project,
- new release,
- major scope change,
- missing test context.

## Responsible Agent

- test-orchestrator

## Required Inputs

- product description or user request
- available requirements
- known stakeholders
- known constraints

## Steps

1. Inventory available input documents.
2. Create a source list.
3. Identify product and business goal.
4. Define preliminary scope.
5. Identify stakeholders and approval roles.
6. Identify constraints and dependencies.
7. Record known quality risks.
8. Record missing information.
9. Create the test context artifact.
10. Run Quality Gate QG-01.
11. Create handover to Test Manager or Test Analyst.

## Output

- projects/<project>/working/test-context.md
- projects/<project>/working/handovers/context-handover.md

## Stop Conditions

Stop the workflow when:

- the product under test cannot be identified,
- no test assignment exists,
- scope cannot be distinguished from assumptions,
- required legal or security constraints are unknown.
```

---



## 22. Schritt 18: Workflow für Test Analysis erstellen

Erstelle `workflows/03-test-analysis.workflow.md`.

```markdown
# Workflow 03: Test Analysis

## Purpose

Transform the test basis and product risks into reviewed test conditions.

## Responsible Agent

- test-analyst

## Required Inputs

- test context
- in-scope requirements
- product risk analysis
- applicable policies

## Skills

- analyze-test-basis
- derive-test-conditions

## Steps

1. Validate required inputs.
2. Analyze the test basis.
3. Record ambiguities and contradictions.
4. Derive candidate test conditions.
5. Link conditions to requirements and risks.
6. Assign priority.
7. Recommend test techniques.
8. Save the test conditions artifact.
9. Request independent review.
10. Apply required corrections.
11. Run Quality Gate QG-02.
12. Create handover to Test Designer.

## Output

- test-basis-analysis.md
- test-conditions.md
- test-conditions-review.md
- analysis-to-design-handover.md
```

---



## 23. Schritt 19: Workflow für Test Design erstellen

Erstelle `workflows/04-test-design.workflow.md`.

```markdown
# Workflow 04: Test Design

## Purpose

Create executable, traceable and risk-focused test cases from reviewed test
conditions.

## Responsible Agent

- test-designer

## Required Inputs

- reviewed test conditions
- relevant requirements
- test data constraints
- environment information

## Skills

- design-test-cases

## Steps

1. Validate that test conditions are review-ready or approved.
2. Group conditions by feature, risk and technique.
3. Select appropriate test design techniques.
4. Define coverage items.
5. Create test cases.
6. Define test data requirements.
7. Mark automation candidates.
8. Check traceability.
9. Save the test case artifact.
10. Request independent review.
11. Apply corrections.
12. Run Quality Gate QG-03.
13. Prepare handover to implementation or execution.
```



### Ergebnis dieses Schrittes

Die Reihenfolge der Agentenarbeit ist jetzt explizit beschrieben.

---



# Teil VIII: Handover und Workflow State



## 24. Schritt 20: Handover-Template erstellen

Erstelle `templates/handover.template.md`.

```markdown
---
artifact_id: <ID>
artifact_type: handover
project: <PROJECT>
version: 0.1
status: draft
created_by: <AGENT>
created_at: <DATE>
---

# Handover

## From

## To

## Completed Workflow Step

## Artifacts Delivered

| Artifact ID | File | Version | Status |
|---|---|---|---|

## Quality Gate Result

## Important Decisions

## Assumptions

## Open Questions

## Known Limitations

## Required Next Action

## Human Approval Required

- [ ] Yes
- [ ] No

## Handover Acceptance

- Status: pending
- Accepted by:
- Date:
- Comments:
```



### Warum ein Handover notwendig ist

Ein Dateiname allein erklärt nicht:

- was abgeschlossen wurde,
- welche Annahmen gelten,
- welche Probleme offen sind,
- welche Qualität geprüft wurde,
- und was der nächste Agent tun soll.

Das Handover ist daher ein eigenes Artefakt.

---



## 25. Schritt 21: Workflowstatus definieren

Erstelle `schemas/workflow-state.schema.yaml`.

```yaml
project: string
current_phase: string
current_step: string
status:
  allowed:
    - not-started
    - ready
    - in-progress
    - blocked
    - in-review
    - awaiting-human-approval
    - completed
active_agent: string
required_inputs: array
available_artifacts: array
blockers: array
next_agent: string
next_action: string
last_updated: date-time
```

Erstelle für ToolShop:

`projects/toolshop/status/workflow-state.yaml`

```yaml
project: toolshop
current_phase: context-analysis
current_step: inventory-inputs
status: ready
active_agent: test-orchestrator
required_inputs:
  - product-description
available_artifacts: []
blockers: []
next_agent: test-orchestrator
next_action: create-test-context
last_updated: 2026-07-14T14:00:00+02:00
```



### Ergebnis dieses Schrittes

Der Orchestrator muss den Projektstatus nicht aus Chatverläufen erraten.

---



# Teil IX: Quality Gates entwickeln



## 26. Schritt 22: Quality Gate für Test Context erstellen

Erstelle `quality-gates/qg-01-context-ready.md`.

```markdown
# QG-01: Test Context Ready

## Purpose

Confirm that sufficient context exists before planning or test analysis starts.

## Criteria

- [ ] Product under test is identified.
- [ ] Business goal is documented.
- [ ] Test assignment is documented.
- [ ] In-scope items are listed.
- [ ] Out-of-scope items are listed or explicitly unknown.
- [ ] Test basis sources are listed.
- [ ] Relevant stakeholders are identified.
- [ ] Constraints are documented.
- [ ] Assumptions are explicitly labelled.
- [ ] Open questions are documented.
- [ ] No human approval is falsely claimed.

## Decision Rule

- PASS: all mandatory criteria are satisfied.
- CONDITIONAL PASS: no critical gap exists and remaining gaps are explicitly
  accepted for the next step.
- FAIL: a critical input is missing or scope cannot be determined.
```

---



## 27. Schritt 23: Quality Gate für Test Analysis erstellen

Erstelle `quality-gates/qg-02-analysis-ready.md`.

```markdown
# QG-02: Test Analysis Ready

## Criteria

- [ ] Every test condition has a unique ID.
- [ ] Every test condition references a source.
- [ ] Critical and high product risks have explicit coverage.
- [ ] Positive and negative conditions are considered.
- [ ] Relevant boundaries and exceptions are considered.
- [ ] Priorities are assigned.
- [ ] Suggested techniques are reasonable.
- [ ] Duplicates are removed or justified.
- [ ] Ambiguities are documented.
- [ ] The artifact received an independent review.

## Fail Conditions

The gate fails when:

- critical risks have no test condition,
- conditions are not traceable,
- invented requirements are used,
- or the independent review reports unresolved critical findings.
```

---



## 28. Schritt 24: Quality Gate für Test Design erstellen

Erstelle `quality-gates/qg-03-design-ready.md`.

```markdown
# QG-03: Test Design Ready

## Criteria

- [ ] Every test case has a unique ID.
- [ ] Every test case links to a reviewed test condition.
- [ ] Every test case has an observable expected result.
- [ ] Preconditions are separated from steps.
- [ ] Test data is synthetic, approved or parameterized.
- [ ] Coverage items from the selected technique are represented.
- [ ] Critical tests are clearly prioritized.
- [ ] Redundant test cases are removed or justified.
- [ ] Automation candidates are marked.
- [ ] An independent review has been completed.

## Decision Rule

A PASS means that the test design is ready for implementation or execution.
It does not mean that tests were executed successfully.
```



### Ergebnis dieses Schrittes

Agenten können nicht allein aufgrund einer überzeugend klingenden Antwort zum
nächsten Schritt wechseln.

---



# Teil X: Den ersten End-to-End-MVP ausführen



## DONE - 29. Schritt 25: Ein kleines ToolShop-Beispiel vorbereiten

Erstelle:

`projects/toolshop/input/registration-user-story.md`

```markdown
# User Story: User Registration

As a visitor,
I want to create a customer account,
so that I can order products from the ToolShop.

## Acceptance Criteria

1. The user can register with first name, last name, email and password.
2. The email address must be unique.
3. The password must contain at least eight characters.
4. After successful registration, the user can log in.
5. Invalid input is rejected and an error message is displayed.
```

Erstelle:

`projects/toolshop/input/product-description.md`

```markdown
# ToolShop Product Description

ToolShop is a web application that allows customers to browse tools, create an
account, log in and place orders.

The first TTT426 workflow increment focuses on customer registration.
```

---



## DONE - 30. Schritt 26: Orchestrator starten

Verwende in deiner Agent-Umgebung folgenden Auftrag:

```text
Use the TTT426 Test Orchestrator.

Project: toolshop

Inspect:
- AGENTS.md
- instructions/
- projects/toolshop/input/
- projects/toolshop/status/workflow-state.yaml

Execute only Workflow 01: Test Context Analysis.
Create the required artifacts in the project working folder.
Run QG-01 and record the evidence.
Do not create test conditions or test cases yet.
```



### Manuelle Prüfung

Prüfe anschließend:

- Wurde ein Test Context erzeugt?
- Sind Quellen genannt?
- Sind Annahmen erkennbar?
- Wurde kein Testfall erzeugt?
- Ist QG-01 nachvollziehbar bewertet?
- Existiert ein Handover?

Committe das Ergebnis erst nach deiner Prüfung.

```bash
git add projects/toolshop
git commit -m "Create ToolShop test context through orchestrated workflow"
```

---



## DONE 17.7 / 31. Schritt 27: Test Analysis ausführen

Verwende folgenden Auftrag:

```text
Use the TTT426 Test Analyst.

Project: toolshop

Read the current workflow state, the reviewed test context, the registration user story and applicable instructions.

Execute Workflow 03: Test Analysis.
Use the analyze-test-basis and derive-test-conditions skills.
Create:
- test-basis-analysis.md
- test-conditions.md
- analysis-to-design-handover.md

Do not create detailed test cases.
```

++++++ 20260717 created by 31.Schritt 27: Test Anlyse durchführen:  

`- projects/toolshop/working/test-basis-analysis.md`

`- projects/toolshop/working/test-conditions.md`

`- projects/toolshop/working/handovers/analysis-to-design-handover.md`

- projects/toolshop/status/workflow-state.yaml

Danach startet der Reviewer:

```text
Use the TTT426 Test Reviewer.
Review the ToolShop test conditions against:
- the source user story,
- the test conditions template,
- QG-02,
- the core guardrails.

Record evidence for every passed criterion.
Do not rewrite the test conditions.
```



### Manuelle Prüfung

Kontrolliere insbesondere:

- Deckt der Agent die E-Mail-Eindeutigkeit ab?
- Erkennt er unklare Regeln zur E-Mail-Syntax?
- Erkennt er unklare Passwortregeln?
- Vermeidet er erfundene UI-Details?
- Verknüpft er Bedingungen mit den Acceptance Criteria?

---



## 32. Schritt 28: Test Design ausführen

Nach erfolgreichem Review:

```text
Use the TTT426 Test Designer.

Execute Workflow 04 for the reviewed ToolShop registration test conditions.
Use appropriate test design techniques.
Create concrete manual test cases only.
Do not create Playwright code.
Mark suitable automation candidates.
Run the defined review and quality gate process.
```



### Erwartete Techniken

Für die Beispielstory sind wahrscheinlich geeignet:

- Equivalence Partitioning für gültige und ungültige Eingaben
- Boundary Value Analysis für die Passwortlänge
- Decision Table Testing für Kombinationen von Eingabegültigkeit
- Error Guessing für bereits registrierte E-Mail-Adressen

Der Agent muss seine Technikentscheidung begründen.

TC wurden erstellt: projects/toolshop/working/[test-cases.md](http://test-cases.md)













+++

+++

+++

+++

+++

+++

+++

+++ BIS HIERHER DURCHGEFÜHRT 20260717 22:32

+++

+++

+++

+++

+++















---



# Teil XI: Agentensystem testen



## 33. Schritt 29: Agent Tests definieren

Agenten benötigen Tests wie andere Softwarekomponenten.

Erstelle `tests/agent-tests/test-analyst-tests.md`.

```markdown
# Test Analyst Agent Tests

## AT-TA-001: Missing Requirement

### Input

A user request without an identifiable requirement source.

### Expected Behaviour

- Agent reports the missing source.
- Agent does not invent requirements.
- Workflow is marked blocked or conditional.

## AT-TA-002: Ambiguous Password Rule

### Input

"The password must be secure."

### Expected Behaviour

- Agent identifies "secure" as ambiguous.
- Agent requests measurable criteria.
- Agent may derive a provisional condition labelled as assumption-dependent.

## AT-TA-003: Contradictory Rules

### Input

Source A: Password requires at least 8 characters.
Source B: Password requires at least 12 characters.

### Expected Behaviour

- Agent reports the contradiction.
- Agent does not silently select one value.
- Detailed test design is blocked until clarified or explicitly parameterized.
```

---



## 34. Schritt 30: Skill Tests definieren

Erstelle `tests/skill-tests/design-test-cases-tests.md`.

```markdown
# Design Test Cases Skill Tests

## ST-DTC-001: Boundary Value Analysis

### Input

Password length must be at least 8 characters.

### Expected Coverage

At minimum:

- 7 characters
- 8 characters
- 9 characters

The result must not claim additional maximum length rules.

## ST-DTC-002: Unsupported UI Detail

### Input

A requirement defines an error message but not its visual location.

### Expected Behaviour

The test case checks that an error message is displayed but does not invent
colour, position or component type.
```

---



## 35. Schritt 31: Workflow Tests definieren

Erstelle `tests/workflow-tests/context-to-design-e2e.md`.

```markdown
# E2E Workflow Test: Context to Design

## Preconditions

- ToolShop input files exist.
- Workflow state is set to context-analysis.
- No output artifacts exist.

## Execution

1. Run Test Context Analysis.
2. Review QG-01.
3. Run Test Analysis.
4. Review QG-02.
5. Run Test Design.
6. Review QG-03.

## Expected Results

- Steps execute in the defined order.
- Each step creates a handover.
- No detailed test cases exist before test analysis is complete.
- The reviewer is different from the producing role.
- Failed gates prevent the next workflow transition.
- Every test case links to a test condition.
```



### Ergebnis dieses Schrittes

Du kannst Änderungen an Agenten und Skills regressiv prüfen.

---



# Teil XII: Einfache Automatisierung hinzufügen



## 36. Schritt 32: Artifact Validator bauen

Erstelle `scripts/validate_artifact.py`.

```python
from __future__ import annotations

import sys
from pathlib import Path

import yaml


REQUIRED_FIELDS = {
    "artifact_id",
    "title",
    "artifact_type",
    "project",
    "version",
    "status",
    "created_by",
    "created_at",
    "source_inputs",
    "assumptions",
    "open_questions",
}


def extract_frontmatter(content: str) -> dict:
    if not content.startswith("---\n"):
        raise ValueError("Artifact has no YAML frontmatter.")

    parts = content.split("---", 2)
    if len(parts) < 3:
        raise ValueError("Artifact frontmatter is not closed.")

    metadata = yaml.safe_load(parts[1])
    if not isinstance(metadata, dict):
        raise ValueError("Artifact frontmatter must contain a YAML object.")

    return metadata


def validate(path: Path) -> list[str]:
    errors: list[str] = []

    try:
        content = path.read_text(encoding="utf-8")
        metadata = extract_frontmatter(content)
    except (OSError, ValueError, yaml.YAMLError) as exc:
        return [str(exc)]

    missing = REQUIRED_FIELDS.difference(metadata.keys())
    if missing:
        errors.append(f"Missing metadata fields: {sorted(missing)}")

    if metadata.get("status") not in {
        "draft",
        "in-review",
        "changes-requested",
        "approved",
        "rejected",
        "obsolete",
    }:
        errors.append("Invalid artifact status.")

    return errors


def main() -> int:
    if len(sys.argv) != 2:
        print("Usage: python validate_artifact.py <artifact.md>")
        return 2

    artifact_path = Path(sys.argv[1])
    errors = validate(artifact_path)

    if errors:
        print(f"Validation failed for {artifact_path}:")
        for error in errors:
            print(f"- {error}")
        return 1

    print(f"Validation passed for {artifact_path}")
    return 0


if __name__ == "__main__":
    raise SystemExit(main())
```

Installiere PyYAML:

```bash
python -m pip install pyyaml
```

Führe die Prüfung aus:

```bash
python scripts/validate_artifact.py projects/toolshop/working/test-context.md
```



### Spätere Erweiterungen

Der Validator kann später prüfen:

- eindeutige IDs,
- erlaubte Statusübergänge,
- vorhandene Source Inputs,
- Traceability Links,
- Pflichtabschnitte,
- offene kritische Findings,
- menschliche Freigaben.

---



## 37. Schritt 33: GitHub Action für Artifact Validation hinzufügen

Erstelle `.github/workflows/validate-artifacts.yml`.

```yaml
name: Validate TTT426 Artifacts

on:
  pull_request:
    paths:
      - "projects/**/*.md"
      - "schemas/**"
      - "scripts/validate_artifact.py"

jobs:
  validate:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.12"

      - name: Install dependencies
        run: python -m pip install pyyaml

      - name: Validate Markdown artifacts
        run: |
          find projects -name "*.md" -print0 | while IFS= read -r -d '' file; do
            if head -n 1 "$file" | grep -q '^---$'; then
              python scripts/validate_artifact.py "$file"
            fi
          done
```



### Ergebnis dieses Schrittes

Formale Qualitätsregeln werden nicht nur dem LLM überlassen.

---



# Teil XIII: Human-in-the-Loop integrieren



## 38. Schritt 34: Freigabepunkte festlegen

Definiere zunächst nur wenige, aber wichtige Freigaben.

### Human Gate H1: Test Context Acceptance

Verantwortlich:

- Product Owner oder Auftraggeber
- Test Lead

Prüft:

- Scope
- Testauftrag
- Annahmen
- fehlende Informationen



### Human Gate H2: Test Plan Approval

Verantwortlich:

- Test Lead
- Projektleitung oder Product Owner, abhängig vom Projekt



### Human Gate H3: Test Design Acceptance

Verantwortlich:

- Test Lead oder Senior Test Engineer



### Human Gate H4: Release Recommendation

Verantwortlich:

- benannte menschliche Entscheidungsrolle

Ein Agent darf für jedes Gate ein Entscheidungspaket vorbereiten:

```markdown
# Approval Package

## Decision Requested

## Recommended Decision

## Evidence

## Open Risks

## Deviations

## Consequence of Approval

## Consequence of Rejection
```

Der Agent darf das Feld `approved_by` nicht selbst befüllen.

---



# Teil XIV: TTT426-spezifisches Wissen integrieren



## 39. Schritt 35: TTT426 Context File erstellen

Erstelle `knowledge/ttt426-context.md`.

```markdown
# TTT426 Context

## Purpose

TTT426 is a hands-on software testing project in which participants apply the
complete software test process to a realistic product increment.

## Learning Principles

- Practice before theory-only documentation.
- Participants take responsibility for concrete test roles.
- Test artifacts must be usable in a real project.
- Missing and changing information is part of the learning scenario.
- Results must be reviewable and traceable.

## Product Under Test

The default training product is ToolShop, based on the Practice Software
Testing application.

## Test Management

Testiny may be used for test management and result reporting.

## Test Automation

Playwright is the preferred UI automation technology for the current learning
context.

## Important Boundary

The Agentic Test Engineering Workflow supports the participants. It must not
replace their learning, reflection, review and accountability.
```

---



## 40. Schritt 36: Terminologie definieren

Erstelle `instructions/terminology.md`.

Nutze verbindliche Definitionen für mindestens:

- Test Basis
- Test Object
- Test Condition
- Test Case
- Test Procedure
- Test Level
- Test Type
- Test Approach
- Product Risk
- Project Risk
- Test Coverage
- Entry Criteria
- Exit Criteria
- Test Completion

Beispiel:

```markdown
## Test Condition

A testable aspect of a component or system identified as a basis for testing.

TTT426 usage rule:

A test condition describes what must be tested. It does not contain detailed
execution steps.
```

Die Terminologiedatei verhindert, dass verschiedene Agenten Begriffe
unterschiedlich verwenden.

---



# Teil XV: Erweiterung nach dem MVP



## 41. Schritt 37: Weitere Agenten priorisieren

Füge nicht alle Agenten gleichzeitig hinzu.

Empfohlene Reihenfolge:

### Ausbaustufe 1

- Test Orchestrator
- Test Manager
- Test Analyst
- Test Designer
- Test Reviewer



### Ausbaustufe 2

- Test Data Agent
- Exploratory Testing Agent
- Defect Analyst Agent
- Test Execution Coordinator



### Ausbaustufe 3

- Test Automation Architect Agent
- Playwright Automation Agent
- Automation Reviewer Agent
- CI/CD Integration Agent



### Ausbaustufe 4

- Test Summary Report Agent
- Quality Gate Agent
- Compliance Review Agent
- Learning Coach Agent

Jeder neue Agent benötigt:

1. klaren Zweck,
2. klar definierte Inputs,
3. klar definierte Outputs,
4. mindestens einen Skill,
5. Guardrails,
6. Agent Tests,
7. einen Platz im Workflow,
8. eine definierte Handover-Beziehung.

---



## 42. Schritt 38: Test Data Agent ergänzen

Der Test Data Agent sollte später unter anderem:

- Testdatenanforderungen aus Testfällen ableiten,
- sensible Daten erkennen,
- synthetische Daten vorschlagen,
- Datenabhängigkeiten dokumentieren,
- Testdatenkollisionen zwischen manuellen und automatisierten Tests verhindern,
- API-basierte Datenerzeugung vorbereiten,
- und Data Cleanup berücksichtigen.

Er sollte nicht:

- echte Credentials erzeugen,
- Produktionsdaten ungeprüft kopieren,
- oder irreversible Datenbankänderungen durchführen.

---



## 43. Schritt 39: Test Automation Workflow ergänzen

Der spätere Automation Workflow könnte so aussehen:

```text
Reviewed Manual Test Case
    |
    v
Automation Suitability Analysis
    |
    v
Automation Design
    |
    v
Test Data and Environment Check
    |
    v
Playwright Implementation
    |
    v
Static Review
    |
    v
Execution
    |
    v
Result Import into Testiny
```

Trenne mindestens:

- Automation Architect
- Automation Implementer
- Automation Reviewer

Der implementierende Agent sollte seinen eigenen Code nicht allein freigeben.

---



# Teil XVI: Plattformintegration



## 44. Schritt 40: Nutzung in Cursor

Für Cursor kannst du die gleichen Grundbausteine verwenden:

- Repository Rules oder zentrale Anweisungsdateien für globale Regeln
- Custom Agents oder Subagents für spezialisierte Rollen
- Agent Skills mit `SKILL.md` für wiederverwendbare Fähigkeiten
- MCP für kontrollierten Zugriff auf externe Systeme
- versionierte Markdown-Workflows für die Ablaufsteuerung

Prüfe nach jeder Cursor-Aktualisierung die aktuelle offizielle Dokumentation,
da sich Dateipfade und Funktionsumfang ändern können.

### Empfohlene Zuordnung


| TTT426 Element          | Cursor-Konzept                            |
| ----------------------- | ----------------------------------------- |
| AGENTS.md               | Projektkontext und gemeinsame Anweisungen |
| agents/*.agent.md       | Custom Agents oder Subagents              |
| skills/*/SKILL.md       | Agent Skills                              |
| workflows/*.workflow.md | explizite Workflow-Anweisungen            |
| quality-gates/*.md      | Review Rules und Checklisten              |
| scripts/                | deterministische Validierung              |


---



## 45. Schritt 41: Nutzung mit GitHub Copilot

GitHub Copilot und VS Code unterstützen repository-basierte Custom
Instructions, Custom Agents und Agent Skills. Skills werden als Ordner mit
einer `SKILL.md` und optionalen Ressourcen oder Skripten organisiert.

### Empfohlene Zuordnung


| TTT426 Element        | GitHub Copilot-Konzept                |
| --------------------- | ------------------------------------- |
| globale Regeln        | repository custom instructions        |
| spezialisierte Rollen | custom agents                         |
| Fachmethoden          | agent skills                          |
| technische Checks     | scripts und GitHub Actions            |
| Review des Outputs    | Pull Request Review und Quality Gates |


Halte die Kernlogik möglichst plattformneutral. Plattformabhängige Wrapper
sollten nur auf die gemeinsamen Dateien verweisen.

---



# Teil XVII: Governance und Sicherheit



## 46. Schritt 42: Agent Registry erstellen

Erstelle eine zentrale Übersicht, zum Beispiel `agents/registry.yaml`.

```yaml
agents:
  - id: test-orchestrator
    owner: TTT426 Test Lead
    status: active
    risk_level: medium
    allowed_write_paths:
      - projects/*/working
      - projects/*/status
    human_approval_required: false

  - id: test-manager
    owner: TTT426 Test Lead
    status: active
    risk_level: medium
    allowed_write_paths:
      - projects/*/working
    human_approval_required: true

  - id: test-reviewer
    owner: TTT426 Test Lead
    status: active
    risk_level: low
    allowed_write_paths:
      - projects/*/working/reviews
    human_approval_required: false
```

Dokumentiere pro Agent:

- Owner
- Version
- Modell oder Plattform
- erlaubte Tools
- erlaubte Schreibbereiche
- Datenzugriff
- Risikoeinstufung
- erforderliche Freigaben
- letzte Evaluierung

---



## 47. Schritt 43: Audit Trail definieren

Jeder relevante Agentenlauf sollte mindestens dokumentieren:

- Agentenname und Version
- verwendetes Modell, soweit verfügbar
- Zeitpunkt
- Auftrag
- gelesene Artefakte
- verwendete Skills
- erzeugte oder geänderte Dateien
- Annahmen
- Quality-Gate-Ergebnis
- menschliche Entscheidung

Ein einfaches Logformat:

```yaml
run_id: RUN-2026-07-14-001
agent: test-analyst
agent_version: 0.1
project: toolshop
workflow: 03-test-analysis
inputs:
  - TTT426-TC-001
  - US-REG-001
skills:
  - analyze-test-basis
  - derive-test-conditions
outputs:
  - TTT426-TCND-001
quality_gate: QG-02
result: conditional-pass
human_review: pending
```

---



# Teil XVIII: Erfolg messen



## 48. Schritt 44: Qualitätsmetriken definieren

Bewerte nicht nur Geschwindigkeit.

Geeignete Metriken:

### Artifact Quality

- Anteil der Artefakte, die das Review beim ersten Versuch bestehen
- durchschnittliche Anzahl Major Findings
- Anteil nicht belegter Aussagen
- Anteil fehlender Pflichtfelder



### Traceability

- Anteil der Testbedingungen mit Requirement-Link
- Anteil der Testfälle mit Test-Condition-Link
- Anteil kritischer Risiken mit Testabdeckung



### Workflow Reliability

- Anzahl übersprungener Quality Gates
- Anzahl fehlerhafter Statusübergänge
- Anzahl blockierter Läufe wegen fehlenden Kontexts
- Wiederholbarkeit bei identischem Input



### Human Effort

- Reviewzeit pro Artefakt
- Anzahl notwendiger Korrekturschleifen
- Anteil akzeptierter Agentenvorschläge



### Learning Value

- Können Teilnehmer Agentenergebnisse kritisch erklären?
- Können Teilnehmer Fehler im Agentenoutput erkennen?
- Können Teilnehmer Testtechniken selbst auswählen und begründen?
- Verbessert der Workflow das Verständnis des Testprozesses?

---



# Teil XIX: Empfohlener Implementierungsplan



## 49. Vier Iterationen zum ersten stabilen Workflow



### Iteration 1: Repository und Context Workflow

Implementiere:

- Verzeichnisstruktur
- AGENTS.md
- Guardrails
- Test Orchestrator
- Test Context Template
- Workflow 01
- QG-01
- Workflow State

Definition of Done:

- Der Orchestrator erzeugt reproduzierbar einen Test Context.
- Er erzeugt keine Testfälle.
- Fehlende Inputs werden sichtbar.
- Ein Handover wird erzeugt.



### Iteration 2: Test Analysis

Implementiere:

- Test Analyst
- Analyze Test Basis Skill
- Product Risk Analysis Skill
- Derive Test Conditions Skill
- Test Conditions Template
- Workflow 03
- QG-02
- Reviewer

Definition of Done:

- Anforderungen und Annahmen bleiben getrennt.
- Testbedingungen sind traceable.
- Kritische Risiken besitzen Abdeckung.
- Ein unabhängiger Reviewbericht existiert.



### Iteration 3: Test Design

Implementiere:

- Test Designer
- Design Test Cases Skill
- Test Cases Template
- Workflow 04
- QG-03
- Skill Tests

Definition of Done:

- Testfälle besitzen erwartete Ergebnisse.
- Testtechniken sind erkennbar angewandt.
- Testfälle verweisen auf Testbedingungen.
- Automatisierungskandidaten sind markiert.



### Iteration 4: Automation und Governance

Implementiere:

- Artifact Validator
- GitHub Action
- Agent Registry
- Audit Trail
- E2E Workflow Tests
- Human Approval Package

Definition of Done:

- Formale Artefaktfehler werden automatisch erkannt.
- Pull Requests führen Validierungen aus.
- Agentenbesitz und Zugriffsrechte sind dokumentiert.
- Menschliche Freigaben sind nachvollziehbar.

---



# Teil XX: Häufige Fehler



## 50. Vermeide diese Anti-Patterns



### Ein Agent für alles

Problem:

Der Agent vermischt Planung, Analyse, Design, Ausführung und Review.

Lösung:

Rollen und Übergaben trennen.

### Nur Chat, keine Artefakte

Problem:

Ergebnisse verschwinden im Verlauf und sind nicht versionierbar.

Lösung:

Artifact First anwenden.

### LLM als alleinige Quality Gate

Problem:

Das gleiche Modell erzeugt und genehmigt seine eigene Antwort.

Lösung:

Unabhängiger Review, deterministische Checks und menschliche Freigabe
kombinieren.

### Zu große Context Files

Problem:

Der Agent erhält zu viele irrelevante oder widersprüchliche Informationen.

Lösung:

Aufgabenspezifische Context Packages erstellen.

### Unklare Agentenautonomie

Problem:

Es ist nicht definiert, was ein Agent ändern oder ausführen darf.

Lösung:

Toolrechte, Schreibbereiche und Approval Gates dokumentieren.

### Keine Tests für Prompts und Skills

Problem:

Eine kleine Änderung verursacht unbemerkte Regressionen.

Lösung:

Agent Tests, Skill Tests und Workflow Tests versionieren.

### Übernahme überzeugender, aber unbelegter Aussagen

Problem:

Sprachliche Qualität wird mit fachlicher Richtigkeit verwechselt.

Lösung:

Evidence-based Reviews und Traceability verlangen.

---



# Teil XXI: Deine unmittelbare Start-Checkliste



## 51. Die ersten zehn konkreten Aktionen

1. Repository erstellen.
2. Verzeichnisstruktur anlegen.
3. `AGENTS.md` hinzufügen.
4. Core Guardrails hinzufügen.
5. ToolShop Input Story hinzufügen.
6. Test Context Template erstellen.
7. Test Orchestrator Agent erstellen.
8. Workflow 01 erstellen.
9. QG-01 erstellen.
10. Den ersten Context-Workflow ausführen und manuell reviewen.

Beginne erst danach mit Test Analyst und Test Designer.

---



# Teil XXII: Zielstruktur des ersten MVP

Nach den ersten drei Iterationen sollte dein Repository mindestens folgende
funktionsfähige Bestandteile enthalten:

```text
AGENTS.md
agents/
  test-orchestrator.agent.md
  test-analyst.agent.md
  test-designer.agent.md
  test-reviewer.agent.md
skills/
  analyze-test-basis/SKILL.md
  derive-test-conditions/SKILL.md
  design-test-cases/SKILL.md
  review-test-artifact/SKILL.md
workflows/
  01-test-context-analysis.workflow.md
  03-test-analysis.workflow.md
  04-test-design.workflow.md
templates/
  test-context.template.md
  test-conditions.template.md
  test-cases.template.md
  handover.template.md
quality-gates/
  qg-01-context-ready.md
  qg-02-analysis-ready.md
  qg-03-design-ready.md
projects/toolshop/
  input/
  working/
  output/
  decisions/
  status/
tests/
  agent-tests/
  skill-tests/
  workflow-tests/
```

Das ist noch kein vollautonomes Multi-Agent-System. Es ist aber bereits ein
kontrollierter, dateibasierter und überprüfbarer Agentic Test Engineering
Workflow.

Genau diese Basis ist notwendig, bevor du MCP-Tools, automatische Agentenketten,
Testiny, Playwright, GitHub Issues oder andere externe Systeme integrierst.

---



# Quellen und weiterführende Dokumentation

Die folgenden Quellen wurden für die strukturelle Einordnung verwendet. Prüfe
bei der technischen Umsetzung jeweils die aktuellste Version der offiziellen
Dokumentation.

- BMAD Method Repository: [https://github.com/bmad-code-org/BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD)
- BMAD Getting Started: [https://github.com/bmad-code-org/BMAD-METHOD/blob/main/docs/tutorials/getting-started.md](https://github.com/bmad-code-org/BMAD-METHOD/blob/main/docs/tutorials/getting-started.md)
- BMAD Workflow Map: [https://github.com/bmad-code-org/BMAD-METHOD/blob/main/docs/reference/workflow-map.md](https://github.com/bmad-code-org/BMAD-METHOD/blob/main/docs/reference/workflow-map.md)
- BMAD Documentation: [https://docs.bmad-method.org/](https://docs.bmad-method.org/)
- Cursor Documentation: [https://cursor.com/docs](https://cursor.com/docs)
- Cursor Agent Skills: [https://cursor.com/docs/skills](https://cursor.com/docs/skills)
- VS Code Agent Skills: [https://code.visualstudio.com/docs/copilot/customization/agent-skills](https://code.visualstudio.com/docs/copilot/customization/agent-skills)
- GitHub Copilot Agent Skills: [https://docs.github.com/en/copilot/concepts/agents/about-agent-skills](https://docs.github.com/en/copilot/concepts/agents/about-agent-skills)
- GitHub Copilot Custom Skills: [https://docs.github.com/en/copilot/how-tos/copilot-sdk/features/skills](https://docs.github.com/en/copilot/how-tos/copilot-sdk/features/skills)

---



# Abschluss

Der wichtigste Erfolgsfaktor ist nicht die Anzahl der Agenten. Entscheidend ist,
dass jeder Agent:

- eine begrenzte Rolle besitzt,
- auf kontrollierten Kontext zugreift,
- ein definiertes Artefakt erzeugt,
- seine Annahmen sichtbar macht,
- eine überprüfbare Übergabe erstellt,
- und einen Quality Gate nicht selbst umgehen kann.

Baue zuerst einen kleinen Workflow, der zuverlässig funktioniert. Erweitere ihn
erst, wenn Agent Tests, Artifact Reviews und menschliche Freigaben zeigen, dass
die bestehende Kette stabil ist.