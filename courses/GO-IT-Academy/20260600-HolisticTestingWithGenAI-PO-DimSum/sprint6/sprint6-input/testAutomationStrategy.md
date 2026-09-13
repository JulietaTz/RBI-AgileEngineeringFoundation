# ToolShop Test Automation Strategy

## Purpose

Protect revenue, security, and multi-market correctness through a small, fast, API-heavy automation spine with production monitoring feedback loops.

## Automation spine (merge gate)

Priority order:

1. T1_Checkout_ValidPaymentCheckout_OrderProcessedSuccessfully
2. T1_Payment_ValidPaymentAuthorization_PaymentAccepted
3. T1_OrderManagement_SuccessfulPayment_OrderCreatedSuccessfully
4. T1_Authentication_ValidCustomerLogin_LoginSuccessful
5. T1_Checkout_LoginDuringCheckout_ProceedToCheckoutContinuesFlow
6. T5_Payment_InvalidPaymentCallback_RequestRejected
7. T5_Checkout_EURHighAmountPolicy_EnforcedOrDocumentedConsistently
8. T5_Authentication_SQLInjection_LoginRejected
9. T5_API_CrossUserResourceAccess_RequestForbidden
10. T5_AdminDestructiveOperation_WithoutAdminRole_Returns403

## Sequencing (sprints 7–8)

- **Sprint 7 (US):** Parametrize spine for USD, US shipping rules; T5 for wrong-region payment visibility.
- **Sprint 8 (China + growth):** Locale smoke T1; exploratory payment integration until stable; scheduled T4 load feeding session/timeout tests.

## Manual / exploratory

UX and copy per locale, accessibility, visual layout, new third-party integrations, post-migration chartered sessions.

## Monitoring

Alert on PAY-VAL-051, PAY-TIMEOUT-504, PAY-GEN-400, auth error bursts, order–payment mismatches. Treat FLAKY_PASS as CI failure; maintain shared `data-qa` locator map.

## Feedback loop

Production alert spikes trigger new or updated T5 scenarios; CI harness fixes precede UI coverage expansion.
