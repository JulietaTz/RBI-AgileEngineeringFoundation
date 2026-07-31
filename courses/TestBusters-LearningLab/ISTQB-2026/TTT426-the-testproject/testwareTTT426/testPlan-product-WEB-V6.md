# Product Test Plan for ToolShop WEB – Release 6.0

**Test Plan Level:** Product Level  
**Document Status:** Draft (In Progress)  
**Tribe:** E-Commerce  
**Product:** ToolShop WEB  
**Application Under Test:** Practice Software Testing Web Application

---

# Document Control

| Version | State       | Date       | Description                                                                                                                                                                        | Author       | Reviewed |
| ------- | ----------- | ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | -------- |
| v1.0    | Released    | 2026-06-18 | Document structure updated and prepared for future enhancements.                                                                                                                   | Gülbin Deniz | R. Grötz |
| v1.1    | Released    | 2026-06-24 | Test Planning for the Test Sprint on July 1, 2026 at 42 Vienna.                                                                                                                    | Gülbin Deniz | R. Grötz |
| v1.2    | In Progress | 2026-07-10 | Updated for Release 6.0. Product Test Plan revised to include the Czech Market features and the AI Assistant (F2.1). The project timeline and test scope were updated accordingly. | Gülbin Deniz | R. Grötz |

---

# 1. Introduction

This Product Test Plan describes the test strategy, test scope, and planned test activities for the ToolShop WEB product.

The test plan is derived from the overarching Test Policy and defines how testing activities are implemented at the product level.

The objective of this test plan is to ensure that the application is tested in a systematic, structured, and traceable manner.

This version of the Product Test Plan has been updated for Release 6.0. The project timeline has been revised to reflect the updated milestone schedule, including the postponement of Milestone M3 and the shortened Phase 2. The existing product features remain within the test scope, while Release 6.0 extends the scope with the AI Assistant (F2.1) and additional Czech Market features. Test activities continue to be based on the project's Test Policy and the risk-based testing approach.

## 1.1 Timeline

| Milestone ID | Milestone | Date | Comment |
|--------------|-----------|------------|---------|
| M1 | Project Kick-off | 2026-06-19 | Shared understanding of objectives, roles, and scope |
| M2 | Product Test Plan Approved | 2026-06-26 | Overall test direction agreed |
| M3 | AI Assistant Ready for Investor Presentation | 2026-08-15 | AI Assistant (F2.1) available for investor presentation |
| M4 | Phase 1 Readiness Review | 2026-09-01 | Czech market readiness assessed according to the updated project schedule |
| M5 | Phase 2 Readiness Review | 2026-10-04 | Phase 2 readiness assessed following the shortened project schedule |
| M6 | Phase 3 Readiness Review | 2026-11-16 | US West Coast readiness assessed |
| M7 | Phase 4 Readiness Review | 2026-12-27 | Japan readiness assessed |
| M8 | Final Project Review | TBD | Overall quality status and recommendation delivered |
| M9 | Investor Presentation | 2027-02-02 | Final project presentation to investors |

---

# 2. Test Objectives

The main objectives of this test plan are:

- Validation of the functional requirements of the web application.
- Validation of the AI Assistant (F2.1) and the Czech Market features.
- Ensuring a stable and error-free user experience.
- Early detection of critical defects.
- Reduction of risks in key business processes and features.

---

# 3. Test Scope

## 3.1 In Scope

### Feature Classification Table

