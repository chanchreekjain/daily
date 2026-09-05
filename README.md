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
