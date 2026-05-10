# Prompt: Analyze Production Payment Test Logs and Create Test Summary Report

You are acting as a senior quality engineer analyzing testing in
production results for a newly integrated payment gateway.

Attached You will find multiple production test log files containing:
- Executed test cases
- Transaction details
- Return status
- Error messages
- Payment information
- Runtime behavior

Your task is to analyze the log files and create a structured Test
Summary Report.

---

# Analysis Tasks

## Part 1 - Analyze Test Execution Results

Identify:
- Total executed tests
- Passed tests
- Failed tests
- Failure distribution
- Most frequently failing tests
- Most critical failures

Detect:
- Patterns in failures
- Recurring error messages
- Time-based anomalies
- Currency-related issues
- Payment amount-related issues
- Stability problems

---

## Part 2 - Analyze Risks and Findings

Identify:
- Business-critical issues
- Production-only failures
- Integration problems
- Reliability concerns
- Security-relevant observations
- Performance or timeout patterns

Pay special attention to:
- Payment failures
- Checkout inconsistencies
- Transaction handling
- Timeout behavior
- High-risk production defects

---

## Part 3 - Create Test Summary Report

Create a structured report containing:

### Executive Summary
- Overall test result assessment
- Production readiness evaluation
- Main findings

### Test Execution Summary
- Number of executed tests
- Pass/fail statistics
- Failure trends

### Key Findings
- Most critical defects
- Root-cause indicators
- Failure patterns
- Risk assessment

### Recommendations
- Immediate actions
- Risk mitigation proposals
- Monitoring improvements
- Regression testing recommendations
- Production rollout recommendations

### Release Recommendation
Provide one of:
- Go
- Go with Risks
- No-Go

Justify the recommendation based on the findings.

---

# Important Instructions

- Base all conclusions on evidence from the log files
- Identify patterns and trends
- Highlight business impact
- Focus on production risks
- Include concrete recommendations
- Be concise but structured