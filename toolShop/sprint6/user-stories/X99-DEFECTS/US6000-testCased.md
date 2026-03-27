# US6000 - Contact Form Test Cases (T1-T5)

## Notes / Assumptions
The original `US6000-task-testDesign.md` mentions uploading a `txt-file` (250kb). The `US6000-contact.md` story/AC does not mention attachments. This test set therefore focuses on what is explicitly specified for the contact form: mandatory fields, allowed subject values, and email validation.

## Decision Table - Email Validation

Business rule (as provided by the task):
Only emails with allowed domains are accepted:
- `gmail.com`
- `gmx.com`
- Business emails (example domains such as `example.com`, `company.co.uk`, etc.)

Invalid emails must be rejected with an error message.

Legend:
- Syntax valid = matches basic email structure rules checked by the UI/backend (single `@`, non-empty local/domain, valid characters, sensible dot-separated domain).
- Domain allowed = domain is `gmail.com`, `gmx.com`, or a business domain (non-gmail/gmx provider).

| DT-ID | Syntax valid? | Domain allowed? | Email example | Expected result |
|---|---|---|---|---|
| DT1 | Yes | Yes (`gmail.com`) | `john.doe@gmail.com` | ACCEPT |
| DT2 | Yes | Yes (`gmx.com`) | `john.doe@gmx.com` | ACCEPT |
| DT3 | Yes | Yes (business domain) | `john.doe@acme-corp.com` | ACCEPT |
| DT4 | No | N/A | `johndoegmail.com` (missing `@`) | REJECT - invalid email format |
| DT5 | No | N/A | `john@@gmail.com` (multiple `@`) | REJECT - invalid email format |
| DT6 | No | N/A | `john doe@gmail.com` (space in local part) | REJECT - invalid email format |
| DT7 | No | N/A | `john..doe@gmail.com` (consecutive dots in local part) | REJECT - invalid email format |
| DT8 | No | N/A | `.john.doe@gmail.com` (local part starts with dot) | REJECT - invalid email format |
| DT9 | No | N/A | `john.doe@gmailcom` (domain missing dot/TLD separation) | REJECT - invalid email format |
| DT10 | No | N/A | `john.doe@gma!il.com` (invalid character in domain) | REJECT - invalid email format |
| DT11 | Yes | No (disallowed consumer provider) | `john.doe@yahoo.com` | REJECT - email domain not allowed |
| DT12 | Yes | No (disallowed consumer provider) | `john.doe@outlook.com` | REJECT - email domain not allowed |
| DT13 | No | N/A | `john.doe@gmail.com.` (trailing dot in domain) | REJECT - invalid email format |
| DT14 | No | N/A | `@gmail.com` (empty local part) | REJECT - invalid email format |
| DT15 | No | N/A | `john.@gmail.com` (local part ends with dot) | REJECT - invalid email format |
| DT16 | No | N/A | `john.doe@gmail..com` (consecutive dots in domain) | REJECT - invalid email format |
| DT17 | No | N/A | `john.doe@-gmail.com` (domain starts with hyphen) | REJECT - invalid email format |
| DT18 | No | N/A | `john.doe@gmail-.com` (domain ends with hyphen) | REJECT - invalid email format |
| DT19 | No | N/A | `john.doe@gmail.c` (TLD too short) | REJECT - invalid email format |
| DT20 | No | N/A | `john.doe@` (empty domain) | REJECT - invalid email format |
| DT21 | Yes | No (disallowed consumer provider) | `john.doe@icloud.com` | REJECT - email domain not allowed |
| DT22 | Yes | No (disallowed consumer provider) | `john.doe@proton.me` | REJECT - email domain not allowed |

## T1 - Standard Cases (Happy Path)

`T1_contact_sendMessage_with_allowed_email_domain_send_email_to_shop_owner`

`T1_contact_contact_linkDisplayed_and_contact_form_fieldsDisplayed`
- Given the user is on any UI of the web shop
- When the user looks at the main menu
- Then a `Contact` link is displayed (ACC1)
- When the user clicks `Contact`
- Then a contact form is displayed with the fields: `First Name`, `Last Name`, `EMail address`, `Subject`, `Message` (ACC2)

1. `T1_contact_sendMessage_with_allowed_email_domain_gmail_send_email_to_shop_owner`
   - Given the user is on any UI of the web shop
   - When the user clicks `Contact`
   - Then a contact form is displayed
   - When the user fills `First Name`, `Last Name`, `EMail address` (value from DT1), selects a valid `Subject` (from the list), fills `Message`, and clicks `Send`
   - Then an email is sent to the shop owner

2. `T1_contact_sendMessage_with_allowed_email_domain_gmx_send_email_to_shop_owner`
   - When the user fills `EMail address` (value from DT2) and clicks `Send`
   - Then an email is sent to the shop owner

3. `T1_contact_sendMessage_with_allowed_email_domain_business_send_email_to_shop_owner`
   - When the user fills `EMail address` (value from DT3) and clicks `Send`
   - Then an email is sent to the shop owner

## T2 - Alternative Cases (Still Valid)

