# ISTQB CT-GenAI v1.0 – Transfer Tasks for All Sections

This catalog provides one practical transfer task for each syllabus subsection (1.1.1 to 5.2.3).

---

## 1.1.1 AI Spectrum: Symbolic AI, Classical ML, Deep Learning and Generative AI
**Transfer task:** Build a comparison matrix with one testing-relevant use case per AI paradigm.
- Map Symbolic AI, Classical ML, Deep Learning, and Generative AI to typical test activities.
- Add one strength, one limitation, and one risk per paradigm.
- Conclude which paradigm best supports requirement-to-test-case generation.
**Deliverable:** 1-page comparison table + 5-bullet conclusion.
**Timebox:** 20 min

## 1.1.2 Foundations of Generative AI and LLMs
**Transfer task:** Explain an LLM response lifecycle for a real testing prompt.
- Use a prompt like: “Generate boundary test ideas for checkout quantity.”
- Describe tokenization, next-token prediction, and why output can vary.
- Add 3 practical implications for testers.
**Deliverable:** Short markdown note with lifecycle diagram (text-based).
**Timebox:** 20 min

## 1.1.3 Foundation, Instruction-Tuned and Reasoning LLMs
**Transfer task:** Evaluate model type suitability for three test tasks.
- Compare expected behavior for: generic summarization, test-case generation, and step-by-step defect analysis.
- Define when to prefer foundation vs instruction-tuned vs reasoning models.
- Propose one governance rule for model selection.
**Deliverable:** Decision matrix + recommendation paragraph.
**Timebox:** 25 min

## 1.1.4 Multimodal LLMs and Vision-Language Models
**Transfer task:** Draft a multimodal test support workflow for UI testing.
- Design a workflow where screenshots + text prompt are used to identify UI anomalies.
- Define 3 quality checks to avoid false positives.
- Add one fallback step when image interpretation is uncertain.
**Deliverable:** Workflow description with 6–8 ordered steps.
**Timebox:** 25 min

## 1.2.1 Key LLM Capabilities for Test Tasks
**Transfer task:** Map LLM capabilities to the testing lifecycle.
- Assign at least 6 capabilities (summarization, extraction, classification, generation, transformation, explanation) to specific test tasks.
- Add expected benefit and risk for each mapping.
- Highlight two high-value quick wins for your team.
**Deliverable:** Capability-to-task matrix.
**Timebox:** 20 min

## 1.2.2 AI Chatbots and LLM-based Test Applications
**Transfer task:** Define chatbot usage patterns for a test team.
- Create 5 recurring chatbot use cases (analysis, design, defect triage, reporting, retrospectives).
- For each use case, define guardrails and review responsibility.
- Add one “do not use chatbot for…” rule.
**Deliverable:** Team usage guideline draft.
**Timebox:** 20 min

## 2.1.1 Structure of Prompts for Generative AI in Testing
**Transfer task:** Rewrite weak prompts into structured prompts.
- Start with 3 vague prompts from your daily work.
- Improve each using role, context, task, constraints, output format, and acceptance criteria.
- Compare output quality before/after.
**Deliverable:** Before/after prompt set with short evaluation.
**Timebox:** 25 min

## 2.1.2 Core Prompting Techniques for Software Testing
**Transfer task:** Apply three prompting techniques to one test problem.
- Use zero-shot, few-shot, and chain-of-thought-style instructions for the same scenario.
- Evaluate which produces the most testable results.
- Document when each technique should be used.
**Deliverable:** Technique comparison table.
**Timebox:** 25 min

## 2.1.3 System Prompts and User Prompts
**Transfer task:** Design a reusable system prompt for your test assistant.
- Create one system prompt defining role, quality gates, and prohibited behavior.
- Pair it with 3 user prompts for analysis, test design, and defect triage.
- Validate that output format stays consistent.
**Deliverable:** Prompt package v1.0.
**Timebox:** 20 min

## 2.2.1 Test Analysis with Generative AI
**Transfer task:** Generate and refine test conditions from requirements.
- Use one feature description from Toolshop docs.
- Ask GenAI for test conditions and classify by risk/priority.
- Remove ambiguous or non-verifiable conditions.
**Deliverable:** Prioritized list of refined test conditions.
**Timebox:** 25 min

## 2.2.2 Test Design and Test Implementation with Generative AI
**Transfer task:** Use the existing transfer task file in this folder and execute it end-to-end.
- Reuse `ISTQB-GenAI-2.2.2_Test_Design_and_Test_Implementation_with_Generative_AI_20260405.md`.
- Ensure at least one prompt refinement cycle is documented.
- Capture pass/fail evidence for executed tests.
**Deliverable:** Completed artifact from the referenced task.
**Timebox:** 30 min

