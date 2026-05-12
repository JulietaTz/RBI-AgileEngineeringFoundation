# Prompt: Risk-Based Test Planning With GenAI

Use this prompt for the transfer task:
`2-plan-identify-risks-transfer-task.md`.

The prompt is designed for comparing two different GenAI models. Run it
once with Model A and once with Model B. Copy each result verbatim into
your task outcome before writing your own comparison.

---

## Prompt To Send To The AI

```text
You are a senior test engineer supporting sprint planning for an
e-commerce webshop that is preparing to enter the Czech market and
improve its security foundation.

Your task is to perform risk-based test planning for the eight user
stories below. Use the guiding principle:

"No risk, no test."

Do not invent additional user stories. Do not skip any user story.

## Sprint Goal

The team wants to establish the foundation for entering the Czech
market by enabling Czech customer support, marketplace integration,
Czech language capabilities, localized product offerings, and PayU
payment support.

In parallel, the team wants to improve security by fixing weak password
storage and replacing a vulnerable authentication library.

## User Stories

1. US2300 - Czech Language Support
   As a customer from the Czech Republic, I want to use the webshop in
   Czech so that I can understand products, navigation, and checkout.

2. US2350 - Provide Product Articles in Czech Language
   As a customer from the Czech Republic, I want to read product
   information in Czech so that I can make informed purchasing
   decisions.

3. US2450 - Delivery Costs
   The webshop needs delivery cost rules:
   - free standard shipping for orders over EUR 75, except oversized
     items
   - EUR 5.95 flat rate for orders under EUR 75
   - EUR 20 surcharge for heavy or oversized items
   - EUR 12.95 express option for in-stock items

4. US3100 - Add PayU Payment Support for Czech Customers
   As a Czech customer, I want to pay with PayU so that I can complete
   my purchase securely and conveniently.

5. US3206 - MD5 Hashed Password
   As a shop admin, I want passwords to be stored with MD5 hashing
   instead of plain text.

6. US3409 - Fix Vulnerability in Auth-X32
   As the security officer, I want the vulnerable xauth-X32 library to
   be replaced with xauth-X64 to close vulnerability ISEC-2499.

7. US4700 - Login During Checkout Workflow
   As a user who is not logged in, I want to log in during checkout so
   that I can finish the checkout workflow.

8. US5003 - Replace ORMapper
   As the data engineer, I want ORMapper 1.2 replaced with ORMapper 2.0
   to improve security and performance and fix wrong database field
   mappings.

## Step 1 - Risk Analysis

Create a markdown table with exactly eight rows and these columns:

| User Story | Business Impact | Technical Complexity | Integration Risk | Security Relevance | Regression Potential | Overall Risk |

Use the values Low, Medium, High, or Very High for Overall Risk.
Write concrete risk statements, not generic labels.

## Step 2 - MoSCoW Prioritization

Prioritize the eight user stories using this exact distribution:

- MUST: 2 stories
- SHOULD: 1 story
- COULD: 2 stories
- WON'T: 3 stories

Create a markdown table with exactly eight rows and these columns:

| User Story | MoSCoW Priority | Reasoning |

The reasoning must explain which business, technical, security,
production, or regression risk influenced the priority.

## Step 3 - Testing Focus

For each MUST and SHOULD story only, define:

- the main test objective
- 3 to 5 concrete test ideas
- the recommended test level
  (unit, integration, API, UI, end-to-end, exploratory, or security)
- required test data or environment assumptions

## Step 4 - Critical Reflection

Answer these questions:

1. Which user story was hardest to prioritize, and why?
2. Which risk could be underestimated by this analysis?
3. Which story would you re-check with stakeholders before finalizing
   the test plan?
4. What is the biggest residual risk if only the MUST and SHOULD
   stories receive strong test coverage?

## Step 5 - Final Testing Recommendation

Write a short final recommendation for the test lead.

The recommendation must include:

- which stories need deep testing
- which stories need selective or lightweight testing
- which stories are out of testing scope for this sprint
- one sentence explaining the biggest remaining risk

## Output Rules

- Use markdown only.
- Use the section headings exactly as defined above.
- Keep the answer concise and practical.
- Do not add user stories that are not listed in this prompt.
- Do not change the required MoSCoW distribution.
- Base the answer on risk, not implementation effort alone.
```

---

## How To Use This Prompt

1. Copy the full prompt inside the fenced code block.
2. Paste it into `(RBI)(Chat)_GPT` using Model A.
3. Save the full model response in your outcome file.
4. Run the same prompt again with Model B.
5. Save the second model response in your outcome file.
6. Compare both outputs and write your own final decision.

---

## Expected Model Output

The AI response should contain:

- one risk analysis table for all eight user stories
- one MoSCoW prioritization table with 2 MUST, 1 SHOULD, 2 COULD, and
  3 WON'T stories
- concrete testing focus for MUST and SHOULD stories
- a critical reflection on uncertainty and residual risk
- a final recommendation for the test lead

---

## Acceptance Criteria For Prompt Execution

- [ ] All eight user stories are included.
- [ ] No extra user stories are invented.
- [ ] The MoSCoW distribution is exactly 2 / 1 / 2 / 3.
- [ ] Risk reasoning includes business, technical, security, production,
      or regression impact.
- [ ] MUST and SHOULD stories include concrete test ideas.
- [ ] The final recommendation is usable for sprint test planning.
