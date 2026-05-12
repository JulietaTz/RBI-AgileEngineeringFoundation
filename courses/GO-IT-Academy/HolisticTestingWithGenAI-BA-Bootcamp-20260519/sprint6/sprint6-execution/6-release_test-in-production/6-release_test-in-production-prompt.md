# Prompt: T1&T5 Test Design for Production Payment Validation

You are acting as a quality engineer during the release stage of a
webshop deployment.

A new payment gateway was integrated into the webshop.

In non-production environments, only a dummy implementation of the
payment provider was available. Because of this limitation, important
production-specific behavior could not be fully validated before
release.

Your task is to create a structured T1&T5 test set for testing in
production.

The goal is to validate:
- Payment processing
- Checkout integration
- Production-specific behavior
- Stability and reliability
- Security and error handling
- Regression risks for existing checkout functionality

---

# Part 1 - Analyze the Risk

Identify:
- Business-critical workflows
- Integration points
- Production-only risks
- High-risk failure scenarios

---

# Part 2 - Define the Production Test Scope

Determine:
- Which functionality requires production validation
- Which existing functionality requires regression testing
- Which risks should be prioritized

---

# Part 3 - Create T1&T5 Test Set

Create a structured T1&T5 test set for one payment checkout use-case.

The test set should include:
- T1 tests
- T2 tests
- T3 tests
- T4 tests
- T5 tests

---

# Important Instructions

- Create ONLY test titles
- Follow the T1&T5 syntax:
  - `<Tx>_<Feature>_<BusinessRule|Requirement>_<ExpectedResult>`
- Do NOT create:
  - Detailed test steps
  - Expected result descriptions
  - Preconditions
  - Test data details