## 2.2.3 Automated Regression Testing with Generative AI
**Transfer task:** Derive a regression subset with AI support.
- Provide recent change notes and ask GenAI for impacted test areas.
- Build a minimal regression suite (smoke + high-risk).
- Justify inclusion/exclusion decisions.
**Deliverable:** Regression scope file with rationale.
**Timebox:** 25 min

## 2.2.4 Test Monitoring and Control with Generative AI
**Transfer task:** Build an AI-assisted test status briefing.
- Feed test execution results to GenAI.
- Request a concise daily status including trends, blockers, and risk forecast.
- Verify every claim against raw data.
**Deliverable:** One daily report template + one example run.
**Timebox:** 20 min

## 2.2.5 Selecting Prompting Techniques for Software Testing
**Transfer task:** Create a prompt-technique selection guide.
- Define selection criteria (task complexity, precision needed, data context, explainability).
- Map at least 5 test tasks to recommended techniques.
- Add anti-patterns and fallback options.
**Deliverable:** 1-page decision guide.
**Timebox:** 20 min

## 2.3.1 Metrics for Evaluating Generative AI Results
**Transfer task:** Define QA metrics for GenAI-generated test artifacts.
- Propose metrics for correctness, completeness, clarity, traceability, and actionability.
- Add scoring scale and acceptance thresholds.
- Demonstrate on one generated artifact.
**Deliverable:** Metric framework + scored example.
**Timebox:** 25 min

## 2.3.2 Techniques for Evaluation and Iterative Prompt Refinement
**Transfer task:** Run a 3-iteration prompt improvement loop.
- Choose one test-generation objective.
- Execute prompt v1, v2, v3 with explicit changes each round.
- Measure improvement using your metrics.
**Deliverable:** Iteration log with improvement summary.
**Timebox:** 30 min

## 3.1.1 Hallucinations, Reasoning Errors and Bias in Generative AI
**Transfer task:** Use the existing transfer task file in this folder and execute it.
- Reuse `ISTQB-GenAI-3.1.1_Hallucinations_Reasoning_Errors_and_Biases_in_Generative_AI_20260405.md`.
- Ensure all flagged issues are evidence-backed.
- Produce corrected test ideas.
**Deliverable:** Completed artifact from the referenced task.
**Timebox:** 30 min

## 3.1.2 Identifying Hallucinations, Reasoning Errors and Bias
**Transfer task:** Create an identification checklist for review sessions.
- Build indicators for hallucination, reasoning gaps, and bias signals.
- Apply checklist to one GenAI output.
- Mark each finding with evidence and severity.
**Deliverable:** Checklist + reviewed example.
**Timebox:** 20 min

## 3.1.3 Mitigation Techniques
**Transfer task:** Design mitigations for low-quality GenAI outputs.
- Define mitigations at prompt level, process level, and governance level.
- Pair each mitigation with verification criteria.
- Test mitigations on one flawed output.
**Deliverable:** Mitigation catalog with proof-of-effect.
**Timebox:** 25 min

## 3.1.4 Handling Non-Deterministic Behavior
**Transfer task:** Define a repeatability strategy for GenAI-assisted testing.
- Run the same prompt 5 times and compare outputs.
- Identify stable and unstable elements.
- Propose controls (templates, constraints, post-processing rules).
**Deliverable:** Variability analysis + control strategy.
**Timebox:** 25 min

## 3.2.1 Data Privacy and Security Risks
**Transfer task:** Perform a privacy/security risk scan for prompt usage.
- Review sample prompts for personal, confidential, or sensitive data exposure.
- Classify risks and propose redaction/masking rules.
- Define “allowed vs prohibited prompt content.”
**Deliverable:** Risk register + usage policy draft.
**Timebox:** 25 min

## 3.2.2 Vulnerabilities in AI-based Testing Tools
**Transfer task:** Analyze threat scenarios for AI test tooling.
- Identify at least 5 vulnerabilities (prompt injection, data leakage, insecure plugins, etc.).
- Assess likelihood and impact.
- Define prevention and detection controls.
**Deliverable:** Threat model table.
**Timebox:** 25 min

## 3.2.3 Mitigation Strategies
**Transfer task:** Build a layered mitigation plan.
- Include technical controls, process controls, and human review controls.
- Assign owner and implementation horizon for each action.
- Define one KPI per mitigation cluster.
**Deliverable:** Actionable mitigation roadmap.
**Timebox:** 25 min

## 3.3.1 Impact on Energy and CO₂ Emissions
**Transfer task:** Propose sustainable GenAI usage practices for testing.
- Identify factors that increase compute cost (prompt length, model size, iteration count).
- Define 5 efficiency rules without lowering quality.
- Estimate expected reduction qualitatively (low/medium/high).
**Deliverable:** Sustainable usage guideline.
**Timebox:** 20 min

