# Issue #8: Performance Optimization

Finish Exercise 07 and merge it before you start. Change one list. Do not memoize the whole app.

## 1. Open this branch

```bash
git switch main
git pull origin main
git switch -c exercise-08-performance
```

## 2. Open this file

- `ecommerce-frontend/src/pages/Home.jsx`

## 3. Do this

1. Create `ecommerce-frontend/src/components/ProductCard.jsx` (or `.tsx` if you prefer). Move the single product card markup out of `Home.jsx` into this component. Props are the product and `onOpen`.
2. Wrap `ProductCard` in `React.memo`.
3. In `Home.jsx`, stop passing an inline `() => handleProductClick(product)` on every render. Pass `handleProductClick` and let `ProductCard` call it with the product id.
4. Create `ecommerce-frontend/src/PERFORMANCE.md` and fill these lines with your own numbers:
   - Interaction you measured: scroll the home catalog until "load more" runs once
   - Tool: React DevTools Profiler
   - Before: commit SHA, how many `Home` or card renders you saw for that one scroll
   - Change: `ProductCard` extracted and memoized
   - After: same scroll, same machine, the new render count
   - What you did not memoize, and why

Measure before you add `memo`, then measure again after. Use the same page, the same scroll, and a production build (`npm run build` and `npm run preview --prefix ecommerce-frontend`).

## 4. Use these prompts

Copy one prompt at a time. Read [the prompting guide](../PROMPTING.md) first.

### Explore

```text
I am on branch exercise-08-performance. Help me form a measurable hypothesis about why scrolling the home catalog and loading more products causes extra renders. Ask what I observed in React DevTools Profiler before inspecting Home.jsx. Do not suggest memoization yet. Do not edit files.
```

### Plan

```text
Given my before profile numbers, help me decide whether extracting ProductCard and wrapping it in React.memo is the smallest useful change. Ask me to predict the after render count. Do not edit files.
```

### Review

```text
Review my Exercise 08 diff and PERFORMANCE.md. Compare the same before/after scroll. Check that ProductCard is memoized and onOpen is stable. Challenge whether memo reduced measured work or only added complexity. Do not edit code.
```

## 5. Check your work

```bash
npm run lint --prefix ecommerce-frontend
npm run typecheck --prefix ecommerce-frontend
npm run build --prefix ecommerce-frontend
```

Clicking a product still opens `/product/:id`. Search and category pages still list the right products.

## 6. Submit

```bash
git add ecommerce-frontend/src/pages/Home.jsx ecommerce-frontend/src/components/ProductCard.jsx ecommerce-frontend/src/PERFORMANCE.md
git commit -m "Memoize the home product card after a profiler pass"
git push -u origin HEAD
```

If the card file is `.tsx`, add that path instead. Open a pull request into `main`. Merge it after **Validate exercise** is green. Then close this issue.
