# New step (login) in checkout workflow**

## Background: 
Until now, when the user is not logged-in and runs the checkout workflow, he/she can not finish the workflow. 
This is very annoying for the user and many customer did not finished the check-out.


## US4700-Login during checkout workflow

As a user who is not logged in
I want the possibility to perform a login during the checkout workflow
to be able to finish the workflow.

## Acceptance Criteria:

### UC1: User is not logged-in
Given the user is not logged-in
And on the checkout-UI
When the user clicks "Proceed to Checkout"
Then the login dialogue will be displayed
And after a successful login the first checkout step will be displayed.


### UC2: User is logged-in
Given the user is logged-in
And on the checkout-UI
When the user clicks "Proceed to Checkout"
Then the first checkout step will be displayed.

### Acceptance criteria
- ACC-01: When a not logged-in user clicks "Proceed to Checkout", a login dialog is displayed.
- ACC-02: After a successful login during checkout, the user is redirected to the first checkout step.
- ACC-03: The products currently stored in the shopping cart remain unchanged after login.
- ACC-04: Logged-in users can directly access the first checkout step without seeing the login dialog.
- ACC-05: Invalid login credentials entered during checkout display an appropriate error message.
- ACC-06: The user can retry the login process after a failed login attempt without leaving the checkout workflow.
- ACC-07: The login dialog provides access to the "Forgot Password" functionality.
- ACC-08: The login dialog provides access to the user registration workflow for new customers.
- ACC-09: The checkout workflow continues correctly after login without losing checkout-related data.
- ACC-10: Unauthorized users cannot access protected checkout steps without successful authentication.