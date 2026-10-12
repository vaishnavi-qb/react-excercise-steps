# Issue #1: Advanced Hooks and Lifecycle Management

You have the clothing-store repo. Do only the steps in this issue. Work on one branch.

## 1. Open this branch

From the repo root:

```bash
git switch main
git pull origin main
git switch -c exercise-01-advanced-hooks
```

If your default branch is not `main`, use that name instead of `main`.

## 2. Open these files

- `ecommerce-frontend/src/pages/ProductDetails.jsx`
- `ecommerce-frontend/src/components/Navbar.jsx`
- `ecommerce-frontend/src/pages/Home.jsx`

## 3. Do this

1. In `ProductDetails.jsx`, the `useEffect` that calls `api.get(\`/products/${id}\`)` can finish after the user has already opened a different product. Add a `cancelled` flag. Set it to `true` in the effect cleanup. After `await`, update state only when `cancelled` is still `false`. When `id` changes, set `product` back to `null` before the new request.
2. In `Navbar.jsx`, the `useEffect` that calls `api.get('/cart')` has the same problem. Use the same `cancelled` flag so a response after logout or unmount does not call `dispatch(setCart(...))`.
3. Create `ecommerce-frontend/src/hooks/useWindowEvent.js`. It takes an event name and a handler, calls `window.addEventListener`, and removes that listener in the cleanup. Use it in `Home.jsx` for the scroll listener that calls `handleLoadMore`. Delete the manual `addEventListener` / `removeEventListener` block in that effect.
4. Use `useWindowEvent` a second time in `ProductDetails.jsx`: listen for `keydown` and, when the key is `Escape` and the login modal is open, close the modal.

Do not change cart, login, or checkout behavior beyond the steps above.

## 4. Use these prompts

Copy one prompt at a time into your AI tutor. Read [the prompting guide](../PROMPTING.md) first. Ask for hints, not a full rewrite.

### Explore

```text
I am on branch exercise-01-advanced-hooks. Trace the product fetch in ecommerce-frontend/src/pages/ProductDetails.jsx from render through cleanup. Show me its dependencies and what happens if I open product A then product B before A returns. Ask me what I expect before you explain the current code. Do not edit files.
```

### Plan

```text
Help me add a cancelled flag to the ProductDetails and Navbar cart effects, then extract ecommerce-frontend/src/hooks/useWindowEvent.js for the Home scroll listener and ProductDetails Escape key. Ask me to predict the current behavior, then give one hint at a time. Do not write the implementation.
```

### Review

```text
Review my Exercise 01 diff. Check stale closures, cleanup on unmount and id changes, and that useWindowEvent is used in Home and ProductDetails. Identify missing tests. Do not edit code.
```

## 5. Check your work

```bash
npm run lint --prefix ecommerce-frontend
npm run build --prefix ecommerce-frontend
```

Then run the storefront (`npm run dev --prefix ecommerce-frontend`) and confirm:

- Open a product, then open another product quickly. The page shows the second product, not the first.
- Scroll the home page to the bottom. More products still load, and leaving the page does not throw a state-update warning.
- On a product page, open the login modal and press Escape. The modal closes.

## 6. Submit

```bash
git add ecommerce-frontend/src/pages/ProductDetails.jsx ecommerce-frontend/src/components/Navbar.jsx ecommerce-frontend/src/pages/Home.jsx ecommerce-frontend/src/hooks/useWindowEvent.js
git commit -m "Fix stale effects and share the window listener hook"
git push -u origin HEAD
```

Open a pull request into `main`. Wait until **Validate exercise** is green. Merge the pull request. Close this issue only after the merge.
