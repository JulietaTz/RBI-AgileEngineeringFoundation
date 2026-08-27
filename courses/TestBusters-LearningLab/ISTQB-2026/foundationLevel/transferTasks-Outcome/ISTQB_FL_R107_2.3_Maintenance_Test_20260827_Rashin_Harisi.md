# Apply Maintenance Testing on Toolshop

## Reference to ISTQB Syllabus chapter
ISTQB FL – 2.3 Maintenance Testing

## Link to the transfer task file
https://github.com/rgroetz2/TBLL-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/foundationLevel/transferTasks/Chapter%202/ISTQB-FL-2.3_Maintenance_Testing_20260405.md

## Outcome

### Selected Feature Flow

Authentication – Social Login

### Recently Changed Behavior

Sprint 5 introduces Social Login with Google and GitHub OAuth.

This change extends the existing authentication functionality by providing additional login methods besides the existing email/password login.

### Maintenance Trigger

**Trigger category:** Modification

The addition of Google and GitHub Social Login is a modification of the existing authentication functionality. This change triggers maintenance testing because both the new behavior and potentially affected existing authentication functionality need to be tested.

### Requirement / Behavior Reference

According to the Sprint 5 documentation, Social Login was introduced with the following API endpoints:

- `GET /auth/social-login` – initiates social login.
- `GET /auth/cb/google` – handles the Google OAuth callback.
- `GET /auth/cb/github` – handles the GitHub OAuth callback.

The expected behavior is that users can authenticate through the supported social login providers while the existing authentication functionality continues to work correctly.

### Impact Analysis

The Social Login modification may affect several areas of the existing authentication flow.

| Potentially affected area | Reason for impact | Testing needed |
|---|---|---|
| Social Login | This is the newly added functionality. | Verify Google/GitHub authentication behavior. |
| Existing email/password login | Social Login extends the existing authentication mechanism. | Verify that normal login still works. |
| User session | All authentication methods must create a valid authenticated session. | Verify that the authenticated user can access the account. |
| Account access | Authentication changes may affect access to protected account functionality. | Verify that the user can access the account after login. |
| Logout | Session handling may be affected by changes to authentication. | Verify that logout still ends the authenticated session. |
| Route protection | Protected routes depend on authentication state. | Verify that unauthenticated users cannot access protected account functionality. |

---

### Test Basis and Testware Impact

The Sprint 5 Social Login requirements and authentication behavior form part of the test basis for this maintenance change.

Existing authentication testware should be reviewed to determine which test cases need to be updated or re-executed. Traceability between the changed authentication requirements and related test cases helps identify the appropriate maintenance and regression test scope.

### Maintenance Test Set

#### Test 1 – Social Login

| Item | Observation |
|---|---|
| Test purpose | Test the recently added Social Login functionality. |
| Steps |Select Google as the Social Login provider and complete the authentication flow using a valid Google account. |
| Input | Valid Google account |
| Expected Result | The user should be authenticated successfully and redirected to the appropriate authenticated area. |
| Actual Result | The user was successfully authenticated with Google and redirected to the user profile page. The account profile was accessible and displayed correctly. |
| Result | PASS |
| Risk / Quality Impact | Failure of the new authentication method prevents users from accessing Toolshop through the supported social provider. |

#### Test 2 – Existing Email/Password Login

| Item | Observation |
|---|---|
| Test purpose | Regression test of existing email/password authentication functionality after the addition of Social Login. |
| Steps | Open the login page and log in using the email and password of a predefined test user provided in the Toolshop documentation. |
| Input | Valid predefined test-user email + valid password |
| Expected Result | The user should be authenticated and redirected to the account/profile area. |
| Actual Result | The user was successfully authenticated using email and password and redirected to the user profile page. |
| Result | PASS |
| Risk / Quality Impact | The addition of Social Login could unintentionally affect the existing email/password authentication flow and prevent existing users from accessing their accounts. |

#### Test 3 – Account Access After Authentication

| Item | Observation |
|---|---|
| Test purpose | Regression test of route protection after the addition of Social Login. |
| Steps | Log out of Toolshop and try to access the account profile page directly. |
| Input | `/account/profile` while unauthenticated |
| Expected Result | An unauthenticated user should not be able to access the profile page and should be redirected to the login page. |
| Actual Result | After logging out, direct access to `/account/profile` redirected the user to the login page. |
| Result | PASS |
| Risk / Quality Impact | Authentication changes could unintentionally allow unauthenticated users to access protected account functionality. |

#### Test 4 – Logout

| Item | Observation |
|---|---|
| Test purpose | Regression test of logout and session handling after the addition of Social Login. |
| Steps | Log in with a predefined email/password user, sign out, and then try to access `/account/profile` directly. |
| Input | Authenticated user session followed by logout |
| Expected Result | The user session should end successfully, and direct access to the protected profile page should redirect to the login page. |
| Actual Result | After signing out, direct access to `/account/profile` redirected the user to the login page. |
| Result | PASS |
| Risk / Quality Impact | A logout or session-handling defect could leave authenticated access active after sign-out and create a security risk. |

### Sprint-ready Conclusion

The addition of Social Login in Sprint 5 was treated as a maintenance trigger because it modifies the existing authentication functionality.

Impact analysis identified existing authentication, protected account access, and session handling as potentially affected areas. The new Google Social Login functionality and the selected regression tests were executed successfully. Existing email/password login, protected route behavior, and logout/session termination continued to work as expected.

All executed maintenance tests passed, and no regression issues were observed in the tested authentication-related functionality.

## Learning Summary

| Concept | One-line takeaway |
|---|---|
| Maintenance Testing | Testing is required after changes to an existing system to verify the change and detect unintended effects. |
| Maintenance Trigger | Adding Social Login is a modification of the existing authentication functionality. |
| Impact Analysis | Analyzing the change helps identify which existing areas may be affected and should be re-tested. |
| Changed Functionality | Google Social Login was tested successfully and provided access to the user profile. |
| Regression Testing | Existing email/password login, protected account access, and logout/session handling were re-tested after the modification. |
| Test Basis and Testware | Changes to system behavior may require related requirements and existing testware to be reviewed or updated. |
| Traceability | Traceability helps identify test cases related to changed or potentially affected requirements. |
| Maintenance Test Result | All executed maintenance and regression tests passed, with no regression issues observed in the tested scope. |