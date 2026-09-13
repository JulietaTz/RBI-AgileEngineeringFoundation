# Example Session Sheet

## CHARTER
Analyze currency conversion and price display consistency across product listing, cart, and checkout.

## TESTER
Nora Fuchs

## TEST NOTES
I switched currencies repeatedly and validated rounding behavior across key buying flows.

### View:
- Product catalog page
- Mini cart and cart page
- Checkout order summary

### Use Cases:
- Change display currency between USD, EUR, and GBP
- Add multiple items with discounts and verify converted totals
- Refresh page and validate currency preference persistence

## RISKS
- Inconsistent conversion creates billing disputes
- Rounding defects can accumulate on multi-item carts
- Session-level preference loss confuses returning users

## BUGS### 

BUG TS-2601
Catalog shows EUR prices rounded to 2 decimals, cart line items rounded to 3 decimals for same product.

BUG TS-2602
Currency preference resets to USD after login redirect during checkout.

## Summary
Currency feature is functional but consistency gaps are visible and likely to erode trust at payment time.