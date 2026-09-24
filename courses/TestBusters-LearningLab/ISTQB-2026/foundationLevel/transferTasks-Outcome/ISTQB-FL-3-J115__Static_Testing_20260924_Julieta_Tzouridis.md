# Apply Static Testing on Toolshop

## Reference to ISTQB Syllabus Chapter

ISTQB FL – Chapter 3: Static Testing

Relevant learning objectives:

- FL-3.1.1 – Recognize types of work products that can be examined by static testing
- FL-3.1.2 – Explain the value of static testing
- FL-3.1.3 – Compare and contrast static testing and dynamic testing
- FL-3.2.1 – Identify the benefits of early and frequent stakeholder feedback
- FL-3.2.2 – Summarize the activities of the review process

---

## Link to the Transfer Task File

[ISTQB-FL-3 – Apply Static Testing on Toolshop](https://github.com/JulietaTz/RBI-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/foundationLevel/transferTasks/Chapter%203/ISTQB-FL-3_Static_Testing_20260829.md)

---

## Outcome

### Learning Summary

Static testing was applied to a Toolshop user story before executing the application.

The purpose of the review was to examine the work product and identify defects, ambiguities, inconsistencies and missing information as early as possible.

The review focused on the following quality characteristics:

- clarity
- completeness
- correctness
- consistency
- testability

According to ISTQB, static testing can detect defects directly in work products without executing the software.

In this task, several anomalies were identified in the user story that could lead to misunderstandings during development and make later dynamic testing more difficult.

Detecting these issues early can reduce rework and improve the shared understanding between testers, developers and other stakeholders.

---

## Reviewed Work Product

`ISTQB-FL-3-User-Story-AIToolsRecommendation.md`

[User Story – AI Tools Recommendation](https://github.com/JulietaTz/RBI-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/foundationLevel/transferTasks/Chapter%203/ISTQB-FL-3-User-Story-AIToolsRecommendation.md)

The reviewed work product is a user story describing an AI-based assistant for Toolshop.

---

## User Story

### User Story Summary

The user story describes an AI assistant that helps Toolshop customers select suitable tools.

A customer can describe a project or problem and the AI assistant should provide appropriate tool recommendations.

The AI assistant should:

- understand the customer's request
- ask clarifying questions when necessary
- recommend relevant tools
- provide several recommendations
- provide information about the recommended tools
- provide price information
- respond within a defined response time
- allow recommendations to be saved
- use cached responses for similar requests
- log AI conversations

The user story also contains technical requirements concerning:

- integration with an external AI/LLM provider
- response-time measurement
- caching
- product and price information
- saving recommendations
- conversation logging

This user story was reviewed using static testing without executing the Toolshop application.

---

## Review Approach

A structured individual static review was performed.

The work product was manually examined.

A checklist-based approach was used with the following questions:

- Is the requirement clear?
- Is the requirement complete?
- Is the requirement correct?
- Is the requirement consistent?
- Is the requirement testable?
- Are acceptance criteria measurable?
- Are expected results clearly defined?
- Are alternative scenarios considered?
- Are error situations defined?
- Are important non-functional requirements defined?

According to the ISTQB review process, identified issues are initially recorded as anomalies.

These anomalies can later be discussed and analyzed to determine whether they are defects or whether clarification or another corrective action is required.

The Toolshop application itself was not executed.

---

# Review Findings

| ID | Review Finding / Anomaly | Type | Severity | Likely Impact | Follow-up Action |
|---|---|---|---|---|---|
| F01 | The terms **"optimal solution"** and **"relevant tools"** are not clearly defined. | Ambiguity | High | A tester cannot objectively determine whether an AI recommendation is relevant or correct. Developers may also interpret relevance differently. | Define measurable relevance criteria, for example project type, skill level, budget and product category. |
| F02 | The AI should ask clarifying questions **"if needed"**, but it is not defined when clarification is required. | Ambiguity | Medium | It is unclear whether a direct recommendation or a clarifying question is the expected behavior for a particular input. | Define conditions that trigger a clarifying question, for example missing budget, skill level or project type. |
| F03 | The system should respond within 5 seconds, but the technical notes exclude network latency and the external AI API call from the measurement. | Inconsistency | High | The technical measurement could pass even though the customer actually waits longer than 5 seconds. | Define exactly when the response-time measurement starts and ends and whether the complete user-perceived response time is included. |
| F04 | Product prices are based on the AI provider's general knowledge rather than the current Toolshop product catalog. | Inaccuracy / Risk | High | Prices may be outdated or different from the actual Toolshop price. | Retrieve current prices directly from the Toolshop product catalog. |
| F05 | Recommended tools are not required to be products that can actually be purchased in Toolshop. | Inconsistency | High | The AI may recommend products that the customer cannot find or purchase in Toolshop. | Require recommendations to map to valid Toolshop catalog items or clearly define when external products are allowed. |
| F06 | Cached answers can be reused for a **"similar message"**, but the meaning of "similar" is not defined. | Ambiguity | Medium | An incorrect or unrelated cached recommendation may be returned. It is also difficult to create reliable tests for this behavior. | Define how message similarity is determined and when cached answers may be reused. |
| F07 | Recommendations can be saved and a link can be sent to an email address, but authentication and ownership are not clearly defined. | Missing Information / Risk | High | There may be privacy, security and access-control risks. | Define email validation, authentication, consent, access control and link expiration. |
| F08 | AI conversations should be logged, but no retention period is defined. | Omission | High | Conversation data could be stored indefinitely and retention behavior cannot be tested. | Define what data is logged, how long it is retained and when it is deleted. |
| F09 | No expected behavior is defined if the external AI/LLM service is unavailable, slow or returns an error. | Omission | High | Developers and testers do not know how the system should behave in failure situations. | Define timeout handling, retries, error messages and possible fallback behavior. |
| F10 | The requirement expects several recommendations, but it is unclear what should happen if not enough suitable tools are available. | Missing Information | Medium | The AI may return irrelevant recommendations only to reach the required number. | Define expected behavior when fewer suitable tools are available. |
| F11 | Important non-functional requirements such as security, privacy, reliability and accessibility are not sufficiently defined. | Missing Information | Medium | Important quality characteristics may not be implemented or tested. | Add measurable non-functional requirements and acceptance criteria. |
| F12 | The **Out of Scope** section is not clearly defined. | Omission | Low | Developers, testers and stakeholders may have different assumptions about what belongs to the user story. | Explicitly define functionality that is outside the scope of this user story. |

---

## Severity Classification

| Severity | Meaning |
|---|---|
| High | The anomaly may lead to incorrect implementation, significant product risk or prevent reliable testing. |
| Medium | The anomaly may cause misunderstandings or incomplete testing and should be clarified. |
| Low | The anomaly mainly affects documentation or scope clarity and has a smaller direct impact on functionality. |

---

## Review Analysis and Follow-up

The review identified several anomalies that should be discussed with the relevant stakeholders before implementation or dynamic testing continues.

The most important findings concern:

- definition of relevant AI recommendations
- response-time measurement
- current product prices
- mapping recommendations to Toolshop products
- security and privacy of saved recommendations
- retention of AI conversation data
- handling failures of the external AI service

The High severity findings should have priority.

After clarification, the user story and its acceptance criteria should be updated so that the requirements are clear, complete, consistent and testable.

---

## Improvement for Future Reviews

For future reviews, a standardized **review checklist** should be used.

The checklist should include questions such as:

- Is every requirement clear and unambiguous?
- Is every requirement complete?
- Is the requirement internally consistent?
- Is the requirement testable?
- Is there a clear expected result?
- Are acceptance criteria measurable?
- Are alternative scenarios defined?
- Are error scenarios defined?
- Are non-functional requirements considered?
- Are dependencies and assumptions clearly documented?
- Is the scope clearly defined?

Using the same checklist for future reviews makes the review process more systematic and reduces the risk of overlooking ambiguities, inconsistencies and omissions.

---

## Conclusion

This task demonstrates the value of static testing on a non-executable work product.

The Toolshop user story could be reviewed without executing the application.

The static review directly identified anomalies in the requirements, including ambiguities, inconsistencies, inaccuracies and missing information.

This is an important difference between static and dynamic testing.

Static testing can identify defects directly in work products, while dynamic testing executes the software and detects failures from which underlying defects can then be identified.

Clarifying the identified anomalies before implementation and dynamic testing improves the testability and quality of the user story.

It also helps create a shared understanding between stakeholders and can reduce defects and costly rework later in the software development lifecycle.