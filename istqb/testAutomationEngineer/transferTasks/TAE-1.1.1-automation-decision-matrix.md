  
\# TAE 1.1.1 – Automation Decision Matrix

\#\# Decision Matrix

| Journey | Advantage | Disadvantage | Decision (Automate Now / Automate Later / Keep Manual) |  
|--------|----------|--------------|--------------------------------------------------------|  
| Search \+ Filter | repeatability | test-data dependency| automate later |  
| Add to Cart \+ Change Quantity | regression coverage |flaky tests  |  keep manual|  
| Checkout until Authentication | high business value | complex test setup | automate now |

\---

\#\# Decision Rationale

\#\#\# 1\. Search \+ Filter  
\- I would automate this user journey later when we are sure of a stable test data and it is uploaded with every build. For testing the filter you have to know exactly which items are there.

\#\#\# 2\. Add to Cart \+ Change Quantity  
\- Adding items to cart is a good candidate for automation but changing quantity is UI sensitive and can cause flaky tests.

\#\#\# 3\. Checkout until Authentication  
\- I would automate this user journey now. It is not heavily dependent on test data. I could get the first item in shop and add it to cart (these are preconditions). On the UI side this journey is basically just clicking one button and verifying the content of redirection page.

\---

\#\# Top Automation Priority

\#\#\# Selected Journey  
\- Checkout until Authentication

\#\#\# Final Rationale   
\- I would automate the checkout journey because it is a critical business flow and it gives us fast and valuable feedback about the stability of the build.

