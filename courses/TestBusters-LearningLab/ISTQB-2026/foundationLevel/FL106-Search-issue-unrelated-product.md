# Search returns unrelated product for "Pliers" query

## Description
The search results for the query "Pliers" include an unrelated product: "Leather toolbelt". This item is not a plier product and should not appear as a main search result.

## Steps to Reproduce:

   - Open the Toolshop demo application Practice Software Testing (https://practicesoftwaretesting.com/) 
   - Search for "Pliers"
   -  Review the search results
   -  Click on "Leather toolbelt"

## Actual Behavior:
The product "Leather toolbelt" is displayed in the search results and opens its detail page, even though it is not a type of pliers.

## Expected Behavior:
Only relevant plier products should be shown for the "Pliers" search query. Unrelated products such as tool belts should be excluded or moved to a clearly marked related-products section.

## Possible Cause:
The product description contains the word "pliers", which may cause a false positive in the full-text search.

## Risks
- Relevant products might be excluded if filtering is too strict
- Changes in search logic could impact other search queries
- Possible regression in existing search behavior
- Performance impact if filtering or ranking becomes more complex
- Dependency on search/indexing implementation

## Technical Considerations 
- Review search query logic (full-text search vs. structured fields like product name and category)
- Prioritize exact matches in product name (e.g. "pliers") over partial matches in description
- Implement category-based filtering for search results (e.g. only show products from "Pliers" category)
- Ensure that filters (category, price range) are correctly applied together with search queries
- Validate search indexing to avoid false positives (e.g. products mentioning "pliers" in description only)
- Align backend search response with frontend display (no mismatch between API and UI results)
- Add stable attributes (e.g. data-testid) to product cards for reliable automation
- Ensure consistent behavior when combining search + filters (e.g. "pliers" + category filter)
- Handle edge cases like partial matches or similar tools (e.g. bolt cutters vs. pliers)
- Log or trace search results to understand why a product is included

## Business Value
- Better user experience (users find correct products faster)
- Increased trust in search functionality
- Reduced confusion caused by irrelevant results
- Improved product quality perception
- Faster and more reliable automated testing

## Environment:
- Application: Toolshop Demo
- Feature: Product search
- Browser: Not specified

## Acceptance Criteria:

- When a user searches for "Pliers", only products that are actual pliers are shown in the main search results
- Products that are not pliers (e.g. tool belts) are not included in the main search results
- Search results are based on relevant product attributes (e.g. product name, category), not only description text
- If unrelated products are shown, they must be clearly marked as "Related products" or similar
- The number of search results matches the visible relevant products
- Clicking on a search result always opens the correct product detail page


## Screenshots

<img width="1000" height="1000" alt="Image" src="https://github.com/user-attachments/assets/4dcb8432-b6e5-4ab3-8ac9-b36741903fdb" />


<img width="1000" height="600" alt="Image" src="https://github.com/user-attachments/assets/f3f2b49c-bb23-481e-9af9-933e33271a9f" />