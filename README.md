# Shopify Featured Products — Infinite Scroll

#Live Preview
[Paste your Shopify preview link here]

#Approach & Logic

1. How featured vs non-featured products are loaded
- JavaScript fetches all products using `/collections/{handle}/products.json?limit=250`
- Products tagged `featured` are separated into a featured array (max 15)
- All other products go into a normal array
- Featured products are always rendered first in the grid

2. How infinite scroll works
- On first load: 15 featured + 5 normal = 20 products shown
- A scroll event listener watches when user reaches bottom (300px threshold)
- Next 20 normal products are appended each time

3. How duplicates are prevented
- A `displayedIds` Set tracks every product ID that has been fetched
- Before adding any product to featured or normal array, ID is checked against this Set

4. How it scales for large collections
- Uses `limit=250` (Shopify max) to minimize API calls
- Scans all pages until 15 featured are found, then stops early
- Client-side filtering and sorting avoids extra API requests

5. How filtering & sorting work
- Filter and sort dropdowns work on the already-fetched arrays in memory
- Featured products always stay on top unless a filter/sort is applied
- Clear filters button resets everything back to default view

6. Liquid limitations & solutions
- Liquid cannot loop all 100+ products in one pass due to pagination limits
- Solution: JavaScript API approach scans all pages to find all featured products
- `collection.handle` is passed from Liquid to JS to build the correct API URL