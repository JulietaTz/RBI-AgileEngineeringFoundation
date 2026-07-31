# QualityQuest Test Policy

## Test Policy

* **Tribe:** E-Commerce
* **Product:** ToolShop WEB
* **Application Under Test:** Practice Software Testing Web Application
* **Standard:** ISO/IEC/IEEE 29119

---

## Document Control

| Version | State    | Date       | Description                                     | Author            | Reviewed     |
| ------- | -------- | ---------- | ----------------------------------------------- | ----------------- | ------------ |
| v2.0    | Released | 2026-06-18 | Initial release of the QualityQuest Test Policy | Julieta Tzouridis | Rudolf Grötz |
| v2.1    | Released | 2026-07-15 | Added Link to Test Automation Policy | Dilek Firat | Rudolf Grötz |

---

# 1. Introduction

## 1.1 Purpose

This Test Policy establishes the quality principles, testing objectives, responsibilities, and governance framework for the QualityQuest Test Project.

The purpose of this policy is to ensure a consistent, transparent, and risk-based approach to testing activities across the project. It provides guidance for planning, designing, executing, evaluating, and reporting testing activities while supporting continuous quality improvement.

Testing is considered a shared responsibility across the Agile team and is integrated throughout the entire Software Development Lifecycle (SDLC).

The policy also defines expectations for defect management, quality governance, Generative AI usage, compliance requirements, and release decision-making.

## 1.2 Who Should Read This Document

This document is intended for all stakeholders involved in the development, testing, and delivery of the QualityQuest project.

It should be read by:

* Product Owners
* Developers
* Test Engineers
* Test Automation Engineers
* Scrum Team Members
* Quality Engineering Coaches
* Project Stakeholders
* Business Representatives

Testing is a shared responsibility in an Agile environment. Therefore, all team members involved in quality-related activities should be familiar with this policy.

## 1.3 Project Background

QualityQuest Consulting is a fictive external test consultancy established as part of the TestBusters-LearningLab initiative in collaboration with 42-Vienna and the RBI Agile Engineering Foundation.

The project provides a realistic Agile learning environment where aspiring Test Engineers apply ISTQB principles, Quality Engineering practices, Test Automation concepts, and Generative AI capabilities while working on a real-world software project.

## 1.4 Terms / Glossary

The QualityQuest project follows the terminology defined in the current version of the ISTQB® Glossary of Testing Terms.

All testing-related terms used within this Test Policy and associated testing artifacts shall be interpreted according to the ISTQB Glossary unless otherwise specified.

For terms related to Artificial Intelligence and Generative AI that are not currently covered by the ISTQB Glossary, project-specific definitions may be provided where required.

**ISTQB Glossary:**
https://glossary.istqb.org/

---

# 2. Test Activities

## 2.1 Responsibilities

In the QualityQuest project, testing is a shared responsibility of the whole team. The team works in an agile setup, which means that development and testing are closely connected and happen at the same time.

The delivery team includes developers, QA Engineers, and the Product Owner. All team members contribute to building a working and stable webshop. Testing is not done by one role only. Instead, it is part of the daily work of the team.

QA Engineers are mainly responsible for designing and executing tests and reporting defects. Developers support testing by performing unit tests and fixing defects. The Product Owner ensures that requirements are clear and validates that features meet business expectations.


## 2.1.1 Four-Eyes Principle

To ensure quality and reduce errors, the QualityQuest project follows the four-eyes principle.

This means that a change created by one person must be reviewed or tested by another person. The person who implements a feature should not be the only one who tests it.

Testing on higher levels (system testing or acceptance testing) should be performed by:

* a QA Engineer
* another developer
* or another team member with a fresh perspective

This approach improves defect detection, objectivity, and overall software quality.

By applying this principle, the team ensures that important issues are not missed and that the system behaves correctly.


## 2.2 Test Activities

### 2.2.1 Test Planning and Control

Test planning defines what will be tested, which risks must be considered, and which overall test approach will be used. It is typically done at the beginning of a sprint or when a new feature is introduced.

The test approach is documented in a Test Plan. The Test Plan includes test objectives, scope, test items, test strategy, test types, entry and exit criteria, risks and assumptions, as well as test environment and test data.

Test control ensures that testing activities remain aligned with the plan. It includes monitoring progress, tracking test execution, and adjusting the plan if necessary.

### 2.2.2 Test Analysis

Test analysis is the activity of reviewing requirements and identifying what needs to be tested. It focuses on understanding user stories, acceptance criteria, and system behavior.

During this activity, test conditions are defined. Test conditions describe what should be tested based on requirements, features, and risks.

Test analysis ensures that:

