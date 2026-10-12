# Issue #9: Moving from Vite to Next.js

Finish Exercise 08 and merge it before you start. Leave the Vite app in `ecommerce-frontend` working.

## 1. Open this branch

```bash
git switch main
git pull origin main
git switch -c exercise-09-nextjs
```

## 2. Create the Next.js app

From the repo root:

```bash
npx create-next-app@latest ecommerce-next --js --app --eslint --no-tailwind --no-src-dir --import-alias "@/*"
```

Use the current stable Next.js. Do not put this app inside `ecommerce-frontend`.

## 3. Do this

1. Copy only the product-details feature. Add `ecommerce-next/app/product/[id]/page.js`.
2. That page is a Server Component. It fetches `process.env.API_PUBLIC_URL + '/api/products/' + id` on the server. If the product is missing, call `notFound()`.
3. Add `ecommerce-next/app/product/[id]/AddToCartButton.js` with `'use client'` at the top. The button is the only client component. It does not fetch the product.
4. Add `ecommerce-next/README.md` with these exact headings and fill them in:
   - How to run (`npm install` and `npm run dev` inside `ecommerce-next`)
   - Env vars (`API_PUBLIC_URL` only; no secret values)
   - What is rendered on the server (the product page)
   - What is a client component (the add-to-cart button) and why
5. In `.github/workflows/validate-exercise.yml`, add a step after the Vite build:

```yaml
      - name: Build Next.js app
        working-directory: ecommerce-next
        run: |
          npm ci
          npm run build
        env:
          API_PUBLIC_URL: http://localhost:3000
```

Commit `ecommerce-next/package-lock.json`.

## 4. Use these prompts

Copy one prompt at a time. Read [the prompting guide](../PROMPTING.md) first.

### Explore

```text
I am on branch exercise-09-nextjs. Trace the product detail feature in the Vite app: route entry, data fetch, and add-to-cart. Ask me which parts need the browser before explaining possible Server and Client Component boundaries for ecommerce-next. Do not edit files.
```

### Plan

```text
Help me plan the smallest Next.js app in ecommerce-next that keeps ecommerce-frontend working. Ask me why the product page fetches on the server and why AddToCartButton is a client component. Give hints about env API_PUBLIC_URL without generating the app.
```

### Review

```text
Review my Exercise 09 diff. Check Server/Client boundaries, notFound for missing products, Vite still builds, Next.js CI build step, and README headings. Point out unsupported assumptions. Do not edit files.
```

## 5. Check your work

```bash
npm run build --prefix ecommerce-frontend
npm ci --prefix ecommerce-next
npm run build --prefix ecommerce-next
```

Vite `npm run dev --prefix ecommerce-frontend` still opens the existing store. `npm run dev --prefix ecommerce-next` opens the product page at `/product/<a real id>` and shows the product name without a client-side fetch in the Network tab.

## 6. Submit

```bash
git add ecommerce-next .github/workflows/validate-exercise.yml
git commit -m "Serve one product page from a separate Next.js app"
git push -u origin HEAD
```

Open a pull request into `main`. Merge it after **Validate exercise** is green. Then close this issue.
