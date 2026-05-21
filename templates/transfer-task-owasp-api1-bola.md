# Transfer Task

## Title

OWASP API1: Broken Object Level Authorization (BOLA / IDOR) — design and execute object-level access tests

## Learning Objective

After completing this task, you can explain what broken object level authorization is, identify high-risk API patterns (IDs in paths or payloads), and produce concrete test steps plus evidence that show whether one authenticated user can access or modify another user’s objects.

## Context

Many APIs expose resources by identifier, for example `/api/v1/accounts/{accountId}` or `/api/v1/documents/{documentId}`. If the server checks only that *someone* is logged in, but not that the caller is allowed to use *that specific* `accountId` or `documentId`, attackers can swap IDs and read or change other users’ data. OWASP API Security ranks this as **API1: Broken Object Level Authorization**.

This failure is common after authentication is added, but per-object authorization is forgotten or implemented inconsistently across endpoints.

## Task Description

1. **Pick a real target** you are allowed to test: a staging API, a training lab (e.g. OWASP Juice Shop, crAPI, WebGoat API lessons), or an internal non-production service with written permission. Do not test production systems or third-party APIs without authorization.

2. **Model the authorization boundary** in one short paragraph:
   - Which *objects* exist (e.g. orders, profiles, files)?
   - Which *roles or users* should be able to access which objects?
   - Where do object IDs appear (URL path, query string, JSON body, headers)?

3. **Create a minimal test matrix** (table or bullet list) with at least **six** cases that combine:
   - two different authenticated identities (User A and User B), and  
   - at least two verbs (e.g. `GET` and `PATCH` or `DELETE`) against a resource that clearly “belongs” to User B, **while authenticated as User A**, by substituting B’s object ID where A’s would normally appear.

4. **Execute the tests** using your normal toolkit (e.g. `curl`, Postman, Insomnia, or an HTTP proxy). Capture:
   - request (method, path, relevant headers redacted if needed),  
   - HTTP status,  
   - whether the response leaks B’s data or confirms a change to B’s object.

5. **Summarize results** in a short “risk statement”: what an attacker could do if the issue exists, and whether your sample showed presence or absence of the flaw.

6. **Draft one remediation recommendation** tied to your findings (e.g. server-side lookup keyed by session subject + policy check before returning the object; avoid trusting client-supplied ownership fields alone).

## Expected Outcome

A single markdown file (name it as your organization prefers, e.g. `bola-transfer-task-outcome.md`) containing:

- Your authorization boundary description  
- The test matrix  
- Execution evidence (status codes + brief notes; paste minimal safe snippets, redact tokens)  
- Risk statement  
- One concrete remediation aligned with your API style  

## Constraints

- Use only environments and accounts you are explicitly allowed to test.  
- Redact secrets (tokens, passwords, full PII) in any pasted traffic or screenshots.  
- Do not perform destructive tests (e.g. mass delete) unless the environment is designed for that and policy allows it.  
- If every test returns `403` / `404` and no data leakage, document that as a **negative result**; the task is still complete.

## Acceptance Criteria

- [ ] Objective boundary (objects, users/roles, where IDs appear) is written clearly.  
- [ ] At least six cross-user ID substitution cases are defined before execution.  
- [ ] Each defined case has a recorded result (status + outcome: access denied vs possible BOLA).  
- [ ] Risk statement reflects the evidence, not generic OWASP text.  
- [ ] At least one remediation item is specific to the API patterns you tested.  

## Hints

- Favor **horizontal** escalation (peer user to peer user) over admin abuse for this task; BOLA is about object ownership, not only role hierarchy.  
- Check **write** operations, not only `GET`: missing checks on `PUT`/`PATCH`/`DELETE` are a frequent source of impact.  
- If the API uses opaque IDs, look for **second references** to the same object (numeric internal ID vs public UUID, alternate endpoints, batch APIs, export/download links).  
- GraphQL: object-level flaws often appear as **field resolvers** that do not re-check authorization per node.

## References

- [OWASP API Security Top 10 2023 — API1: Broken Object Level Authorization](https://owasp.org/API-Security/editions/2023/en/0xa1-broken-object-level-authorization/)  
- [OWASP Cheat Sheet Series — Authorization](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)  
- [OWASP Testing Guide — Testing for Insecure Direct Object References (legacy IDOR framing; still useful for patterns)](https://owasp.org/www-project-web-security-testing-guide/)

## Estimated Duration

90–120 minutes (including environment setup and write-up).
