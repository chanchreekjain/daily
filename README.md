# Daily Log

What I learn, one commit a day.

## 2026-09-01 — Day 1

Set up git and made my first repository.

Learned:
- `git config --global` — set name and email so commits get credited to me
- `git init` — turn a folder into a repository (creates the hidden `.git` folder)
- `git status` — shows the current state; run it constantly
- `git add` — move a file to the staging area, the shortlist for the next snapshot
- `git commit -m` — save a permanent snapshot with a message


## 2026-09-02 — Day 2

Scaffolded the jobtrail Next.js project and pushed it.

Learned:
- `npx` runs a tool once and throws it away; `npm install` is for things you keep
- `git log --oneline` — compact history; `HEAD -> main` means "you are here"
- `.gitignore` — patterns git refuses to track, most importantly `.env*` for secrets
- `git rm --cached` — stop tracking a file without deleting it from disk


## 2026-09-03 — Day 3

Deployed jobtrail to Vercel. Every push to main now auto-deploys.

Learned:
- Vercel needs its GitHub app installed to see your repos
- `npm run dev` — local dev server on localhost:3000
- `page.tsx` — Next.js renders whatever it exports as `default`
- JSX uses `className`, not `class`



## 2026-09-04 — Day 4

Connected jobtrail to Postgres (Neon). Homepage now reads live data.

Learned:
- `.env.local` holds secrets locally; production env vars live in Vercel separately
- SQL: `create table`, `uuid primary key`, `not null`, defaults, `timestamptz`
- A `.sql` file is just text — something has to run it
- Server Components can `await` a query directly, no API route needed
- `git add` ≠ commit — push sends commits, not staged changes
- Read the error message: "Connection string: DATABASE_URL" told us the value was wrong, not missing



## 2026-09-05 — Day 5

Built UI Dissect from scratch — a Chrome extension that extracts and isolates modern UI styling on hover — and submitted it to the Microsoft Edge Add-ons store.

Learned:
- `e.composedPath()` vs `elementFromPoint` — `elementFromPoint` gets blocked by extension overlays; `e.composedPath()` pierces open Shadow Roots to find the real element under the cursor
- Event lifecycles in async frames — reading event properties inside `requestAnimationFrame` returns empty arrays because the browser dispatches and clears the `MouseEvent` synchronously
- Event capture (`useCapture: true`) — listening on the capture phase catches keystrokes (like `Space` to freeze) before complex sites like GitHub can intercept and consume them
- Self-isolation in Shadow DOM — hosting extension UI in an isolated ShadowRoot with `pointer-events: none` prevents the inspector from inspecting its own highlight box
- The computed styles trap — `window.getComputedStyle` dumps 300+ default properties and returns empty strings for shorthands; extracting the visual "DNA" requires targeted filtering
- Git tracking vs `.gitignore` — adding files to `.gitignore` after they are committed does nothing; you have to run `git rm -r --cached` to purge them from the remote index
- Extension packaging — store submission zips require `manifest.json` at the absolute root of the archive; PowerShell's `Compress-Archive` builds standard ZIPs in Windows terminal
- Store data disclosures — in browser store policies, "data collection" strictly refers to transmitting data off-device, not reading DOM nodes in-memory



## 2026-09-06 — Day 6

Published both ContextFreeze and UI Dissect to the Microsoft Edge Add-ons Store, taking two extensions from local development to public production listings.

Learned:
- Cross-browser compatibility — Chromium extensions (Manifest V3) run on Microsoft Edge without modifying a single line of extension logic or manifest code
- Store review economics — the Microsoft Edge Partner Center has no developer registration fee (unlike the Chrome Web Store's $5 fee), making it the fastest free path to a public 1-click store link
- Reviewer certification notes — providing concise, reproduction-style testing steps in the submission notes prevents manual certification review flags and back-and-forth rejections
- Store listing asset requirements — store storefronts require specific aspect ratios: 300×300 1:1 logos, 440×280 and 1400×560 promotional banners, and pixel-exact 1280×800 screenshots
- Decoupling software releases from store reviews — developers have three tiers of distribution: cloning source (`npm run build`), downloading standalone `.zip` releases from GitHub Releases, or 1-click installs via official browser store channels
- GitHub Pages as compliance infrastructure — serving a clean `docs/index.html` from the `main` branch provides an instant, free, zero-maintenance HTTPS privacy policy URL required by store reviewers



## 2026-09-07 — Day 7 (Part 1: Kadane's Algorithm Across Languages)

Implemented Best Time to Buy and Sell Stock and explored its relationship to Kadane’s Algorithm across C++, Java, and Python3.

Revised:
- The Kadane invariant — at each step, you decide whether to extend the current local tracking window or discard it and reset; tracking minimum buy price is effectively tracking the minimum prefix of the running price series
- $O(n)$ time & $O(1)$ space optimization — replacing the naive $O(n^2)$ pairwise comparison with a single-pass greedy scan that updates `min_price` and `max_profit` simultaneously
- Cross-language nuances:
  - **C++**: using `std::max` / `std::min` with `INT_MAX` from `<climits>`; passing vectors by `const &` to avoid $O(n)$ copy overhead
  - **Java**: using `Math.max()` with primitive `int` arrays to prevent wrapper boxing/unboxing overhead
  - **Python3**: using `float('inf')` for initialization and idiomatic single-loop traversal (`for price in prices:`) without index lookups

## 2026-09-07 — Day 7 (Part 2: Distinct Subsequences DP)

Solved the Distinct Subsequences problem in C++, analyzing string matching states, recurrence transitions, and memory compression.

Learned:
- DP state definition — `dp[i][j]` represents the number of distinct subsequences of string `s[0...i-1]` that match target `t[0...j-1]`
- Recurrence transitions:
  - If `s[i-1] == t[j-1]`: `dp[i][j] = dp[i-1][j-1] + dp[i-1][j]` (summing the decision to match the current character with the decision to skip it to search for other matches)
  - If `s[i-1] != t[j-1]`: `dp[i][j] = dp[i-1][j]` (must skip current character in `s`)
- Base case semantics — `dp[i][0] = 1` for all `i`, because an empty target string can always be formed exactly once by deleting all remaining characters
- Memory compression — reducing space from a 2D $O(m \times n)$ table to a 1D $O(n)$ array by iterating the inner loop backwards to avoid overwriting values needed for the current transition
- Integer overflow handling — large test cases exceed standard 32-bit signed integers, requiring `unsigned long long` or modulo clamping in C++ to prevent undefined runtime behavior




## 2026-09-08 — Day 8

Added the write path to jobtrail — a form that inserts into Postgres.

Learned:
- Repository pattern: SQL lives in `repo.ts`, pages just call functions
- `"use server"` + `<form action={fn}>` — server actions, no API route needed
- `${}` in SQL templates sends values as parameters, not text — that's what blocks SQL injection
- `revalidatePath` refreshes cached data after a write
- `turbopack.root` — Next.js was scanning my whole home folder and running out of memory


## 2026-09-08 — Day 8 (DSA)

Solved two problems.

- **Longest substring without repeating characters** — sliding window with a set/map of seen characters; expand right, shrink left on a repeat.
- **Commas in range** — worked through the range logic and formatting.



## 2026-09-09 — Day 9

Added AI to jobtrail — paste a job description, get its requirements back as structured JSON.

Learned:
- `responseSchema` constrains the model's output shape — it can't return prose or skip a field
- API keys live in `.env.local` locally and in Vercel's env vars for production, never in the repo
- The folder path under `src/app/` is the URL — `app/jd/page.tsx` → `/jd`
- `console.log` in a server action prints to the terminal, not the browser
