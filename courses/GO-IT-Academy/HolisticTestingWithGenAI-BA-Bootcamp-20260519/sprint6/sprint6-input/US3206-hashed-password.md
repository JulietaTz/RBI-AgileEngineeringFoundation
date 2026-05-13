#  **US3206 - MD5 Hashed password**

- Background: Due to a wrong implemented hash-function, currently the password is stored in plain text.

## US3206-MD5 Hashed password

As a shop admin,
I want that the password is stored hashed with MD5
to improve the security of the shop.


## Acceptance Criteria:

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