* requirements are clear
* risks are identified early
* important features are prioritized

This activity builds the foundation for effective test design.
### 2.2.3 Test Design

Test Design is the activity of transforming identified test conditions into structured and executable test cases. The objective is to define how the system, feature, or requirement will be tested in a clear, consistent, and efficient manner.

During test design, requirements, user stories, acceptance criteria, risks, and business objectives are analyzed and translated into appropriate test scenarios and test cases. The focus is on achieving sufficient test coverage while maintaining an effective and manageable test suite.

The QualityQuest project applies risk-based and requirements-based test design approaches. Where appropriate, recognized test design techniques may be used to improve coverage and effectiveness.

Examples include:

* Equivalence Partitioning (EP)
* Boundary Value Analysis (BVA)
* Decision Table Testing
* State Transition Testing
* Error Guessing
* Exploratory Testing

For critical business processes and high-risk functionality, additional positive, negative, exception, and misuse scenarios should be considered.

AI-supported test design may be used to assist in generating test ideas, test scenarios, and test cases. However, all AI-generated outputs shall be reviewed and approved by a qualified human before use.

The result of this activity is a structured set of test specifications and test cases that support effective and traceable test execution.

### 2.2.4 Test Implementation

Test Implementation is the activity of preparing and organizing the necessary testware required for test execution.

During this activity, test cases are finalized, test data is prepared, test environments are verified, and test suites are organized to support efficient test execution.

Where applicable, automated test scripts may be developed, reviewed, and integrated into the test automation framework.

AI-supported tools may be used to assist in the preparation of test data, test scripts, and other testing artifacts. All AI-generated outputs shall be reviewed and approved by a qualified human before use.

The objective of Test Implementation is to ensure that all required testing assets are available, traceable, and ready for execution.

### 2.2.5 Test Execution

Test Execution is the activity of running the defined test cases on the system under test. The objective is to verify that the system behaves as expected and fulfills specified requirements, acceptance criteria, and business expectations.

During test execution, actual results are compared with expected results defined in the test cases. Based on this comparison, a test case is assigned an appropriate status, such as passed, failed, blocked, or not executed.

Test results shall be recorded and documented to provide transparency and traceability. Any defects, anomalies, or deviations from expected behavior shall be documented, analyzed, and managed according to the Defect Management process.

Test execution may be performed manually or through automated testing solutions. Automated test execution should be used where it provides value, improves efficiency, or supports continuous feedback.

AI-supported tools may assist in analyzing execution results, identifying patterns, and supporting defect investigation. However, all significant findings and decisions shall be reviewed and validated by a qualified human before use.

### 2.2.6 Test Evaluation

Test Evaluation is the activity of assessing test results and determining whether the defined quality objectives, acceptance criteria, and testing goals have been achieved.

During this activity, test execution results, identified defects, risks, and quality indicators are reviewed and analyzed. The outcome of the evaluation supports decisions regarding defect resolution, risk mitigation, test completion, and release readiness.

When failures, defects, or quality concerns are identified, appropriate corrective actions should be considered. These may include:

* Defect correction
* Requirement clarification or refinement
* Test case updates
* Additional testing activities
* Risk mitigation measures

Test Evaluation provides stakeholders with information about the current quality status of the product and supports informed decision-making throughout the software development lifecycle.

### 2.2.7 Defect Management

Defect Management ensures that identified defects are documented, assessed, prioritized, tracked, and resolved in a transparent and consistent manner throughout the software development lifecycle.

All defects identified during testing shall be recorded and managed using an approved defect tracking system.

Each defect record should include:

* A clear description of the issue
* Steps to reproduce
* Expected result
* Actual result
* Severity and priority
* Relevant supporting information where applicable

Defects shall be analyzed and prioritized based on business impact, technical impact, risk, and severity.

Developers are responsible for defect resolution, while Test Engineers, Product Owners, and other relevant stakeholders support defect analysis, prioritization, verification, and release decisions.

A shared defect management tool shall be used to ensure transparency, traceability, and effective communication across the Agile team.

#### 2.2.7.1 Zero-Bug Policy

The QualityQuest project follows a Zero-Bug Policy to support transparent quality decisions, effective defect management, and continuous quality improvement.

The objective of the Zero-Bug Policy is not to claim that software is defect-free. All identified defects must be documented, analyzed, prioritized, and tracked until they are resolved or formally accepted by the Product Owner.

**Defect Identification**

Defects may be discovered during any testing activity.

Every defect must be documented in the defect tracking system.

**Defect Assessment**

Defects are reviewed by the QA Engineer, Product Owner, and developers.

Severity and priority are assigned based on business impact and technical risk.

