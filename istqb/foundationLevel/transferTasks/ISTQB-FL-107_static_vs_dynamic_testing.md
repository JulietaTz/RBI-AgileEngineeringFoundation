# Static vs Dynamic Testing — ISTQB Foundation Level

## Overview

Static Testing and Dynamic Testing are two complementary software testing approaches described in ISTQB Foundation Level Chapter 3.

Both approaches help improve software quality, detect defects, and reduce project risks.

---

# What is Static Testing?

Static Testing evaluates work products without executing the software.

Examples of work products:

* requirements
* user stories
* design documents
* source code
* test cases
* contracts
* project documentation

Static Testing focuses on:

* defect prevention
* early defect detection
* improving quality before execution

---

# Main Characteristics of Static Testing

* No software execution required
* Can start very early in the SDLC
* Finds defects directly
* Supports verification and validation
* Improves communication between stakeholders
* Helps reduce rework and project costs

---

# Static Testing Techniques

Static Testing mainly includes two techniques:

## 1. Reviews

Manual examination of work products by people.

Examples:

* Informal Review
* Walkthrough
* Technical Review
* Inspection

### Purpose of Reviews

* detect defects
* improve quality
* gain shared understanding
* improve communication
* provide feedback early

---

## 2. Static Analysis

Analysis of work products using tools without executing the software.

Examples:

* code analysis
* security scanning
* complexity analysis
* readability checks
* naming convention validation

### Static Analysis Can Detect

* unreachable code
* duplicated code
* undefined variables
* security vulnerabilities
* coding standard violations

---

# What is Dynamic Testing?

Dynamic Testing evaluates software by executing it.

The software must run during testing.

Dynamic Testing focuses on:

* observing system behavior
* validating functionality
* finding failures during execution

---

# Main Characteristics of Dynamic Testing

* Software execution is required
* Starts after executable software exists
* Failures are observed first
* Defects are analyzed afterwards
* Used to validate software behavior
* Measures runtime quality characteristics

---

# Examples of Dynamic Testing

* Functional Testing
* Regression Testing
* Performance Testing
* Usability Testing
* Integration Testing
* Exploratory Testing

---

# Core Difference Between Static and Dynamic Testing

| Static Testing         | Dynamic Testing                 |
| ---------------------- | ------------------------------- |
| No execution required  | Execution required              |
| Finds defects directly | Finds failures during execution |
| Can start early        | Requires executable software    |
| Prevents defects       | Reveals failures                |
| Focus on work products | Focus on running software       |

---

# How Static Testing Works

Typical process:

Requirements / Design / Code
→ Reviews
→ Static Analysis
→ Defects found early

### Benefits

* cheaper fixes
* faster feedback
* improved quality
* lower project risk

---

# How Dynamic Testing Works

Typical process:

Executable Software
→ Execute Tests
→ Failures Observed
→ Analyze Root Cause
→ Defects identified

### Benefits

* validates functionality
* detects runtime issues
* verifies integrations
* measures system behavior

---

# Review Types (ISTQB)

## Informal Review

Least formal review type.

### Goal

Quick feedback and defect detection.

### Characteristics

* no strict process
* no formal documentation required
* flexible and fast

---

## Walkthrough

Review led by the author.

### Goal

* understanding
* collaboration
* knowledge sharing

### Characteristics

* author explains the work product
* reviewers ask questions
* useful for learning

---

## Technical Review

Performed by technically skilled reviewers.

### Goal

* evaluate technical quality
* make technical decisions
* detect defects

### Characteristics

* moderated review
* expert participation
* technical focus

---

## Inspection

Most formal review type.

### Goal

Find the maximum number of defects.

### Characteristics

* strict process
* defined roles
* metrics collected
* formal documentation

---

# Review Roles

| Role          | Responsibility                                  |
| ------------- | ----------------------------------------------- |
| Manager       | Provides resources and decides what is reviewed |
| Author        | Creates and fixes the work product              |
| Moderator     | Facilitates the review process                  |
| Reviewer      | Examines the work product                       |
| Scribe        | Documents findings and decisions                |
| Review Leader | Organizes the review                            |

---

# Benefits of Early Feedback

Early and frequent stakeholder feedback helps:

* prevent misunderstandings
* improve communication
* reduce costly rework
* improve product quality
* align development with business needs

---

# Typical Defects Found More Easily by Static Testing

* ambiguous requirements
* inconsistent requirements
* missing acceptance criteria
* unreachable code
* duplicated code
* poor architecture design
* naming convention violations
* interface mismatches
* security vulnerabilities

---

# Typical Defects Found More Easily by Dynamic Testing

* runtime failures
* incorrect calculations
* integration problems
* performance bottlenecks
* usability issues
* environment configuration issues

---

# When to Use Static Testing

Use Static Testing:

* early in the SDLC
* before coding starts
* during requirements analysis
* during design reviews
* before execution testing
* to improve quality early

---

# When to Use Dynamic Testing

Use Dynamic Testing:

* after executable software exists
* to validate functionality
* to verify integrations
* to test performance
* to test real system behavior

---

# ISTQB Exam Takeaways

## Important Concepts

* Static Testing does not require execution.
* Dynamic Testing requires execution.
* Static Testing finds defects directly.
* Dynamic Testing reveals failures first.
* Reviews and Static Analysis are the main Static Testing techniques.
* Both approaches complement each other.
* Early defect detection reduces project cost.

---

# Key Takeaway

Static Testing helps prevent defects early.
Dynamic Testing helps reveal failures during execution.

Both are essential for effective software quality assurance.

---

