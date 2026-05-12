# Example Session Sheet

## CHARTER
Analyze ESG information display and unit conversion behavior for sustainability-related product data.

## TESTER
Marco Eder

## TEST NOTES
I explored ESG badges, CO2 impact labels, and imperial unit display across category and detail pages.

### View:
- Category product grid
- Product detail ESG section
- Locale-dependent unit display settings

### Use Cases:
- Verify ESG badge visibility on filtered product listings
- Compare CO2 impact label consistency between list and detail view
- Switch unit preference and validate imperial conversion display

## RISKS
- Incorrect ESG data presentation can create compliance risk
- Inconsistent CO2 labels reduce credibility of sustainability claims
- Wrong unit conversion can cause product misuse

## BUGS### 

BUG TS-3001
CO2 impact rating differs between product tile (B) and product detail page (C) for same SKU.

BUG TS-3002
Imperial fallback displays inches for weight field in one product template instead of pounds.

## Summary
ESG features are visible and useful, but data consistency and unit mapping issues need correction.