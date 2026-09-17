# Apply Static Testing on Toolshop

## Reference to ISTQB Syllabus chapter

ISTQB FL – 3 Static Testing

## Link to the transfer task file

https://github.com/rgroetz2/TBLL-AgileEngineeringFoundation/blob/b6563e722b4157a267062a2945d991e7b97df138/courses/TestBusters-LearningLab/ISTQB-2026/foundationLevel/transferTasks/Chapter%203/ISTQB-FL-3_Static_Testing_20260829.md

## Outcome

# Static Review – AI Tools Recommendation User Story

## Reviewed Work Product

[US-F2.1-AIToolsRecommendation](https://github.com/rgroetz2/TBLL-AgileEngineeringFoundation/blob/b6563e722b4157a267062a2945d991e7b97df138/courses/TestBusters-LearningLab/ISTQB-2026/foundationLevel/transferTasks/Chapter%203/ISTQB-FL-3-User-Story-AIToolsRecommendation.md)

The user story and its related functional requirements, non-functional
requirements, acceptance criteria, technical notes, recommendations,
and assumptions were reviewed without executing the application.

The review focused on clarity, consistency, completeness, and testability.


## Review Log
| ID | Finding | Severity | Impact | Follow-up Action |
|---|---|---|---|---|
| ST-01 | The term **"optimal solution"** in the user story is not defined. | Medium | Developers do not have clear criteria for determining what makes a recommendation optimal, and testers cannot objectively verify whether the result is optimal. | Define measurable criteria for an optimal recommendation, such as project suitability, budget, skill level, or other ranking criteria. |
| ST-02 | The supported scope of the **AI Tool Advisor** and its distinction from the existing customer-support chat are not clearly defined. | Medium | It is unclear which prompts the Advisor should handle, how out-of-scope requests should be treated, and how users can distinguish the purpose of the Advisor from customer support. This also makes negative testing unclear. | Define the supported prompt scope, expected behavior for out-of-scope requests, and how the AI Tool Advisor is distinguished from the customer-support functionality. |
| ST-03 | The condition **"if needed"** for asking clarifying questions is ambiguous. | Medium | There are no defined criteria for deciding when the AI should ask a clarifying question. Developers may implement different behavior, and testers cannot objectively determine when a clarifying question is expected. | Define the minimum information required from a project description and specify the conditions that trigger clarifying questions. |
| ST-04 | The term **"similar message"** used for reusing cached AI responses is not defined. | High | Similar messages may contain important differences, such as budget or skill level, that require different recommendations. The current requirement does not provide testable rules for cache reuse or invalidation. | Define measurable criteria for message similarity and specify which changes to the user's input must invalidate the cached response. |
| ST-05 | The **5-second response-time requirement** is inconsistent with its measurement definition. | High | The requirements state that the system should respond within 5 seconds, while the technical notes exclude network latency and the third-party AI API call. The actual user-perceived response time may therefore exceed 5 seconds, and the pass/fail criterion is unclear. | Define whether the 5-second target applies to end-to-end user-perceived response time or internal server processing, including clear measurement start and end points. |
| ST-06 | No maximum waiting time or expected behavior is specified when the external AI service is slow, unavailable, or does not return a response. | High | It is unclear how long the customer should wait and what the system should do if the external AI provider or network is unavailable. Testers cannot define expected results for these failure scenarios. | Define a maximum waiting time and expected timeout/error behavior, including appropriate user feedback. |
| ST-07 | Recommended tools and prices are not required to match the **Toolshop product catalog**. | High | The AI may recommend products that cannot be purchased from Toolshop or display prices different from the actual catalog prices. This may provide misleading information and makes recommendation accuracy difficult to verify. | Define whether recommendations must be limited to Toolshop catalog products. If prices are displayed, define the authoritative source for current product and price information. |
| ST-08 | Email validation and error handling for the **"save for later"** feature are not specified. | Medium | It is unclear which email addresses are considered valid and how the system should behave when an invalid email is entered or the email cannot be delivered. Negative test scenarios therefore have no clear expected result. | Define email validation rules and expected system behavior and user feedback for invalid addresses and failed email delivery. |
| ST-09 | The behavior and destination of the **"save for later" link** are not defined. | Medium | It is unclear what recommendation information is saved, where the link directs the customer, and what should be displayed when the link is opened. The feature therefore lacks clear expected results for implementation and testing. | Define what recommendation data is saved, where the link leads, and what content and behavior are expected when the customer opens it. |
| ST-10 | The term **"relevant tools"** and the criteria for selecting or ranking recommendations are not defined. | Medium | Tools may be generally related to the project but not necessarily the most useful or suitable recommendations. Developers do not have clear criteria for selecting or ranking the 3–5 tools, and testers cannot objectively determine whether the returned recommendations satisfy the acceptance criterion. | Define measurable relevance and ranking criteria, such as project requirements, required features, budget, skill level, and the priority or weighting of these factors. |
| ST-11 | The term **"key features"** is not defined. | Medium | It is unclear which product features should be considered important and displayed for each recommendation. Developers may select different features, and testers cannot objectively verify whether the required key features are shown. | Define criteria for selecting key features or specify which product attributes should be displayed for each tool category. |
| ST-12 | The retention period for logged AI conversations is not defined. | High | The system is expected to store AI conversations, but there is no defined retention period. This leaves the data-handling behavior incomplete and prevents testers from verifying when stored conversation data should be removed. | Define the retention period and deletion rules for stored AI conversations. |
| ST-13 | The behavior of **"save a set of recommendations"** is not clearly defined. | Medium | It is unclear whether the customer saves all generated recommendations or can select individual recommendations. This creates ambiguity for both implementation and testing. | Define what constitutes a saved set and whether customers can select individual recommendations before saving. |
| ST-14 | The **Out of Scope** section is not defined. | Medium | The boundaries of the feature are unclear, which may lead to different assumptions about what should be implemented and tested. | Explicitly define functionality that is outside the scope of this user story. |
| ST-15 | The acceptance criterion **"Option to save recommendations for later"** does not define successful save behavior. | Medium | A visible save option could satisfy the written acceptance criterion even if saving or later accessing the recommendations does not work correctly. | Add acceptance criteria defining successful saving, access to the saved recommendations, and expected failure behavior. |

## Proposed Review Improvement

Use a standard review checklist for future user-story reviews.

The checklist should help reviewers check requirements for clarity,
completeness, consistency, and testability, including whether acceptance
criteria are measurable and error scenarios are defined.

Using the same checklist for each review can help identify ambiguous or
missing requirements earlier and make the review process more consistent.


### Learning Summary ###

Static testing can identify defects in requirements and other work
products without executing the application. Reviewing the AI Tool
Advisor user story showed that ambiguous terms such as `optimal`,
`relevant`, `similar`, `key`, and `if needed` can make requirements
difficult to implement and objectively test.

The review also showed the importance of checking consistency across
the complete work product rather than reviewing individual requirements
in isolation. Requirements, acceptance criteria, technical notes,
dependencies, and non-functional requirements can contradict each
other or reveal missing behavior.

A structured review before implementation can clarify the test basis,
improve testability, and reduce defects and rework later in the
development lifecycle.