# **US3206 - MD5 Hashed password / Password strength**

- Background: 
    - Due to a wrong implemented hash-function, currently the password is stored in plain text.
    - Due to missing password policy the user used password which are easy to find out with a brut force attack.

## Hashed password

As a shop admin,
I want that the password is stored hashed with MD5
to improve the security of the shop.

## Password strenght
As a user
I want to see whether my password is strong enough during password creation or change
So that I can create a secure password that fulfills the webshop security requirements.

- The password policy must enforce the following rules:
    - Minimum length of 8 characters
    - At least one uppercase letter
    - At least one lowercase letter
    - At least one numeric digit
    - At least one special character (e.g. @, #, $, %, &)

## Acceptance Criteria

### Password strenght
- The webshop validates the password against all defined password policy rules.
- The password strength indicator updates dynamically while the user types the password.
- The password strength indicator displays different strength levels (e.g. Weak, Medium, Strong).
- Password creation is blocked if the password does not fulfill the required password policy.
- A validation message explains which password rule is currently violated.
- Password validation is applied during registration, password change, and forgot password workflows.
- The password field supports special characters required by the password policy.
- The password strength indicator is displayed next to or below the password input field.
- Successfully validated passwords can be stored and used for authentication.
- Existing authentication workflows continue to function correctly after the password strength feature is introduced.

### Hashed password
- ACC-01: Newly registered user passwords are stored as MD5 hashes in the database and not in plain text.
- ACC-02: Passwords changed via the user profile page are stored as MD5 hashes in the database.
- ACC-03: Passwords changed during the login workflow are stored as MD5 hashes in the database.
- ACC-04: Passwords reset via the "Forgot Password" workflow are stored as MD5 hashes in the database.
- ACC-05: Plain text passwords are no longer stored in the database after password creation or password change operations.
- ACC-06: Existing customer passwords are converted from plain text to MD5 hashed values during the migration process.
- ACC-07: Existing users can still successfully log in with their current password after the password conversion process.
- ACC-08: Authentication using MD5 hashed passwords works correctly for all supported login workflows.
- ACC-09: The webshop does not expose passwords in plain text in logs, API responses, or UI screens.
- ACC-10: Failed password conversion attempts are logged with technical error information without exposing the original password value.

