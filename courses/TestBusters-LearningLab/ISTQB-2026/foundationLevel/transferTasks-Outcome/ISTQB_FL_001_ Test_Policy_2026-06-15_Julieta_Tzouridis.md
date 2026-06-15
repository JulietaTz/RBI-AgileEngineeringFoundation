# Test Policy – ToolShop

**Standard:** ISO/IEC/IEEE 29119  
**Tribe:** E-Commerce

## Ownership

| Responsibility | Person |
|----------------|---------|
| QA Lead / Test Policy Owner | Julieta Tzouridis |
| Test Automation Strategy Owner | Dilek Firat |
| Reviewer / Mentor | Rudolf Grötz |
| Test Lead | Gülbin Deniz |
| Test Designer/Analyst | Ângela Simões Krainer |
|Test Designer/Analyst | Serife Cicek |
| Test Designer/Analyst | Viktoria Zelenak |


## Document History

**Created:** March 2026  
**Last Updated:** 15 June 2026

---

# 1. Introduction

## 1.1 Who Should Read This Document

This document is intended for all stakeholders involved in the development, testing, and delivery of the ToolShop webshop.

It should be read by:

* Delivery Leads and Product Owners, to understand required testing activities and how quality is ensured within the project.
* QA Engineers and Test Engineers, to understand how testing activities are planned, designed, executed, and reported.
* Developers, to understand testing expectations, support unit testing, and collaborate with QA Engineers during defect resolution.
* A Test Manager or responsible QA role (if defined), to coordinate testing activities and ensure consistent application of the test process.

In addition, all internal and external team members involved in testing activities should be familiar with this Test Policy, as testing is a shared responsibility in an agile environment.

This document ensures that all participants have a common understanding of testing processes, responsibilities, and quality objectives within the ToolShop project.

---

## 1.2 Terms / Glossary

To ensure a consistent understanding of testing terminology, this project follows the **ISTQB® Standard Glossary of Terms Used in Software Testing**.

### Software Testing

The process of verifying and validating that the system behaves as expected and identifying defects before the product is released.

### Defect (Bug)

A flaw in the system that may cause the system to behave incorrectly.

### Test Case

A set of conditions, inputs, and expected results used to verify that a system behaves as expected.

### Test Condition

Defines what should be tested based on requirements, features, or risks.

---

# 2. Test Activities

## 2.1 Responsibilities

In the ToolShop project, testing is a shared responsibility of the whole team.

The delivery team includes:

* Developers
* QA Engineers
* Product Owner

### Responsibilities

| Role          | Responsibility                                       |
| ------------- | ---------------------------------------------------- |
| QA Engineer   | Design and execute tests, report defects             |
| Developer     | Perform unit testing, fix defects                    |
| Product Owner | Clarify requirements, validate business expectations |

---

## 2.1.1 Four-Eyes Principle

To ensure quality and reduce errors, the ToolShop project follows the four-eyes principle.

A change created by one person must be reviewed or tested by another person.

Testing on higher levels (system testing or acceptance testing) should be performed by:

* QA Engineer
* Another developer
* Another team member with a fresh perspective

Benefits:

* Improved defect detection
* Increased objectivity
* Higher software quality

---

## 2.2 Test Activities

Testing in the ToolShop project is structured into clearly defined activities.

---

### 2.2.1 Test Planning and Control

Test planning defines:

* What will be tested
* Risks
* Test approach

The Test Plan includes:

* Test objectives
* Scope
* Test items
* Test strategy
* Test types
* Entry and exit criteria
* Risks and assumptions
* Test environment
* Test data

#### Artifacts

* Test Plan → `test-plan-toolshop.md`

---

### 2.2.2 Test Analysis

Test analysis focuses on:

* Reviewing requirements
* Identifying test conditions
* Understanding system behavior

Goals:

* Requirements are clear
* Risks identified early
* Important features prioritized

---

### 2.2.3 Test Design

Test design transforms test conditions into executable test cases.

#### Techniques Used

* Equivalence Partitioning (EP)
* Boundary Value Analysis (BVA)

Optional:

* T1–T5 Model

  * Happy Path
  * Alternative Scenarios
  * Exception Cases
  * Negative Testing
  * Misuse Scenarios

#### Artifact

* Test Cases → `test-case-toolshop.md`

---

### 2.2.4 Test Execution

Test execution verifies whether the system behaves as expected.

Activities:

