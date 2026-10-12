# Issue #5: Server-State Management with TanStack Query

Finish Exercise 04 and merge it before you start.

## 1. Open this branch

```bash
git switch main
git pull origin main
git switch -c exercise-05-tanstack-query
```

## 2. Install the library

From `ecommerce-frontend`:

```bash
npm install @tanstack/react-query
```

## 3. Open these files

- `ecommerce-frontend/src/main.jsx`
- `ecommerce-frontend/src/pages/Home.jsx`
- `ecommerce-frontend/src/pages/ProductDetails.jsx`

## 4. Do this

1. In `main.jsx`, wrap the app in `QueryClientProvider`. Create the client in `ecommerce-frontend/src/queryClient.js` with `staleTime: 60_000` and `retry: 1`. Export that client.
2. In `Home.jsx`, delete the `useEffect` that calls `api.get` for the product list. Load the first page with `useQuery`. The query key must be `['products', { search: searchQuery, category }]`.
3. Keep "load more" as a `useInfiniteQuery` or as a `fetchNextPage` flow on that same key family. The Next button or the existing scroll loader must request the next `pageNumber` and append products. Show three different UI states: `Loading products...`, the error message from the failed request, and `No products found` when the list is empty.
4. In `ProductDetails.jsx`, load the product with `useQuery` and key `['product', id]`.
5. Change add-to-cart in `ProductDetails.jsx` to `useMutation`. On success, invalidate the `['cart']` query. Do not invalidate the product list.

Write the retry and stale-time choice in a 5-line comment at the top of `queryClient.js`.

## 5. Use these prompts

Copy one prompt at a time. Read [the prompting guide](../PROMPTING.md) first.

### Explore

```text
I am on branch exercise-05-tanstack-query. Trace the product list fetch in Home.jsx and the product fetch in ProductDetails.jsx. Ask me to classify each value as server state, local UI state, or global client state before explaining which part TanStack Query should own. Do not edit files.
```

### Plan

```text
Help me design query keys ['products', { search, category }] and ['product', id], plus a cart mutation that invalidates ['cart']. Ask what should happen after add-to-cart. Give one hint at a time about staleTime 60000 and retry 1. Do not generate the implementation.
```

### Review

```text
Review my Exercise 05 diff. Check key completeness, cache sharing when navigating back to Home, loading/error/empty states, and that product list is not invalidated on add-to-cart. Suggest test scenarios. Do not edit code.
```

## 6. Check your work

```bash
npm run lint --prefix ecommerce-frontend
npm run build --prefix ecommerce-frontend
```

In the storefront:

- Open Home, go to a product, then go back. The product list does not send a second request within 60 seconds (check the Network tab).
- Changing `?search=` does send a new request.
- Stop the backend and reload Home. You see the error message, not a blank page.
- Add a product to the cart. The navbar count updates after the mutation succeeds.

## 7. Submit

```bash
git add ecommerce-frontend/package.json ecommerce-frontend/package-lock.json ecommerce-frontend/src/main.jsx ecommerce-frontend/src/queryClient.js ecommerce-frontend/src/pages/Home.jsx ecommerce-frontend/src/pages/ProductDetails.jsx
git commit -m "Load products and cart updates with TanStack Query"
git push -u origin HEAD
```

Open a pull request into `main`. Merge it after **Validate exercise** is green. Then close this issue.
