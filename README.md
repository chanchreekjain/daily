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