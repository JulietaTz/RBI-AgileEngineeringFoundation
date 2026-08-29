# **### User Stories**



## **US3206 - MD5 Hashed password**

- Background: Due to a wrong implemented function, currently the password is stored in plain text.



### User Story: MD5 Hashed password

As a shop admin,

I want that the password is stored hashed with MD5

to improve the security of the shop.


### User Story: Convert existing password

As a shop admin,

I want that the existing passwords are converted to hashed MD5

to improve the security of the shop.



### Acceptance Criteria:

UC1: Set/Change via UI

at registration: set new password

at profile: change password

at login: change password

at forgot password:



## **US3207 - New payment method China Pay**

Background: Toolshop wants to enter the Chinese market and must offer additional payment method.



User Story: Enable China Pay as payment method

As a shop owner,

I want that the user can pay with China Pay

to offer an additional payment method that leads to more revenue.



### Acceptance Criteria:

UC1: Enable China Pay

Given the admin is on the setup payment UI

When the admin set "China Pay" = enabled

Then the user can select "China Pay" from the payment list of value.



UC2: Pay via China Pay

Given the user is on the checkout UI

When the user opens the payment list of values

And the admin has enabled "China Pay" as payment method

And the location of the user is within "China"

Then "China Pay" is displayed in the list and can be selected.



UC3: Process payment via China Pay

Given the user is on the checkout UI

When the user select "China Pay"

And the user press the "Pay"-Button

Then the payment will be processed via payment gateway "XBO-China-Pay-SD0923".



## **US3203 - Allow Chinese character set in registration**

Background: Toolshop wants to enter the Chinese market and must offer at least the registration in Chinese characters.



User Story:

As a shop admin,

I want to setup the UI with character set Big5-HKSCS

so that the UI also can be used by Chinese characters.



### Acceptance Criteria:

UI Language

Given the web shop is displayed in the browser

When I select "Chinese" from the language dropdown list

Then the UI elements will be displayed in Chinese characters

And the keyboard language switch to Chinese language

And the interaction will be done in Chinese language.



Location

Given the location of the user is within China

When the homepage is loaded

Then the used character set is Big5-HKSCS.



## **US3409 - Replace Auth-Lib**

Background: A security scan found a vulnerability X2499 in the used open-source library "Auth-X32". According to ISECC-2499 the lib should be replaced with the lib "Auth-XX64"



User Story: Fix vulnerability in "Auth-X32"

As the security officer

I want that the used open-source library "Auth-X32" will be replaced with the lib "Auth-XX64"

to close the security vulnerability ISECC-2499.



### ACC1: A vulnerability scan do not report an X2499 critical finding.



## **US3411 - New step (login) in checkout workflow**

Background: Until now, when the user is not logged-in and runs the checkout workflow, he/she can not finish the workflow. This is very annoying for the user and we many customer did not finished the check-out.



User Story: Login during checkout workflow

As a user who is not logged in

I want the possibility to perform a login during the checkout workflow

to be able to finish the workflow.



### Acceptance Criteria:

UC1: User is not logged-in

Given the user is not logged-in

And on the checkout-UI

When the user clicks "Proceed to Checkout"

Then the login dialogue will be displayed

And after a successful login the first checkout step will be displayed.



UC2: User is logged-in

Given the user is logged-in

And on the checkout-UI

When the user clicks "Proceed to Checkout"

Then the first checkout step will be displayed.



## **US5003 - Replace OR-Mapper**

Background: Due to security and performance reasons the maintainance of ORMapper Versions 1.2 is deprecated. We must replace the version 1.2 with 2.0.



User Story: Replace ORMapper 1.2 with ORMapper 2.0

As the data engineer

I want that the actual ORMapper V1.2 will be replace with ORMapper V2.0

In order to improve the security and performance of the data access.



### Acceptance Criteria
ACC1: All data structure are migrated to the new schema of V2.0.

ACC2: Data structure ACCESS is denormalized.

ACC3: A full table scan of reference table REF\_ORG\_Data (in PERF-ENV-Cloud-098) take < 1,5sec.

