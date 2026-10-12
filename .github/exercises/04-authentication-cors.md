# Issue #4: Authentication, JWT, Cookies, and CORS

Finish Exercise 03 and merge it before you start. Do not commit `.env` or any real token.

## 1. Open this branch

```bash
git switch main
git pull origin main
git switch -c exercise-04-authentication-cors
```

## 2. Open these files

- `ecommerce-frontend/src/services/axios.js`
- `ecommerce-frontend/src/pages/Login.jsx`
- `ecommerce-frontend/src/App.jsx`
- `ecommerce-frontend/src/store/authSlice.js`
- `ecommerce-backend/server.js`
- `ecommerce-backend/controller/userController.js`

## 3. Do this

The backend already sets an HTTP-only cookie named `jwt` in `authUser` (`userController.js`) and clears it in `logoutUser`. `protect` in `authMiddleware.js` already reads `req.cookies.jwt`. Do not add a second cookie. Change only what is listed here.

1. In `ecommerce-backend/server.js`, stop using `origin: true`. Read allowed origins only from `CLIENT_URL` (comma-separated). If `CLIENT_URL` is missing, refuse to start and print `CLIENT_URL is required`.
2. In `authUser`, delete `token` from the JSON body. Keep `_id`, `name`, and `email`. Leave the `jwt` cookie options as they are.
3. In `authMiddleware.js`, delete the `Bearer` header fallback. A request is authenticated only when the `jwt` cookie is present and valid.
4. Add `GET /api/users/profile` in `userRoutes.js`, protected by `protect`. It returns `{ _id, name, email }` for `req.user`.
5. In `axios.js`, keep `withCredentials: true`. Delete the interceptor that reads `localStorage` and sets `Authorization`.
6. In `Login.jsx` and `App.jsx`, stop writing `token` or `user` to `localStorage`. After login, keep `name` and `email` in Redux only. On load, call `GET /api/users/profile`. If it succeeds, dispatch `restoreAuth` with the user and no token. If it returns 401, dispatch `logout`.
7. In `Navbar.jsx`, `handleLogout` must `await api.post('/users/logout')` before it clears Redux. Delete both `localStorage.removeItem` lines.
8. In `ecommerce-backend/README.md`, add a **Local auth env** section that lists only these names: `MONGO_URI`, `JWT_SECRET`, `CLIENT_URL`. Do not paste secret values.

## 4. Use these prompts

Copy one prompt at a time. Read [the prompting guide](../PROMPTING.md) first. Never paste `.env`, cookies, or tokens into the prompt.

### Explore

```text
I am on branch exercise-04-authentication-cors. Trace login in userController.js, the jwt cookie, authMiddleware.js, axios.js, App.jsx restoreAuth, and CORS in server.js. Explain where credentials travel and where they are stored today. Ask me which threat we reduce by removing the token from localStorage and the login JSON. Do not edit files.
```

### Plan

```text
Help me lock CORS to CLIENT_URL, remove token from the login JSON and localStorage, drop the Bearer fallback, add GET /api/users/profile, and call /users/logout from the Navbar. Ask me to decide the trusted origin and the 401 behavior. Do not configure it for me.
```

### Review

```text
Review my Exercise 04 diff. Check cookie name jwt, no token in JSON or localStorage, credentialed CORS without a wildcard, profile restore on load, and logout clearing the cookie. Suggest tests only. Do not edit code.
```

## 5. Check your work

```bash
npm test
npm run lint --prefix ecommerce-frontend
npm run build --prefix ecommerce-frontend
```

With the backend and frontend running:

- Log in. In the browser Application tab, the cookie name is `jwt` and it is HTTP-only. `localStorage` has no `token` key. The login JSON has no `token` field.
- Refresh the page. You are still logged in.
- Log out. The cookie is gone and `/cart` sends you to `/login`.
- A request from an origin that is not in `CLIENT_URL` is rejected by CORS.

## 6. Submit

```bash
git add ecommerce-backend/server.js ecommerce-backend/controller/userController.js ecommerce-backend/middleware/authMiddleware.js ecommerce-backend/routes/userRoutes.js ecommerce-backend/README.md ecommerce-frontend/src/services/axios.js ecommerce-frontend/src/pages/Login.jsx ecommerce-frontend/src/App.jsx ecommerce-frontend/src/components/Navbar.jsx
git commit -m "Store the session in an HTTP-only cookie and lock CORS to CLIENT_URL"
git push -u origin HEAD
```

Open a pull request into `main`. Merge it after **Validate exercise** is green. Then close this issue.
