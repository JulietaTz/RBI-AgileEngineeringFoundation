# **Title**

Hands-on: T1–T5 test design for production-oriented payment validation on ToolShop (you write the prompts)


# **Background**

The **ToolShop** is the system under test. Imagine a **new payment integration** (aligned with the course user story on supporting an additional payment type) that could only be exercised with a **dummy gateway** in lower environments. Important **production-only** risks therefore remain: real issuer behaviour, callbacks, timeouts, reconciliation, security controls, and customer-visible failure modes at checkout. Your job is to practise **risk-oriented T1–T5 test design** for **safe production validation** and **post-deploy monitoring awareness**, not to copy a ready-made model instruction. This handout is intentionally **not** a 1:1 prompt: you define goals, constraints, output shape, and follow-up questions yourself.

# **Reference**


- User story (payment type and integration context): `sprint6/sprint6-input/US3100-support-payment-type-PayU.md`
- T1T5 technique description: `sprint6-execution/3-understand_use-atdd_designT1T5-DONE/T1T5-testdesign-description.md`

# **Tasks**

1. Read the user story and T1T5 material; list **risks** and **production-specific unknowns** that dummy environments could not settle (payment processing, checkout integration, error handling, security, reliability, observable production behaviour).

2. Author **prompt A** for one model/provider whose purpose is to help **you** shape a **structured T1–T5 test set** for ToolShop’s payment and checkout flows under production constraints—**you** choose how much story text to include, what “safe validation” means in your wording, and what table or checklist structure you want back.

3. OPTIONAL: Author **prompt B** using a **different** model, provider, or instruction style; do not reuse one long prompt verbatim—adapt so you learn what changes coverage, tone, and feasibility of production-safe checks.

4. Ensure your consolidated design explicitly considers **acceptance** of the new integration, **regression** of impacted checkout paths, and **post-deployment validation**—still framed as **test ideas** and **controls**, not unapproved live experiments on real money flows unless your facilitator defines an allowed sandbox.

5. Build **your** table with columns: **T1T5 test case title** (syntax per reference), **topic covered**, **priority**; titles must respect the T1T5 naming rules from the technique description. If your facilitator assigns T-types to teams (T1 … T5), contribute rows only for **your** assigned type; otherwise cover the full range yourself with a balanced mix across types.

6. Submit **your prompt(s)** as you actually used them, the table (or equivalent artefact), and a **short reflection** on what differed between approaches (risk depth, production safety, duplication, format, missing security or reliability angles) and what you would change next time.
