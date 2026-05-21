# User Story: Quick-Login Shortcut for Test Sessions

Status: review

## Story

As a tester,
I want a `Quick-Login` link in the top main navigation,
so that I can sign in immediately with a predefined test account without manually entering credentials.

## Acceptance Criteria

1. A `Quick-Login` link is visible in the main navigation at the top.
2. Clicking `Quick-Login` performs login with:
   - Email: `customer@practicesoftwaretesting.com`
   - Password: `welcome01`
3. On successful login, the token is persisted and the user is redirected like a normal authenticated user flow.
4. The quick-login action is covered by automated UI unit tests.

## Tasks

- [x] Add `Quick-Login` navigation link in the header.
- [x] Implement quick-login action in header component using the existing auth service.
- [x] Persist token and trigger auth state update.
- [x] Add/extend i18n label for `Quick-Login`.
- [x] Add unit tests for quick-login success and failure handling.

## Implementation Notes

- Implemented in `sprint5/UI/src/app/header/header.component.html` and `sprint5/UI/src/app/header/header.component.ts`.
- Added locale key `header.menu.quick-login` in all maintained languages.
- Added unit tests in `sprint5/UI/src/app/header/header.component.spec.ts`.
