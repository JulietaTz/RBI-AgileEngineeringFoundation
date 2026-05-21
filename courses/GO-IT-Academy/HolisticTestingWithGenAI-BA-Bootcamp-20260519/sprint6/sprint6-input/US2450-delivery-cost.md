# User Story: Delivery Costs Calculation
## US2450-Calculate Delivery Costs During Checkout

As a customer
I want to see the delivery costs before placing my order
So that I can understand the total price of my purchase and make an informed buying decision.

## Description

The webshop shall calculate and display delivery costs during the checkout workflow based on predefined business rules.

The delivery cost calculation:

Free Standard Shipping: 
- Applies to all orders over €75 (excluding "Oversized" items).

Flat Rate Shipping: 
- €5.95 for orders under €75. 

Heavy/Oversized Surcharge: 
- Any single item over 25kg or large dimensions adds €20 surcharge, regardless of order total. 

Express Option: 
- €12.95 for next-day delivery (only for in-stock items).

## Acceptance Criteria

### Functional
- Delivery costs are calculated based on the selected delivery method.
- Delivery costs are updated automatically when products are added or removed from the cart.
- Country-specific delivery costs are applied based on the shipping address.
- Free shipping is applied automatically when the order value exceeds the configured threshold.
- The customer can view delivery costs before confirming the order.
- The order summary displays product costs, delivery costs, and total costs separately.
- Delivery costs are recalculated when the shipping address changes.
- Unsupported delivery destinations display an appropriate error message.

### Non-Functional
- Delivery cost calculation is completed within 2 seconds during checkout.
- Delivery cost configuration can be maintained without code changes.
