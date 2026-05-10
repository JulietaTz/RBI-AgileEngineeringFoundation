# Apply Review Process Activities on Toolshop

## 1. Title
Apply Review Process Activities on Toolshop

## 2. Syllabus Reference
ISTQB FL – 3.2.2 Review Process Activities

## 3. Learning Objective
Apply ISTQB FL section 3.2.2 (Review Process Activities) in a realistic webshop testing situation. Produce one concrete testing artifact that can be used by the Scrum team in the current sprint.

## 4. Context / Scenario
Your Scrum team is preparing the next Toolshop increment at https://practicesoftwaretesting.com/. During sprint refinement, you are asked to apply this syllabus concept to reduce risk before release. 

## 5. Task Instructions
1. Run a mini-review cycle with 2 fAVENGERS (planning, preparation, review meeting, rework, follow-up) for the user story in section 9 (Social Media Registration Extension9.
4. Record observations with evidence (steps, inputs, expected result, actual result, and notes on risk/quality impact).
5. Refine your result into a sprint-ready artifact that can be shared in a daily stand-up or review.

## 6. Expected Outcome / Deliverable
A review result log in md-file (findings, severity, rework owner, and follow-up status).

## 7. Timebox
30 minutes

## 8. Hints (Optional)
- Keep scope narrow: one feature flow is enough.
- If documentation is unclear, state assumptions explicitly in your artifact.
- Use screenshots only if they speed up communication; concise textual evidence is sufficient.

## 9. User Story
# User Story: Social Media Registration Extension

## Title
Extend Registration with Social Media Login Options

---

## User Story
As a new user,  
I want to register using my existing social media accounts (Google,  
GitHub, LinkedIn),  
so that I can sign up quickly without creating a new username and  
password.

---

## Business Value
- Reduce friction in the registration process  
- Increase conversion rate for new users  
- Improve user experience with faster onboarding  

---

## Acceptance Criteria

### AC1: Display Social Login Options
- Given I am on the registration page  
- When the page is loaded  
- Then I see options to register with:
  - Google  
  - GitHub  
  - LinkedIn  

---

### AC2: Successful Registration via Google
- Given I select "Register with Google"  
- When I authenticate successfully with my Google account  
- Then a new user account is created  
- And I am logged into the webshop  

---

### AC3: Successful Registration via GitHub
- Given I select "Register with GitHub"  
- When I authenticate successfully with my GitHub account  
- Then a new user account is created  
- And I am logged into the webshop  

---

### AC4: Successful Registration via LinkedIn
- Given I select "Register with LinkedIn"  
- When I authenticate successfully with my LinkedIn account  
- Then a new user account is created  
- And I am logged into the webshop  

---

### AC5: Existing Account Handling
- Given an account already exists with the same email  

---

### AC6: Error Handling
- Given authentication with a social provider fails  
- When the error occurs  

---

### AC7: Required Data Handling
- Given I register via a social provider  
- When mandatory user data is missing (e.g. address)  
- Then I am prompted to complete the missing information  

---

## Definition of Done
- TBD

