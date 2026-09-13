# Example Session Sheet

## CHARTER
Analyze screen-reader compatibility and accessible form behavior during registration and checkout.

## TESTER
Pauline Kern

## TEST NOTES
I tested with NVDA and VoiceOver, focusing on labels, announcements, and error feedback.

### View:
- Registration form
- Address and payment forms
- Error summary component

### Use Cases:
- Submit forms with missing required fields
- Correct invalid values after inline error is announced
- Navigate form controls and helper text by screen reader shortcuts

## RISKS
- Missing labels can make form completion impossible
- Unannounced errors lead to repeated failed submissions
- Incorrect reading order can hide critical terms

## BUGS### 

BUG TS-2901
Credit card expiry field has no programmatic label; screen reader announces only "edit".

BUG TS-2902
After submit with validation errors, focus remains on submit button and error summary is not announced.

## Summary
Form accessibility has foundational support, but labeling and error-announcement defects are high-impact.