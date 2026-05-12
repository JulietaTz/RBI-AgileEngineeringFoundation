# Example Session Sheet

## CHARTER
Analyze rating update behavior and report on defects in edit history consistency and visibility.

## TESTER
Mila Novak

## TEST NOTES
I focused on editing ratings from multiple entry points and checking synchronization between list and detail views.

### View:
- My ratings overview
- Product detail rating section
- User profile activity page

### Use Cases:
- Edit existing star rating from 2 to 5 stars
- Edit text comment repeatedly within a short interval
- Open same rating in two browser tabs and save conflicting edits

## RISKS
- Last-write wins can overwrite meaningful customer feedback
- Cached views can display old ratings and mislead buyers
- Missing audit metadata reduces transparency

## BUGS### 

BUG TS-2201
After editing a rating, product detail page keeps old star value until hard refresh.

BUG TS-2202
Concurrent edits from two tabs overwrite each other without conflict warning or timestamp comparison.

## Summary
Editing capability is present, but consistency and conflict handling need fixes before broad rollout.