| Feature                 | Description                                                                                                                                          | Business Critical (YES/NO) | Tested (MUST/SHOULD/WON'T) |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------- | -------------------------- |
| Admin Account           | CRUD all entities, Reporting                                                                                                                         | NO                         | MUST                       |
| AI Assistant            | Chat interface, AI-powered recommendations, recommendation history, response time                                                              | YES                        | MUST                       |
| Authentication          | Login, Register, Forgot Password, 2FA, Account Lock, Register with Google | YES                        | MUST                       |
| Chat Widget             | Customer support chat functionality                                                                                                                  | NO                         | SHOULD                     |
| Checkout Flow           | Increase/Decrease Quantity, Delete Item, Address Details, Payment Options (Advanced), PayU Payment, Delivery Costs                                   | YES                        | MUST                       |
| Compliance Readiness    | Compliance requirements for the Czech market                                                                                                         | YES                        | MUST                       |
| Contact Form (Advanced) | Advanced Contact Form, File Upload                                                                                                                   | NO                         | SHOULD                     |
| Customer Account        | Update Profile, Change Password, Invoices Overview, Invoice Detail, Invoice PDF, Favorites, Contact Messages                                         | YES                        | MUST                       |
| Discount                | Geo-location, Combined Products                                                                                                                      | NO                         | WON'T                      |
| Google Analytics        | Analytics integration                                                                                                                                | NO                         | WON'T                      |
| Multi-Language          | Multi-language support, including Czech language support                                                                                             | YES                        | MUST                       |
| New Logo                | Updated application logo                                                                                                                             | NO                         | SHOULD                     |
| Privacy Policy          | Privacy policy information                                                                                                                           | NO                         | MUST                       |
| Product Category        | Product categorization and navigation                                                                                                                | YES                        | MUST                       |
| Product Comparison      | Side-by-side specifications comparison, highlight differences only                                                                                   | NO                         | SHOULD                     |
| Product Detail          | Product details, Product specifications                                                                                                              | YES                        | MUST                       |
| Product Overview        | Product listing, Pagination, Filter, Search, Sorting, Price Range, Czech products                                                                    | YES                        | MUST                       |
| Rentals                 | Removal of the Rentals product group from the web-shop interface                                                                                     | YES                        | MUST                       |
| Version Information     | Display of the correct application version                                                                                                           | NO                         | SHOULD                     |


## 3.2 Out of Scope

The following areas are not part of this test plan:

- Performance testing (e.g. load and stress testing)
- Mobile applications (focus is on the web application only)
- Infrastructure and backend components outside the user interface (UI)
- Third-party integrations (where applicable)

---

# 5. Test Strategy

## 5.1 Test Levels

| Level | Description | Comment |
|---------|-------------|---------|
| Component Testing | Verification of individual software components in isolation. | Performed by developers |
| System Testing | Validation of the complete integrated system against specified requirements. | Performed by testers |
| System Integration Testing | Validation of interactions between integrated system components through end-to-end user scenarios. | Performed by Test Engineer |
| User Acceptance Testing (UAT) | Validation of predefined acceptance criteria for the Czech Market features and the AI Assistant from a user perspective. | Performed by Test Engineer |

## 5.2 Quality Criteria Coverage (ISO 9126)

The following quality characteristics are considered within the testing activities for ToolShop WEB.

| Attribute | Sub-Attribute | Explanation | Validation Needed | Comment |
|-----------|---------------|-------------|-------------------|---------|
| Functionality | Suitability | Can software perform the tasks required? | MUST | |
| | Accuracy | Is the result of the calculations correct? | MUST | |
| | Interoperability | Can the system interact with another system? | MUST | |
| | Compliance | Does the system comply with applicable standards, regulations and business rules? | MUST | PCI-DSS, GDPR |
| Security | | Does the system prevent unauthorized access? | MUST | PCI-DSS |
| Usability | Understandability | Can users understand how the system works? | SHOULD | |
| | Learnability | Can users learn how to use the system efficiently? | SHOULD | |
| | Operability | Can users perform their tasks effectively? | MUST | |
| Reliability | Maturity | Does the system operate consistently without failures? | SHOULD | |
| | Fault Tolerance | Can the system continue to operate when faults occur? | SHOULD | |
| | Recoverability | Can the system recover after a failure? | SHOULD | |

The selected quality characteristics are aligned with ISO 9126 and support the evaluation of the overall product quality.

## 5.3 Test Approach

| Test Approach | Description | Comment |
|---------------|-------------|---------|
| Risk-based testing | Testing activities are prioritized based on identified business and technical risks. | Focus on business-critical functionality |
| Requirements-based testing | Tests are derived from requirements, user stories and acceptance criteria. | Ensures traceability between requirements and tests |
| Use case-based testing | The focus is on validating user interactions and business-critical end-to-end processes of the web application. | Focus on end-to-end business workflows |
| User Acceptance Testing (UAT) | Validation of predefined acceptance criteria for the Czech Market features and the AI Assistant from a user perspective. | Executed by Test Engineer |
| Exploratory Testing | Testing is performed without predefined test cases while simultaneously learning and exploring the application. | Useful for discovering unexpected defects and usability issues |
| Regression Testing | Regression testing is performed after changes or bug fixes to ensure that existing functionality remains stable while introducing the Release 6.0 features. | Executed after bug fixes and releases |

## 5.4 Test Design Techniques

The following test design techniques are applied:

| Test Design Technique | Description | Minimum Coverage |
|------------------------|-------------|------------------|
| Equivalence Partitioning | Input data is divided into valid and invalid equivalence classes to reduce the number of test cases. | All input fields |
| Boundary Value Analysis | Tests values at the boundaries of valid and invalid input ranges. | Numeric and range-based input fields |
| Decision Table Testing | Tests combinations of conditions and business rules. | Business rules with multiple conditions |
| State Transition Testing | Verifies valid and invalid transitions between system states. | Features with defined state changes |
| Scenario-based Testing | Validates complete end-to-end user scenarios. | Business-critical workflows |
| Exploratory Testing | Simultaneous learning, test design and execution to discover unexpected defects. | Complex or high-risk areas |
| Use Case-based Testing (T1–T5) | Test scenarios are derived from user interactions using the T1–T5 model. | Focus on end-to-end business workflows |

The T1–T5 model is used to classify test scenarios for use case-based testing.

| Category | Description | Minimum Coverage |
|-----------|-------------|------------------|
| T1 | Happy Path (standard case) | All MUST features |
| T2 | Alternative Scenarios | Where alternative flows exist |
| T3 | Exception Cases | Critical exception scenarios |
| T4 | Negative Testing | Invalid and error scenarios |
| T5 | Misuse Scenarios | Where robustness or misuse risks exist |

## 5.5 Traceability

Test cases are linked to corresponding user stories and acceptance criteria to ensure complete traceability.

## 5.6 Test Reporting and Defect Management

Test progress and results are continuously monitored and tracked using appropriate tools (e.g. GitHub Issues, Pull Requests, and Trello).

Defects are systematically documented, tracked, and prioritized based on their severity and business impact.

This ensures transparency throughout the test process and supports effective decision-making during the testing lifecycle.

Each defect report includes:

- Clear description
- Steps to reproduce
- Expected result
- Actual result
- Severity level

---

# 6. Test Environment

Tests are performed in the following environment:

- **Application:** Practice Software Testing (Web)
- **Browser:** Modern web browsers (e.g. Firefox)
- **Operating System:** Windows-based systems
- **Internet connection:** Required

**URL:** https://holtesting.practicesoftwaretesting.com

---

# 7. Test Data

Typical test data is used to cover different application scenarios:

- User data (valid and invalid)
- Password combinations with varying complexity
- Product-related data
- Shopping cart and checkout data
- AI Assistant input data (e.g. project descriptions, skill levels and budget information)

Test data should be designed to ensure repeatable test execution despite periodic database resets.

---

# 8. Test Automation

Detailed test automation activities are described in the separate **Test Automation Strategy** document.

**Reference**

- [Test Automation Strategy](https://github.com/rgroetz2/TBLL-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/TTT426-the-testproject/testwareTTT426/testAutomationStrategy.md)

---

# 9. Roles and Responsibilities

| Role | Who | Responsibility |
|------|------|----------------|
| Test Project Lead | Gülbin Deniz | Coordination of test planning activities, project communication, maintenance of the Product Test Plan, and support of test design and test execution activities. |
| QA Lead / Test Policy Owner | Julieta Tzouridis | Definition, maintenance and review of the Test Policy |
| Test Automation Lead | Dilek Firat | Definition and maintenance of the Test Automation Strategy and automation-related activities |
| Developer & Test Automation | Rashin Harisi | Definition and maintenance of the Test Automation Strategy and automation-related activities |
| Test Designer / Analyst | Jane Doe | Test analysis, test design and support of test execution activities |
| Product Owner / QE Specialist / Reviewer | Rudolf Grötz | Defines business priorities, reviews test deliverables, makes the final release decision and provides quality assurance guidance |

---

# 10. Entry and Exit Criteria

## 10.1 Entry Criteria

- Relevant requirements and user stories are defined and available.
- Test cases based on the T1–T5 test design approach have been created.
- The test environment is ready and accessible.
- Required test data is available.
- Release 6.0 features, including the AI Assistant (F2.1) and Czech Market functionality, are available in the test environment.

## 10.2 Exit Criteria

- All planned test cases have been executed.
- No critical or blocking defects remain open.
- Test results are fully documented.
- A Test Summary Report has been created.
- Planned regression testing and User Acceptance Testing (UAT) have been completed.

---

# 11. Risks and Assumptions

## Risks

| Risk ID | Risk | Impact | Probability | Mitigation |
|---------|------|--------|-------------|------------|
| R1 | Errors in user registration may prevent access. | High | Medium | Validation and boundary testing |
| R2 | Checkout defects may lead to revenue loss. | High | Medium | End-to-end and regression testing |
| R3 | Invalid validation may cause security issues. | High | Medium | Negative testing |
| R4 | Pricing or tax errors may cause inconsistencies. | High | Medium | Boundary testing |
| R5 | AI Assistant provides inaccurate, incomplete or irrelevant recommendations. | High | Medium | Functional testing, UAT and regression testing |
| R6 | Reduced Phase 2 schedule may limit available testing time. | High | High | Prioritize business-critical features using risk-based testing and focus on MUST requirements first |

## Assumptions

- The test environment is stable and available.
- Requirements and user stories are correctly defined.
- External dependencies (e.g. services or APIs) function as expected.
- The AI Assistant feature is available and accessible in the test environment.

---

# 12. Test Deliverables

| Deliverable | Description | Storage Location |
|-------------|-------------|------------------|
| Product Test Plan | Product-level test planning document | `testwareTTT426/testPlan-product-WEB.md` |
| Test Policy | Project-wide testing principles and governance | `testwareTTT426/testPolicy-ToolShop.md` |
| Test Automation Strategy | Test automation approach and implementation details | `testwareTTT426/testAutomationStrategy.md` |
| Test Design (T1–T5) | Test case design based on the T1–T5 model | `testwareTTT426/testDesign-T1T5.md` |
| Test Design (POISED) | API-oriented test design using POISED | `testwareTTT426/testDesign-POISED.md` |
| Sprint Test Plan | Sprint-level test planning for the AI Assistant feature | `testwareTTT426/testPlan-sprint-AIAssistant.md` |
| Defect Reports | Defect tracking and issue management | GitHub Issues |
| Test Summary Report | Summary of test execution results and quality status | `testwareTTT426/Test-Summary-Report-V5.md` |