* Execute test cases
* Compare actual and expected results
* Assign Pass / Fail status
* Record results
* Document defects

Execution can be:

* Manual
* Automated

#### Artifacts

* Test Report → `test-report-toolshop.md`
* Defect Report → `defect-report-toolshop.md`

---

### 2.2.5 Test Evaluation

Test evaluation determines whether acceptance criteria are fulfilled.

Possible corrective actions:

* Fixing code
* Updating test cases
* Improving requirements

---

### 2.2.6 Defect Management

All defects found during testing must be documented and tracked.

Each defect must include:

* Description
* Reproduction steps
* Expected result
* Actual result
* Severity

#### Artifact

* Defect Report → `defect-report-toolshop.md`

---

### 2.2.6.1 Bug Handling Policy (Zero-Bug Approach)

The ToolShop project follows a structured bug handling process to ensure product quality and transparency.

#### Defect Identification

* Defects may be discovered during any testing activity.
* Every defect must be documented in the defect tracking system.

#### Defect Assessment

Defects are reviewed by:

* QA Engineer
* Product Owner
* Developers

Severity and priority are assigned based on:

* Business impact
* Technical risk

#### Resolution Rules

| Severity | Rule                                  |
| -------- | ------------------------------------- |
| Critical | Must be fixed before release          |
| High     | Must be fixed before release          |
| Medium   | Requires Product Owner approval       |
| Low      | May be deferred if risk is acceptable |

#### Retesting

* Resolved defects must be retested.
* Regression testing should be performed where appropriate.

#### Release Decision

* Releases must not contain known Critical or High severity defects.
* Accepted open defects must be documented and communicated.

Final release decision:

* Product Owner
* Delivery Team

#### Continuous Improvement

* Review defect trends during retrospectives.
* Analyze root causes.
* Prevent recurring defects.

#### Stakeholders

* Product Owner
* QA Engineer
* Developers
* QE Coach (optional)
* Delivery Team

---

### 2.2.7 Test Reporting

Test reporting provides visibility into:

* Executed tests
* Passed tests
* Failed tests
* Defects
* Risks

#### Artifact

* Test Summary Report → `test-report-toolshop.md`

---

### 2.2.8 Test Environment Management

The test environment should resemble production as closely as possible.

Includes:

* Browser setup
* System configuration
* Test data

The environment must be prepared before testing and maintained during the project.

---

### 2.2.9 Test Data Management

Test data should:

* Represent realistic scenarios
* Include valid and invalid inputs
* Cover edge cases

Requirements:

* Sensitive data must be anonymized
* GDPR compliance is mandatory
* DORA principles should be considered where applicable

#### Artifact

* Test Data → `test-data-toolshop.md`

---

# 3. Test Deliverables / Test Artifacts

Test artifacts support transparency, communication, and traceability.

---

## 3.1 Test Plan

Defines:

* Test objectives
* Scope
* Test items
* Test strategy
* Entry criteria
* Exit criteria
* Risks and assumptions
* Test environment
* Test data

---

## 3.2 Test Specification

Describes:

* Test scenarios
* Test conditions
* Test cases

Provides:

* What should be tested
* Under which conditions
* With which inputs

---

## 3.3 Test Case

Contains:

* Preconditions
* Test steps
* Expected results

Used to verify system behavior.

---

## 3.4 Defect Report

Contains:

* Description
* Reproduction steps
* Expected result
* Actual result
* Severity

Supports defect tracking and resolution.

---

## 3.5 Test Report

Contains:

* Executed test cases
* Passed tests
* Failed tests
* Identified defects
* Overall quality assessment

Supports release decisions.

---

## 3.6 Test Data

Test data should:

* Represent realistic scenarios
* Include valid and invalid inputs
* Support edge cases

Sensitive data must be anonymized and GDPR compliant.

---

# 4. Test Levels

Testing is performed on multiple levels to ensure quality across the system.

---

## 4.1 Unit Testing

Performed by developers.

Purpose:

* Verify individual functions and components
* Detect defects early

---

## 4.2 Integration Testing

Verifies:

* Interaction between components
* Interfaces
* Data flow

---

## 4.3 System Testing

Validates the complete system.

Focus:

* End-to-end testing
* User perspective

---

## 4.4 Acceptance Testing

Verifies business requirements.

Purpose:

* Determine release readiness
* Validate business expectations

Typically performed with involvement from the Product Owner.
