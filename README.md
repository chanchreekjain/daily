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


## 2026-09-09 — Day 9 (debugging)

Production threw a 500 on the JD extraction while local worked fine.

Learned:
- Vercel runtime logs carry the actual exception; the browser only shows a generic error page
- 503 UNAVAILABLE was the model being overloaded — someone else's problem, not a bug in my code
- The same call took 8s, then 20s — the SDK retries internally, so slow can mean "retrying"
- External API calls need retry with backoff and a readable error, not a white 500 page


## 2026-09-09 — Day 9 (DSA)

- **Longest Repeating Character Replacement** (LeetCode 424) — solved in C++, Java and Python3.
- **Count Commas in Range II**




## 2026-09-10 — Day 10 

Made the JD extraction survivable and visible.

Learned:
- Retry with exponential backoff, but only on transient statuses (503, 429) — everything else rethrows
- Return errors as data instead of throwing; a thrown error is a white 500 page
- `"use client"` marks the browser/server boundary — only client components hold state
- `useActionState` gives you the action's return value plus an `isPending` flag for the loading state


## 2026-09-10 — Day 10 (DSA)

- **Minimum Window Substring** (LeetCode 76, Hard) — solved in C++, Java and Python3. Sliding window with a need/have count map; expand right until the window is valid, then shrink left while it stays valid, tracking the smallest.



## 2026-09-11 — Day 11

Released version 0.2.0 of UI Dissect to the Microsoft Edge Add-ons store, introducing a "Live Edit" tab and real-time WCAG contrast readouts without requesting any new manifest permissions.

Learned:
- `element.style.setProperty` for Live Editing — writing temporary inline CSS properties (background, border, font-size, shadow) directly to the frozen element allows for immediate, cosmetic sandbox tweaking.
- In-memory volatility as a security feature — style mutations are kept strictly local to the user's rendered page. They are never persisted to storage, transmitted over the network, or written back to the host site, meaning everything cleanly resets on page reload.
- Real-time accessibility checks — hooking the color controls to a contrast calculator enables the panel to instantly update the contrast ratio and WCAG AA/AAA compliance badges as the user drags the pickers.
- Reactive code generation — applying inline styles dynamically feeds back into the code generator, allowing the CSS, Tailwind, and React export tabs to reflect the live edits immediately (while also fixing two bugs from the 0.1.0 generator).
- Privacy policy mapping — even when a feature (like Live Edit) runs entirely offline without telemetry or remote code execution, store policies require the privacy policy to explicitly state that the temporary changes are local and never transmitted.



## 2026-09-12 — Day 12 (Weekend)

Kept things light today because even compilers deserve a day off. Just a couple of DSA problems to keep the streak alive without burning out.

Learned:
- **Min Stack** — solved in C++, Java, and Python3. Achieved O(1) minimum retrieval by keeping a parallel universe of minimums. Every time an element is pushed to the stack, it either goes onto a secondary stack or gets stored as a `(value, current_minimum)` pair, carrying a snapshot of history with it.
- **Maximum Score of Non-overlapping Intervals** — solved in C++. The algorithmic equivalent of schedule FOMO. Sorted the intervals by their end times, then used dynamic programming paired with binary search (`std::upper_bound` in C++) to constantly ask: "If I commit to this time block, what's the absolute maximum value I can still salvage from the non-overlapping past?"



## 2026-09-13 — Day 13 (Weekend)

Still rolling with the weekend LeetCode routine. Postfix notation and 2D matrix translations to keep the brain engaged before Monday.

Learned:
- **Evaluate Reverse Polish Notation** (C++, Java, Python3) — A classic stack workout where parentheses don't exist and order of operations is peacefully resolved. Numbers get pushed; operators pop the last two, evaluate, and push the result back. The real test is navigating cross-language quirks for string conversion and division: C++'s `std::stoi()`, Java's `Integer.parseInt()`, and Python's integer division handling for negative numbers (using `int(a / b)` to truncate toward zero instead of the standard `a // b` floor division).
- **Image Overlap** (C++) — Matrix manipulation that quickly turns into 2D vector math. Instead of physically simulating sliding one matrix over the other in a massive nested loop, you can just extract the 2D coordinates of all the `1`s in both images. By calculating the translation vector `(x_A - x_B, y_A - y_B)` between every pair of `1`s and counting their frequencies in a map, the most frequent translation vector instantly gives you the maximum possible overlap.



