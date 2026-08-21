# Example Session Sheet

## CHARTER
Analyze the product rating workflow for verified purchases and report on defects that block trustworthy customer feedback.

## TESTER
Alex Steiner

## TEST NOTES
I explored rating creation from product page and order history, including positive path and misuse attempts.

### View:
- Product detail page
- Order history page
- Rating modal component

### Use Cases:
- Submit 1-5 star rating after verified purchase
- Attempt to rate product without verified purchase
- Add optional comment with boundary lengths (0, 1, 500, 501)

## RISKS
- Unverified users can manipulate product reputation
- Character limit can break UI or truncate data silently
- Confirmation appears even when save fails

## BUGS### 

BUG TS-2101
Rating form allows submission for guest users when product page is opened in a stale authenticated tab.

BUG TS-2102
Comment field accepts 501 characters through paste operation; API returns 500, UI still shows success toast.

## Summary
Core flow mostly works, but trust and validation defects make current implementation not release-ready.