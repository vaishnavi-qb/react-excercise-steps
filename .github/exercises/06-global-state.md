# Issue #6: Context API, Zustand, and Redux Toolkit

Finish Exercise 05 and merge it before you start. Cart and auth already use Redux. Do not move them.

## 1. Open this branch

```bash
git switch main
git pull origin main
git switch -c exercise-06-global-state
```

## 2. Do this

1. Install Zustand from `ecommerce-frontend`:

```bash
npm install zustand
```

2. Create `ecommerce-frontend/src/theme/ThemeContext.jsx`. It stores `theme` as `light` or `dark`, and a `toggleTheme` function. Default is `light`. Wrap the app with this provider in `main.jsx`.
3. Add a **Theme** button in `ecommerce-frontend/src/components/Navbar.jsx` that calls `toggleTheme`. When `theme` is `dark`, set `document.documentElement.dataset.theme` to `dark`. When it is `light`, set it to `light`.
4. In `ecommerce-frontend/src/index.css`, add:

```css
:root[data-theme='dark'] {
  color-scheme: dark;
  background: #111;
  color: #f5f5f5;
}
```

5. Create `ecommerce-frontend/src/store/uiStore.js` with Zustand. It stores `isSearchOpen` (boolean) and `setSearchOpen`. Use it so the Navbar search form is hidden until the search icon is clicked, and hidden again when the form is submitted.
6. Create `ecommerce-frontend/src/STATE.md`. Use these headings and fill each with 4–8 lines about this repo:
   - What stays in Redux (auth and cart) and why you did not move it
   - What the theme Context owns
   - What the Zustand store owns
   - Why product lists stay in TanStack Query and are not copied into Redux or Zustand

## 3. Use these prompts

Copy one prompt at a time. Read [the prompting guide](../PROMPTING.md) first.

### Explore

```text
I am on branch exercise-06-global-state. Trace how cart and auth move through Redux today, and how the Navbar search input works. Ask me who owns each value and whether it is server-owned before explaining where a theme Context or a Zustand UI store would fit. Do not edit files.
```

### Plan

```text
Compare Context for theme, Zustand for isSearchOpen, and keeping auth/cart in Redux. Ask me to defend each choice for this repo. Help me outline ThemeContext and uiStore without writing the stores.
```

### Review

```text
Review my Exercise 06 diff and STATE.md. Check that auth and cart stayed in Redux, theme uses Context, search open state uses Zustand, and products were not copied into a global client store. Identify missing evidence. Do not edit files.
```

## 4. Check your work

```bash
npm run lint --prefix ecommerce-frontend
npm run build --prefix ecommerce-frontend
```

In the storefront:

- Click **Theme**. The page background switches and stays switched after you open a product.
- Click the search icon. The search form appears. Submit a search. The form closes and the URL has `?search=`.
- Add to cart still updates the cart count. Login still updates the navbar user name.

## 5. Submit

```bash
git add ecommerce-frontend/package.json ecommerce-frontend/package-lock.json ecommerce-frontend/src/theme/ThemeContext.jsx ecommerce-frontend/src/store/uiStore.js ecommerce-frontend/src/components/Navbar.jsx ecommerce-frontend/src/main.jsx ecommerce-frontend/src/index.css ecommerce-frontend/src/STATE.md
git commit -m "Add theme context and a small Zustand UI store"
git push -u origin HEAD
```

Open a pull request into `main`. Merge it after **Validate exercise** is green. Then close this issue.
