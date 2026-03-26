F4: Currency & Tax Adaptation
Currency & Tax Adaptation enables true global accessibility by delivering localized 
experiences in each market. Customers see prices in their local currency with accurate
tax calculations based on their location, eliminating confusion and cart abandonment.
Starting with the US market, this feature creates scalable infrastructure for expansion
into additional regions, transforming Roy's Roy's from a European shop into a genuinely
global marketplace serving professionals worldwide.

---

F4.1: As a US customer, I want to see all prices in USD so I understand costs without conversion.

### Test Cases 
**TC_ID:** TC_F4.1   
**Title:** Currency Display Based on User Location 

**Precondition:** User is on the product page.

- T1_Currency_validUSUserSeesPricesInUSD_allPricesAreDisplayedInUSD

- T2_Currency_userManuallySelectsUSD_allPricesAreDisplayedInUSD

- T3_Currency_systemCannotDetermineUserLocation_allPricesAreDisplayedInDefaultCurrencyEUR

- T4_Currency_userSelectsInvalidCurrency_AnErrorMessageIsDisplayed

- T5_Currency_userManipulatesCurrencyParameterViaURL_systemHandlesInvalidCurrencySecurely

---

F4.2: As a US customer, I want correct sales tax calculated for my state so I know the final price.

### Test Cases 
**TC_ID:** TC_F4.2   
**Title:** Sales Tax Calculation Based on User Location

**Precondition:** User is in the checkout process.

- T1_SalesTax_validUSUserWithSelectedState_finalPriceIsCalculatedWithCorrectSalesTax 

- T2_SalesTax_validUSUserChangesStateDuringCheckout_finalPriceIsRecalculatedWithCorrectSalesTax  

- T3_SalesTax_taxRateForStateIsUnavailable_finalPriceCannotBeCalculatedCorrectly  

- T4_SalesTax_validUSUserEntersInvalidStateDuringCheckout_stateIsRejectedAndErrorMessageIsDisplayed  

- T5_SalesTax_userManipulatesStateParameterViaURL_systemRejectsInvalidStateAndHandlesRequestSecurely  

---

F4.3: As a business analyst, I want regional pricing strategies so we remain competitive.
### Test Cases 
**TC_ID:** TC_F4.3     
**Title:** Regional Pricing Strategies

**Precondition:** User is on the product page.

- T1_RegionalPricing_validUserSelectsUSRegion_regionSpecificPricesAreDisplayed

- T2_RegionalPricing_validUserChangesRegion_pricesAreRecalculatedAccordingToNewRegion

- T3_RegionalPricing_pricingDataForSelectedRegionIsUnavailable_pricesCannotBeDisplayed

- T4_RegionalPricing_userSelectsInvalidRegion_regionIsRejectedAndErrorMessageIsDisplayed

- T5_RegionalPricing_userManipulatesRegionViaVPN_systemDetecsAndPreventsIncorrectPricingDueToRegionManipulation


---


As a US customer, I want to see shipping costs before checkout so there are no surprises.

### Test Cases 
**TC_ID:** TC_F4.5  
**Title:** Shipping Costs Visibility Before Checkout

- T1_ShippingCost_userViewsProductPage_shippingCostIsDisplayedBeforeCheckout

- T2_ShippingCost_userEntersShippingAddress_shippingCostIsDisplayedBeforeCheckout

- T3_ShippingCost_shippingDataForSelectedRegionIsUnavailable_shippingCostCannotBeDisplayed

- T4_ShippingCost_userDoesNotEnterShippingAddress_shippingCostIsNotCalculatedAndCannotBeDisplayed

- T5_ShippingCost_userManipulatesShippingRegionViaURL_systemPreventsIncorrectShippingCostDisplay
