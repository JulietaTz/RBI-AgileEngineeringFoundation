#  **US3206 - MD5 Hashed password**

- Background: Due to a wrong implemented function, currently the password is stored in plain text.

## US3206-MD5 Hashed password

As a shop admin,
I want that the password is stored hashed with MD5
to improve the security of the shop.


## Acceptance Criteria:

UC1: Set/Change via UI
- at registration: set new password
- at profile: change password
- at login: change password
- at forgot password:


UC2: Convert existing passwords
- the existing passwords are converted to hashed MD5
to improve the security of the shop.