## 3.4.1 Relevant Regulations and Frameworks
**Transfer task:** Use the existing transfer task file in this folder and execute it.
- Reuse `ISTQB-GenAI-3.4.1_AI_Regulations_Standards_and_Frameworks_Relevant_to_GenAI_in_Software_Testing_20260405.md`.
- Keep checks evidence-oriented and risk-prioritized.
- Map results to sprint planning.
**Deliverable:** Completed artifact from the referenced task.
**Timebox:** 30 min

## 4.1.1 Key Architecture Components
**Transfer task:** Design a reference architecture for LLM-based test support.
- Include prompt interface, model layer, context source, validation layer, and logging.
- Describe data flow and control points.
- Add 3 architecture risks and mitigations.
**Deliverable:** Architecture sketch (markdown + bullets).
**Timebox:** 30 min

## 4.1.2 Retrieval-Augmented Generation (RAG)
**Transfer task:** Create a mini RAG design for test documentation retrieval.
- Define source corpus, chunking strategy, retrieval criteria, and answer constraints.
- Add citation requirement for every generated claim.
- Describe evaluation criteria for retrieval quality.
**Deliverable:** RAG concept note.
**Timebox:** 30 min

## 4.1.3 Role of AI Agents in Testing
**Transfer task:** Define an agent role model for your QA workflow.
- Propose at least 3 agent roles (analyst, designer, monitor).
- Specify input/output contracts and approval gates.
- Identify one failure mode per role.
**Deliverable:** Agent operating model.
**Timebox:** 25 min

## 4.2.1 Fine-Tuning LLMs for Testing
**Transfer task:** Assess whether fine-tuning is justified for your context.
- Compare base model + prompting vs fine-tuned model.
- Define data requirements, benefits, risks, and governance prerequisites.
- Provide a go/no-go recommendation.
**Deliverable:** Feasibility assessment.
**Timebox:** 25 min

## 4.2.2 LLMOps for Deployment and Management
**Transfer task:** Draft an LLMOps pipeline for QA usage.
- Include model onboarding, evaluation gates, monitoring, rollback, and audit logging.
- Define roles and responsibilities.
- Add 5 operational KPIs.
**Deliverable:** LLMOps process blueprint.
**Timebox:** 30 min

## 5.1.1 Risks of Shadow AI
**Transfer task:** Identify and mitigate Shadow AI in testing teams.
- List realistic Shadow AI scenarios.
- Assess business/testing impact.
- Define preventive policy, enablement, and monitoring actions.
**Deliverable:** Shadow AI control plan.
**Timebox:** 20 min

## 5.1.2 Key Aspects of a GenAI Strategy
**Transfer task:** Build a lightweight GenAI test strategy canvas.
- Cover vision, use cases, governance, risk, skills, tooling, and metrics.
- Define near-term and mid-term objectives.
- Add decision criteria for use-case prioritization.
**Deliverable:** Strategy one-pager.
**Timebox:** 30 min

## 5.1.3 Selection of LLMs/SLMs
**Transfer task:** Create model selection criteria for QA tasks.
- Compare at least 3 models (or model classes) across quality, cost, latency, privacy, and controllability.
- Weight criteria and compute a final ranking.
- State assumptions and constraints.
**Deliverable:** Weighted decision matrix.
**Timebox:** 25 min

## 5.1.4 Phases of Adoption
**Transfer task:** Define a phased rollout roadmap.
- Create phases: pilot, controlled scale, institutionalized usage.
- Define entry/exit criteria and success metrics per phase.
- Add risk controls for each phase.
**Deliverable:** Adoption roadmap.
**Timebox:** 25 min

## 5.2.1 Required Skills and Knowledge
**Transfer task:** Perform a QA GenAI skills-gap analysis.
- Define required skills by role (tester, test lead, automation engineer).
- Assess current vs target proficiency.
- Prioritize upskilling actions.
**Deliverable:** Skills matrix + training priorities.
**Timebox:** 25 min

## 5.2.2 Building GenAI Capabilities
**Transfer task:** Propose a capability-building plan for the next 90 days.
- Include people, process, tooling, governance, and knowledge-sharing.
- Set measurable milestones.
- Add ownership per milestone.
**Deliverable:** 90-day capability plan.
**Timebox:** 25 min

## 5.2.3 Evolving Test Processes
**Transfer task:** Redesign one existing test process with GenAI integration.
- Pick one process (test design, execution, triage, or reporting).
- Mark which steps are AI-assisted and which remain human-controlled.
- Define controls for quality, compliance, and traceability.
**Deliverable:** Updated process flow + RACI-style ownership.
**Timebox:** 30 min
