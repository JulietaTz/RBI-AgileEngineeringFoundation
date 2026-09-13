# Example Session Sheet

## CHARTER
Analyze sales-tax calculation by shipping state and report on defects affecting final order total accuracy.

## TESTER
Luca Gruber

## TEST NOTES
I exercised checkout with multiple US states and edge inputs to verify tax line-item behavior.

### View:
- Cart summary
- Checkout address step
- Payment review page

### Use Cases:
- Calculate tax for CA, NY, TX, and OR shipping addresses
- Change shipping state late in checkout and verify recalculation
- Trigger tax service failure path and evaluate user messaging

## RISKS
- Wrong tax can cause compliance and legal exposure
- Delayed recalculation can misstate final payable amount
- Silent failure undermines checkout confidence

## BUGS### 

BUG TS-2501
Switching shipping state from OR to CA updates total amount but keeps tax line at 0.00 until page reload.

BUG TS-2502
When tax service times out, banner displays generic error and hides subtotal and total values.

## Summary
Tax logic exists, but dynamic update and failure handling defects can impact compliance and conversion.