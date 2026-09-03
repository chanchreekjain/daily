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