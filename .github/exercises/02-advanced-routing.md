# Issue #2: Dynamic Routing and Protected Routes

Finish Exercise 01 and merge it before you start. Do only the steps below.

## 1. Open this branch

```bash
git switch main
git pull origin main
git switch -c exercise-02-advanced-routing
```

## 2. Open these files

- `ecommerce-frontend/src/App.jsx`
- `ecommerce-frontend/src/pages/Home.jsx`
- `ecommerce-frontend/src/pages/ProductDetails.jsx`
- `ecommerce-frontend/src/pages/Login.jsx`
- `ecommerce-frontend/src/components/Navbar.jsx`

## 3. Do this

1. Create `ecommerce-frontend/src/components/ProtectedRoute.jsx`. It reads `isAuthenticated` from the Redux auth state. If the user is not logged in, render `<Navigate to="/login" replace state={{ from: location.pathname + location.search }} />`. If they are logged in, render `<Outlet />`.
2. In `App.jsx`, put `/cart` and `/checkout` inside that protected route. Leave `/`, `/product/:id`, `/login`, `/signup`, and `/order-success` public.
3. In `Login.jsx`, after a successful login, go to `location.state?.from` when it is a path that starts with `/` and does not start with `//`. Otherwise go to `/`. Remove the old `redirectToProductId` branch.
4. In `Home.jsx`, stop reading the query string with `new URLSearchParams(location.search)`. Use `useSearchParams` from `react-router-dom`. Keep the existing `search` query parameter. When `search` is missing, show the normal catalog.
5. In `ProductDetails.jsx`, if the product request fails or returns no product, stop the loader and show the text `Product not found` plus a link back to `/`. Do not leave the previous product on screen.

## 4. Use these prompts

Copy one prompt at a time. Read [the prompting guide](../PROMPTING.md) first.

### Explore

```text
I am on branch exercise-02-advanced-routing. Trace how a URL becomes a rendered page in this app. Find the route definitions in App.jsx, the product detail page, and the auth state. Ask me what should happen for an unknown product and for a logged-out visit to /cart before explaining the current behavior. Do not edit files.
```

### Plan

```text
Help me add ProtectedRoute for /cart and /checkout, return to location.state.from after login, switch Home to useSearchParams, and show Product not found. Ask which state belongs in the URL and why. Give one hint at a time. Do not write the solution.
```

### Review

```text
Review my Exercise 02 diff. Check parameter parsing, missing product IDs, refresh/back/forward for ?search=, safe post-login redirects, and whether protected content can flash before auth is known. Identify test cases. Do not edit code.
```

## 5. Check your work

```bash
npm run lint --prefix ecommerce-frontend
npm run build --prefix ecommerce-frontend
```

In the running storefront:

- Visit `/cart` while logged out. You land on `/login`. After login you return to `/cart`.
- Visit `/product/this-id-does-not-exist`. You see `Product not found`.
- Visit `/?search=shirt`, refresh, then press the browser Back button. The search box and the product list match the URL.

## 6. Submit

```bash
git add ecommerce-frontend/src/App.jsx ecommerce-frontend/src/components/ProtectedRoute.jsx ecommerce-frontend/src/pages/Home.jsx ecommerce-frontend/src/pages/ProductDetails.jsx ecommerce-frontend/src/pages/Login.jsx
git commit -m "Protect cart and checkout and sync search with the URL"
git push -u origin HEAD
```

Open a pull request into `main`. Merge it after **Validate exercise** is green. Then close this issue.
