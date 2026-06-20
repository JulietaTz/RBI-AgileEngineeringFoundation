# Agile Vision Statement — ToolShop HOLTES

## Vision

**For** Czech customers and ToolShop stakeholders seeking sustainable international growth,  
**who** need a trustworthy, localized shopping experience and a secure platform they can rely on,  
**the** ToolShop **is** a professional tools webshop  
**that** opens the Czech market with native language support, local payments and delivery, and a frictionless checkout — while hardening authentication, data access, and platform security in parallel.  
**Unlike** a generic international storefront with bolt-on localization,  
**our product** validates Czech market acceptance through marketplace integration (Alza.cz), PayU, and Zasilkovna delivery — and earns customer trust by fixing known security gaps and removing checkout blockers before scale.

---

## Strategic Context

After the planned China expansion was cancelled, ToolShop pivots to the Czech Republic as its next growth market. ToolShop HOLTES establishes the minimum viable foundation to test commercial potential in Czechia: localized customer experience, regional payment and logistics, and a security baseline that supports future scalability.

---

## Target Users

| User | Need |
|------|------|
| **Czech customers** | Browse, understand, and buy tools in Czech with familiar payment and delivery options |
| **Guest shoppers** | Complete checkout without abandoning the flow when not logged in |
| **Shop admins & security officers** | Operate a platform with hashed credentials and no critical auth vulnerabilities |
| **Data engineers** | Reliable, performant data access without mapping defects |

---

## Desired Future State

At the end of Sprint 6, ToolShop is ready to pilot the Czech market:

- Customers can switch the webshop to Czech and read navigation, UI, errors, and selected product content in their language.
- Czech customers can pay with PayU and choose delivery options suited to the region (including Zasilkovna).
- Delivery pricing is transparent: free standard shipping above €75, flat rate below, surcharges for oversized items, and express where applicable.
- Guests can log in during checkout and continue without restarting the purchase journey.
- Passwords are no longer stored in plain text; existing credentials are migrated to hashed storage.
- The vulnerable `xauth-X32` library is replaced with `xauth-X64`, closing ISEC-2499.
- ORMapper V1.2 is upgraded to V2.0, fixing the mapping bug and improving security and performance of data access.

---