## 2026-09-14 — Day 14 (Ganesh Chaturthi Extended Cut)

The compiler got an extra day off for Ganesh Chaturthi, but the DSA grind never sleeps. Fueled by festive modaks, I tackled some backtracking and 2D collision physics to cap off the long weekend.

Learned:
- **Generate Parentheses** (C++, Java, Python3) — Welcome to the backtracking multiverse. The golden rule of the bracket club: you can only close `)` what you've already opened `(`. It’s pure state-space tree exploration. The real meta-game was handling the memory states across languages: exploiting C++'s pass-by-value branching, babysitting Java’s `StringBuilder` (append, recurse, delete!), and just letting Python glue immutable strings together like it’s magic.
- **Rectangle Overlap** (C++) — Basically building the collision detection for a primitive 2D game engine. The coolest trick here is "negative logic." Instead of doing heavy math to find the exact intersection zone, you just aggressively prove that the rectangles *missed* each other. If Box A is entirely to the left, right, above, or below Box B—they pass right through each other as ghosts. It turns a messy geometry problem into a razor-sharp, 4-condition $O(1)$ vibe check.




## 2026-09-16 — Day 15

Added a navbar to jobtrail.

Learned:
- `Link` does client-side navigation; `<a>` throws the whole page away and reloads
- The root layout wraps every page, so shared UI is written once
- `$_` in a double-quoted string gets eaten by bash before the other program sees it
- Turbopack's "memory allocation failed" was my laptop being full, not a code bug — 8 GB total, 280 MB free

## 2026-09-16 — Day 15(DSA)

Back to the weekday rhythm. Traded the weekend collision physics for time-traveling arrays and some seriously heavy state-machine dynamic programming. 

Learned:
- **Daily Temperatures** (C++, Java, Python3) — The quintessential "Monotonic Stack" initiation. Instead of looping forward like a rookie to find the next warm day, you stack up the indices of unresolved cold days. The moment a hot day arrives, it acts like a thermal trigger, popping all the colder days off the stack and resolving their wait times in one shot. It's essentially $O(N)$ time-traveling weather prediction.
- **Number of Sets of K Non-Overlapping Line Segments** (C++) — A dynamic programming boss fight. You have a line of points and need to draw `k` segments. Instead of brute-forcing boundary combinations, you build a 3D state machine: at any given point, are you currently *drawing* a segment, or *waiting* to start a new one? It turns a chaotic, overlapping geometry nightmare into a clean `dp[index][k][is_drawing]` table, proving once again that state machines can untangle any logic puzzle.



## 2026-09-17 — Day 16(DSA)

Mid-week grind. Swapped out 2D geometry for some sliding window DP and a literal traffic jam simulation.

Learned:
- **Car Fleet** (C++, Java, Python3) — Traffic jam physics modeled with arrays. Instead of simulating the cars moving frame-by-frame, you calculate each car's theoretical time to reach the destination and sort them by starting position. Working backward from the finish line, if a car is mathematically destined to arrive faster than the one ahead of it, it rear-ends it and forms a fleet bottlenecked by the slower speed. You basically just count the unbothered pace cars.
- **Find Two Non-overlapping Sub-arrays Each With Target Sum** (C++) — A sliding window that requires a rearview mirror. Finding one valid sub-array is easy, but finding two that don't overlap while minimizing their combined length is tricky. The galaxy-brain move is maintaining a `best_length_so_far` array. When your sliding window hits the target sum, you check your historical array for the best non-overlapping complement to your left. It turns a nested loop nightmare into a clean $O(N)$ sweep.




## 2026-09-18 — Day 16

jobtrail now saves extractions to Postgres and reuses them.

Learned:
- Foreign keys + `on delete cascade` — the database enforces consistency, not my code
- `returning id` hands back the row Postgres just created
- Hashing the input makes a reliable dedupe key; same JD twice costs one query, not one API call
- Migrations are written in the repo but run against the database separately
- "Insufficient system resources" from Turbopack means the laptop is full, not the code