`T2_contact_sendMessage_with_different_allowed_subject_value_send_email_to_shop_owner`

1. `T2_contact_sendMessage_subject_Customer_Service_send_email_to_shop_owner`
   - Use a valid allowed email (DT1/DT2/DT3) and select `Customer Service`
   - Expected: email is sent to the shop owner

2. `T2_contact_sendMessage_subject_Webmaster_send_email_to_shop_owner`
   - Use a valid allowed email and select `Webmaster`
   - Expected: email is sent to the shop owner

3. `T2_contact_sendMessage_subject_General_send_email_to_shop_owner`
   - Use a valid allowed email and select `General`
   - Expected: email is sent to the shop owner

4. `T2_contact_sendMessage_subject_Info_send_email_to_shop_owner`
   - Use a valid allowed email and select `Info`
   - Expected: email is sent to the shop owner

## T3 - Exception Cases (Deviation Handling)

`T3_contact_sendMessage_mail_service_unavailable_show_error_message`
1. `T3_contact_sendMessage_when_backend_email_send_fails_show_error_message_and_no_success_confirmation`
   - Given the user filled all mandatory fields with a valid allowed email (DT1/DT2/DT3) and clicks `Send`
   - When the backend fails to send the email (SMTP/service error/timeout)
   - Then an error message is shown and the user is not incorrectly informed about a successful email send

## T4 - Negative Cases (Invalid Input/State)

`T4_contact_form_shows_error_for_invalid_or_missing_input`

### T4 - Missing mandatory fields
1. `T4_contact_sendMessage_when_FirstName_is_missing_show_error_message`
2. `T4_contact_sendMessage_when_LastName_is_missing_show_error_message`
3. `T4_contact_sendMessage_when_Email_is_missing_show_error_message`
4. `T4_contact_sendMessage_when_Subject_is_missing_show_error_message`
5. `T4_contact_sendMessage_when_Message_is_missing_show_error_message`

### T4 - Invalid email formats (decision table mapping)
1. `T4_contact_sendMessage_when_email_missing_at_symbol_reject_show_error_message` (DT4)
2. `T4_contact_sendMessage_when_email_has_multiple_at_symbols_reject_show_error_message` (DT5)
3. `T4_contact_sendMessage_when_email_contains_spaces_reject_show_error_message` (DT6)
4. `T4_contact_sendMessage_when_email_local_part_has_consecutive_dots_reject_show_error_message` (DT7)
5. `T4_contact_sendMessage_when_email_local_part_starts_with_dot_reject_show_error_message` (DT8)
6. `T4_contact_sendMessage_when_email_domain_missing_dot_or_tld_separator_reject_show_error_message` (DT9)
7. `T4_contact_sendMessage_when_email_domain_contains_invalid_characters_reject_show_error_message` (DT10)
8. `T4_contact_sendMessage_when_email_domain_has_trailing_dot_reject_show_error_message` (DT13)
9. `T4_contact_sendMessage_when_email_local_part_is_empty_reject_show_error_message` (DT14)
10. `T4_contact_sendMessage_when_email_local_part_ends_with_dot_reject_show_error_message` (DT15)
11. `T4_contact_sendMessage_when_email_domain_has_consecutive_dots_reject_show_error_message` (DT16)
12. `T4_contact_sendMessage_when_email_domain_starts_with_hyphen_reject_show_error_message` (DT17)
13. `T4_contact_sendMessage_when_email_domain_ends_with_hyphen_reject_show_error_message` (DT18)
14. `T4_contact_sendMessage_when_email_tld_is_too_short_reject_show_error_message` (DT19)
15. `T4_contact_sendMessage_when_email_domain_is_empty_reject_show_error_message` (DT20)

### T4 - Valid syntax but disallowed email domains
1. `T4_contact_sendMessage_when_email_domain_is_yahoo_reject_show_error_message` (DT11)
2. `T4_contact_sendMessage_when_email_domain_is_outlook_reject_show_error_message` (DT12)
3. `T4_contact_sendMessage_when_email_domain_is_icloud_reject_show_error_message` (DT21)
4. `T4_contact_sendMessage_when_email_domain_is_proton_reject_show_error_message` (DT22)

### T4 - Invalid subject value (must be from the allowed list)
1. `T4_contact_sendMessage_when_subject_value_is_not_from_allowed_list_reject_show_error_message`
   - Attempt to submit an invalid `Subject` value (not one of: `Customer Service`, `Webmaster`, `General`, `Info`)
   - Expected: error message and no email is sent

## T5 - Misuse / Robustness Cases

`T5_contact_input_validation_resists_malicious_payloads`
1. `T5_contact_sendMessage_email_field_header_injection_payload_is_rejected_show_error_message`
   - Example payload idea: email value containing newline/control characters in addition to a valid-looking email
   - Expected: rejection with error message; no email is sent

2. `T5_contact_sendMessage_subject_or_message_contains_script_tags_is_escaped_and_not_executed`
   - Enter payload such as `<script>alert(1)</script>` in `Message` (or `Subject` if possible)
   - Expected: the application does not execute the script; user sees a safe output/error handling; form submission behavior follows AC (email only sent when mandatory fields are valid)