**Resolution Rules**

Critical and High severity defects must be resolved before release.

Medium severity defects require review and approval by the Product Owner.

Low severity defects may be deferred if the risk is considered acceptable.

**Retesting**

Resolved defects must be retested to verify the fix.

Regression testing should be performed where appropriate.

**Release Decision**

Releases must not contain known Critical or High severity defects.

All accepted open defects must be recorded in the Defect Report and communicated to relevant stakeholders before release.

The final release decision is made jointly by the Product Owner and the delivery team.

**Continuous Improvement**

Defect trends and recurring issues should be reviewed during retrospectives.

Root causes should be analyzed to prevent similar defects in the future.

**Stakeholders involved in bug handling:**

* Product Owner
* QA Engineer
* Developers
* QE Coach (if applicable)
* Delivery Team

### 2.2.8 Test Reporting

Test Reporting provides an overview of testing progress, execution results, identified risks, and the overall quality status of the product.

A Test Report includes:

* Number of executed tests
* Passed, failed, blocked, and not executed tests
* Identified defects and defect trends
* Open risks and quality concerns
* Test coverage information
* Results from manual and automated testing activities

The purpose of Test Reporting is to provide transparency and support informed decision-making by the Agile team and project stakeholders.

Test reports help stakeholders understand the current quality status of the product and support decisions regarding defect resolution, risk management, and release readiness.

### 2.2.9 Test Environment Management

Testing requires a stable, controlled, and well-defined test environment.

The test environment should reflect the production environment as closely as possible to ensure reliable and meaningful test results.

A test environment includes:

* Hardware and software configurations
* Browsers and supported devices
* Network and system settings
* Test tools and automation frameworks
* Test data and supporting services

The test environment shall be prepared and verified before test execution and maintained throughout the project lifecycle.

Any significant changes to the test environment should be documented and communicated to relevant stakeholders to minimize testing risks and disruptions.

### 2.2.10 Test Data Management

Test data is required to support the execution of test cases and testing activities.

Test data should:

* Represent realistic business scenarios
* Include valid and invalid inputs
* Support boundary, exception, and edge cases
* Be appropriate for the intended testing objectives

Test data shall be managed, maintained, and protected throughout its lifecycle to ensure accuracy, consistency, and availability.

Personal, confidential, or sensitive information shall not be used for testing unless it is properly anonymized, masked, or otherwise protected.

Test data management shall comply with applicable legal, regulatory, security, and privacy requirements, including GDPR. Where applicable, broader requirements related to operational resilience, information security, and data protection, such as DORA principles, should also be considered.

AI-supported tools may be used to assist in generating test data; however, all generated data shall be reviewed to ensure suitability, accuracy, and compliance with applicable requirements.

# 3. Test Deliverables / Test Artifacts

## 3.1 Test Plan

The Test Plan defines the overall test approach for the QualityQuest project.

It describes what will be tested, how testing will be performed, and which risks must be considered.

The Test Plan includes:

* Test objectives
* Definition of Ready & Definition of Done
* Test Strategy
* Test Environment
* Test Data
* Project Team
* Entry- & Exit Criteria
* Risk & Assumptions
* Test Deliverables

## 3.2 Test Specification

The Test Specification describes how the system will be tested in detail.

It includes test scenarios, test conditions, and test cases.

It provides a structured description of:

* what should be tested
* under which conditions
* with which inputs

Test specifications ensure that testing is consistent and repeatable.

## 3.3 Test Case

A test case defines a specific test situation.

It includes inputs, actions, and expected results.

A test case contains:

* preconditions
* test steps
* expected results

Test cases are used during test execution to verify system behavior.

## 3.4 Defect Report

A Defect Report documents a defect found during testing.

It ensures that issues are clearly described and can be fixed by the development team.

Each defect report includes:

* description of the issue
* steps to reproduce
* expected result
* actual result
* severity

Defect reports support defect tracking and resolution.

## 3.5 Test Report

The Test Report summarizes the results of test execution.

It provides an overview of testing progress and system quality.

It includes:

* executed test cases
* passed and failed tests
* identified defects
* overall quality assessment

The Test Report supports decisions such as release approval.

## 3.6 Test Data

Test data is used to execute test cases.

It must be prepared and managed carefully.

Test data should:

* represent realistic scenarios
* include valid and invalid inputs
* support edge cases

Sensitive data must not be used unless it is anonymized or protected.

Test data handling must follow data protection rules (e.g., GDPR).

## 3.7 Risk Register

The Risk Register is used to document, assess, and monitor product and project risks that may impact quality, testing activities, or project objectives.

