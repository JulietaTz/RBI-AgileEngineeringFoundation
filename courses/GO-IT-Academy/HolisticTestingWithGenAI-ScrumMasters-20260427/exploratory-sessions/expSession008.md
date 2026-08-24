# Example Session Sheet

## CHARTER
Analyze keyboard-only navigation in core shopping journeys and report accessibility blockers.

## TESTER
Emma Wimmer

## TEST NOTES
I navigated all critical user paths with keyboard only and verified focus behavior.

### View:
- Home page hero and navigation
- Category list and product detail page
- Cart and checkout forms

### Use Cases:
- Complete product search and add to cart using keyboard only
- Navigate dropdown menus with arrow keys and Esc
- Use skip link to jump directly to main content

## RISKS
- Keyboard traps make core flows inaccessible
- Missing focus indicators cause navigation uncertainty
- Non-logical tab order increases task completion time

## BUGS### 

BUG TS-2801
Focus disappears after closing category dropdown with Esc; next Tab starts again at browser chrome.

BUG TS-2802
Skip-to-content link is present in DOM but not visible on focus due to clipped container style.

## Summary
Critical keyboard path is partially accessible, but focus management defects still block reliable usage.