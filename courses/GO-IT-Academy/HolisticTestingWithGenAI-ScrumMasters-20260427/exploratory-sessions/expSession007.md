# Example Session Sheet

## CHARTER
Analyze shipping cost transparency in checkout and report defects that can surprise customers before payment.

## TESTER
David Huber

## TEST NOTES
I explored standard and express shipping options with varied cart values and destinations.

### View:
- Cart shipping estimator
- Checkout shipping method step
- Payment review step

### Use Cases:
- Compare shipping prices between standard and express delivery
- Validate free-shipping threshold behavior around boundary amounts
- Update postal code and verify shipping recalculation

## RISKS
- Hidden shipping fees can increase cart abandonment
- Wrong threshold logic can impact margin and customer trust
- Delayed updates can show outdated totals

## BUGS### 

BUG TS-2701
Shipping estimator shows free shipping at 99.99 although threshold policy is 100.00.

BUG TS-2702
Changing postal code updates shipping method labels but keeps previous cost values until method is reselected.

## Summary
Shipping visibility is close to expected, but threshold and recalculation defects can mislead users at checkout.