Risks should be reviewed regularly and updated as new information becomes available.

The Risk Register supports risk-based testing and informed decision-making throughout the project lifecycle.

## 3.8 AI-Generated Test Artifacts

AI-Generated Test Artifacts may include test cases, test data, automation scripts, defect analyses, test reports, and other testing-related outputs created with the support of approved AI tools.

All AI-generated artifacts shall be reviewed and approved by a qualified human before use.

Significant AI-generated artifacts should be documented and remain traceable where appropriate.

# 4. Test Levels

Testing within the QualityQuest project may be performed at different test levels to provide confidence in the quality, functionality, reliability, and overall fitness of the system under test.

The selected test levels depend on project objectives, identified risks, business requirements, and testing scope.

The following test levels may be applied throughout the software development lifecycle:

## 4.1 Component Testing

Component Testing verifies individual software components, modules, or functions in isolation.

The objective is to detect defects as early as possible and ensure that individual components behave according to their specifications.

Component Testing is typically performed by developers and may be supported by automated testing.

## 4.2 Integration Testing

Integration Testing verifies that components, modules, services, and interfaces interact correctly with each other.

The focus is on validating data flow, communication, and integration points between system components.

Integration Testing helps identify defects that may not be visible during Component Testing.

## 4.3 System Testing

System Testing validates the complete integrated system against specified requirements and expected business behavior.

Testing is performed from an end-user perspective and may include functional and non-functional testing activities.

The objective is to ensure that the system works as intended in the target environment.

## 4.4 User Acceptance Testing (UAT)

User Acceptance Testing verifies that the system meets business requirements, user needs, and acceptance criteria.

The objective is to confirm that the solution is suitable for its intended use and ready for release.

User Acceptance Testing is typically performed by business representatives, end users, Product Owners, or other designated stakeholders.

# 5. Generative AI Governance

## 5.1 Purpose

Generative AI can be used to support testing activities while maintaining quality, security, transparency, and human accountability.

## 5.2 Approved AI Usage

Generative AI supports:

* Test planning
* Test design
* Test case generation
* Test data generation
* Test automation development
* Defect analysis
* Test reporting
* Documentation support

## 5.3 Approved AI Tools

The following AI solutions are approved for use within the QualityQuest project:

* ChatGPT (GPT-5.5)
* Microsoft Copilot
* GitHub Copilot
* Gemini

Additional AI solutions require approval by the QA Lead and Product Owner.

## 5.4 Human Oversight

All AI-generated outputs must be reviewed and approved by a qualified human before use.

## 5.5 Data Protection

When using AI tools:

* Personal data shall not be uploaded.
* Confidential information shall not be disclosed.
* Customer data shall be anonymized.
* GDPR requirements shall be respected.

## 5.6 Traceability

Significant AI-generated artifacts and decisions should be documented where appropriate.

# 6. Test Automation

The project adopts a dedicated Test Automation Policy that defines the governance and organizational principles for test automation.

The Test Automation Policy is maintained as a separate document to allow independent evolution while remaining aligned with this Test Policy.

For details, refer to:

- [Test Automation Policy](./testAutomationPolicy.md)

# 7. Compliance and Security

QualityQuest recognizes the importance of compliance, information security, and data protection throughout the testing lifecycle.

Testing activities shall consider applicable legal, regulatory, contractual, and organizational requirements.

## GDPR

Personal data used during testing shall be protected in accordance with the General Data Protection Regulation (GDPR).

Where possible, test data should be anonymized, masked, or generated to avoid the use of real personal information.

## DORA

Where applicable, testing activities should support the principles of the Digital Operational Resilience Act (DORA), including operational resilience, risk management, and testing of critical systems.

## PCI-DSS

Projects involving payment processing should consider the requirements of the Payment Card Industry Data Security Standard (PCI-DSS).

Sensitive payment information shall not be exposed within test environments unless appropriate controls are in place.

## Security Testing

Security testing should be integrated throughout the software development lifecycle.

Security-related testing activities may include:

* Authentication testing
* Authorization testing
* Vulnerability testing
* Input validation testing
* Session management testing
* Security-focused exploratory testing

Identified security defects shall be documented, prioritized, and managed according to the Defect Management process.

# 8. Continuous Improvement

Testing processes, quality metrics, risks, and lessons learned shall be reviewed regularly.

Improvement opportunities identified during retrospectives, reviews, Root Cause Analysis activities, and release reviews should be incorporated into future work.

# 9. Document Approval

This Test Policy shall be reviewed and approved by the designated project stakeholders before becoming effective.

The policy shall be reviewed periodically and updated whenever significant process, governance, compliance, or technology changes occur.
