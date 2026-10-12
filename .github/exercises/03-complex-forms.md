# Issue #3: React Hook Form and Zod

Finish Exercise 02 and merge it before you start.

## 1. Open this branch

```bash
git switch main
git pull origin main
git switch -c exercise-03-complex-forms
```

## 2. Install the form libraries

From `ecommerce-frontend`:

```bash
npm install react-hook-form zod @hookform/resolvers
```

## 3. Open this file

- `ecommerce-frontend/src/pages/Checkout.jsx`

The address form in this file is the only form you change. Leave Login and Signup as they are.

## 4. Do this

1. Create `ecommerce-frontend/src/pages/checkoutSchema.js`. Export a Zod schema for the checkout address with these rules:
   - `firstName` and `lastName`: required, at least 2 characters
   - `email`: required, must be an email
   - `shippingAddress`, `city`, `state`, `country`: required
   - `phoneNumber`: required, exactly 10 digits
   - `zipCode`: exactly 6 digits when `country` is `India`; exactly 5 digits for every other country
   - `shippingMethod`: only `standard` or `express`
2. In `Checkout.jsx`, connect that schema with React Hook Form and `zodResolver`. Remove the manual `formData` / `setFormData` state for those fields. Keep saved-address loading: when a saved address is chosen, call `reset` with that address.
3. Show each field error in a `<p>` whose `id` matches the input `aria-describedby`. Put `aria-invalid` on the input when that field has an error.
4. Disable the pay button while the payment request is in progress so a second click cannot send a second request.
5. Do not call the payment API when the schema is invalid.

## 5. Use these prompts

Copy one prompt at a time. Read [the prompting guide](../PROMPTING.md) first.

### Explore

```text
I am on branch exercise-03-complex-forms. Trace the checkout address form in ecommerce-frontend/src/pages/Checkout.jsx from input to payment request. Identify where values, validation, errors, and loading state live today. Ask me which rules are client-side UX and which the server must also enforce. Do not edit files.
```

### Plan

```text
Help me design checkoutSchema.js and connect it with React Hook Form and zodResolver in Checkout.jsx. Ask me to list three valid and three invalid address examples first. Then help me reason through the India zip rule and aria-invalid one decision at a time. Do not generate the completed form.
```

### Review

```text
Review my Exercise 03 diff. Try empty, malformed zip for India, pending pay button, duplicate submit, and success. Check that errors use aria-describedby and that invalid data cannot reach the payment call. Do not edit code.
```

## 6. Check your work

```bash
npm run lint --prefix ecommerce-frontend
npm run build --prefix ecommerce-frontend
```

On `/checkout`, while logged in:

- Submit an empty form. Each required field shows its own error. No payment request is sent.
- Set country to `India` and zip to `12345`. The zip field shows an error.
- Set country to a non-India value and zip to `12345`. That zip error goes away.
- Click pay once with a valid form. The button stays disabled until the request finishes.

## 7. Submit

```bash
git add ecommerce-frontend/package.json ecommerce-frontend/package-lock.json ecommerce-frontend/src/pages/Checkout.jsx ecommerce-frontend/src/pages/checkoutSchema.js
git commit -m "Validate checkout with React Hook Form and Zod"
git push -u origin HEAD
```

Open a pull request into `main`. Merge it after **Validate exercise** is green. Then close this issue.
