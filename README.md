# Advanced React Architecture Lab

[![Copy the exercise](https://img.shields.io/badge/COPY%20THE%20EXERCISE-%E2%86%92-2ea44f?style=for-the-badge&logo=github&logoColor=white)](https://github.com/vaishnavi-qb/ecommerce-shopping-store/generate)

An issue-driven learning track for production React architecture. It uses a Vite/React storefront and Express/MongoDB backend as the learning codebase; the exercises progressively introduce advanced architecture without replacing the app in advance.

## Start the track

You are looking at the starter repo. Do the steps in order. Each exercise is one branch and one pull request.

1. Click **Copy the exercise** and create the repository under your GitHub account.
2. Clone that new repository to your machine. Open a terminal in the repo root.
3. On GitHub, open the **Actions** tab and run **Initialize exercises** on `main`. Wait until it is green.
4. Open the **Exercise 01** issue. Follow it from the top. It tells you the branch name, the files to open, the edits, three AI tutor prompts (Explore, Plan, Review), the commands to run, and the commit command.
5. Push that branch and open a pull request into `main`. Wait until **Validate exercise** is green. Merge the pull request. Then close the issue.
6. The next issue appears after that merge is validated and the current issue is closed. Repeat step 4 on the new issue. Always start the new branch from an updated `main`:

```bash
git switch main
git pull origin main
git switch -c exercise-0N-short-name
```

7. Stop at Exercise 10. Closing it finishes the track. There is no Exercise 11.

The same instructions are in `.github/exercises/`. If a step is unclear, follow the issue, not this page.

## How automation works

The workflows use the repository-provided `GITHUB_TOKEN`; no personal access token or external service is needed.

| Workflow | Trigger | What it does |
| --- | --- | --- |
| `Initialize exercises` | Default-branch push or manual dispatch | Reads Exercise 01 and creates it as a GitHub issue if it does not already exist. |
| `Validate exercise` | Push, pull request, or manual dispatch | Installs dependencies, checks backend JavaScript syntax, lints the frontend, tests the workflow scripts, and builds the Vite frontend. |
| `Advance exercise` | Exercise issue closed or validation workflow completes | Checks completion order and the latest default-branch validation, then creates the next issue once. |

### Progression rules

- Every generated issue has a numbered title and hidden exercise marker. The marker lets the scripts find the same issue reliably even if its title changes.
- Initialization and advancement check for an existing issue before creating one. Advancement is serialized so simultaneous events do not create duplicates.
- An issue can advance only when it is closed, all earlier exercises are closed, no next exercise already exists, and **Validate exercise** succeeded for the current default-branch commit.
- Pull request checks are useful before merge, but they do not count as validation of the default branch. After merging, the default-branch push runs validation. A failed check never advances the track.
- Closing an issue does not bypass failed checks. A later successful validation on the latest default-branch commit can complete the pending transition.

### Script responsibilities

- `.github/scripts/exercise-workflow.mjs` contains the shared Markdown parser, issue-title formatter, exercise paths, and progression eligibility rule.
- `.github/scripts/github-api.mjs` makes authenticated GitHub API requests, lists issues, ensures labels, and creates an issue only if its title or marker is not already present.
- `.github/scripts/initialize-exercises.mjs` creates the first issue from `01-advanced-hooks.md`.
- `.github/scripts/advance-exercise.mjs` verifies issue order and current-commit validation, then creates the next issue or marks the track complete.
- `.github/scripts/exercise-workflow.test.mjs` tests parsing, title formatting, and progression gates without requiring GitHub credentials.

The Actions token needs read access to repository contents and workflow runs, plus write access to issues. Check **Settings → Actions → General → Workflow permissions** if issue creation fails. The automation never commits code or closes an exercise issue for you; you submit your work and close the issue after meeting its criteria.

## Prompt-based learning

Each issue has three copy-ready prompts under **Use these prompts**:

1. **Explore** — understand the current code before you change it
2. **Plan** — get one hint at a time for the steps in that issue
3. **Review** — check your diff against the issue after you implement

Use them in that order. You write the code. The guide is [.github/PROMPTING.md](.github/PROMPTING.md). Do not paste credentials, tokens, cookies, or `.env` contents into a prompt.

## Track

Do these in order. The branch name is what you pass to `git switch -c`.

| Issue | Branch | What you do |
| --- | --- | --- |
| 01 | `exercise-01-advanced-hooks` | Stop stale product and cart responses. Share one window-listener hook. |
| 02 | `exercise-02-advanced-routing` | Protect `/cart` and `/checkout`. Read search from the URL. Show a missing product. |
| 03 | `exercise-03-complex-forms` | Validate the checkout address with React Hook Form and Zod. |
| 04 | `exercise-04-authentication-cors` | Move the JWT into an HTTP-only cookie and allow only `CLIENT_URL`. |
| 05 | `exercise-05-tanstack-query` | Load the catalog and product with TanStack Query. |
| 06 | `exercise-06-global-state` | Add a theme Context and a small Zustand UI store. Leave cart and auth in Redux. |
| 07 | `exercise-07-typescript-react` | Type the product page and protected route. Run `tsc` in CI. |
| 08 | `exercise-08-performance` | Measure the home list, then memoize the product card. |
| 09 | `exercise-09-nextjs` | Add a separate Next.js app that serves one product page. |
| 10 | `exercise-10-capstone` | Write the architecture note and confirm the shopper path still works. |

## Local development

Requirements: Node.js 22 or later and npm. The backend also needs the environment variables and services documented in [the backend README](ecommerce-backend/README.md).

```sh
npm ci
npm ci --prefix ecommerce-frontend
npm run start
```

Run the frontend separately with `npm run dev --prefix ecommerce-frontend`. Checks used by CI:

```sh
npm test
npm run lint --prefix ecommerce-frontend
npm run build --prefix ecommerce-frontend
```

The starter application is JavaScript, so CI checks backend syntax, frontend lint, workflow tests, and the production build. Exercise 07 adds strict TypeScript checking, and Exercise 09 adds a separate Next.js build.

The validation workflow uses Node.js 22 and installs the Linux Rolldown binding required by the current Vite toolchain. Later exercises should extend the workflow when they add a TypeScript project or a separate Next.js app so those new checks are part of the same progression gate.
