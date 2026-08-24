# Example Session Sheet

## CHARTER
Analyze multilingual behavior of the AI advisor and report on language fallback and localization defects.

## TESTER
Sofia Berger

## TEST NOTES
I used German, Spanish, and mixed-language prompts to validate language handling across prompts and responses.

### View:
- AI advisor input area
- Localization toggles in profile settings
- Saved recommendations list

### Use Cases:
- Ask for recommendations in German with euro budget
- Switch interface language after initial English conversation
- Save and reopen multilingual recommendations

## RISKS
- Incorrect language fallback creates confusing recommendations
- Currency formatting mismatch can lead to wrong purchase decisions
- Partial localization degrades usability

## BUGS### 

BUG TS-2401
AI starts in German but explanation paragraphs partially revert to English after follow-up question.

BUG TS-2402
Saved recommendation title is always stored in English regardless of active locale.

## Summary
Multilingual support works in simple cases, but language persistence and saved-content localization are incomplete.