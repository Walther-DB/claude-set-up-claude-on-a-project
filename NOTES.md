# Notes

## What I put in CLAUDE.md, and what I left out

I started from the course's five headings: Project status, Commands, Conventions, Architecture, and Maintenance & health checks. Each section only holds what Claude can't work out quickly from the code, or would otherwise ask me about.

**Kept in:**
- **A one-line description** of the project, plus the rule not to change the app code unless asked, because the course work is the Claude setup around it.
- **Commands ready to copy and paste**, including the less obvious ones: running a single test file, and a `curl` health check against a running server.
- **Conventions a linter can't enforce:** CommonJS instead of ES modules, data access only through `db/store.js`, the JSON error format with status codes, `node:test` and `supertest` instead of Jest, and working on a branch with a pull request rather than committing to `main`. I also kept my own working preferences (language, tone, one question at a time), so every session starts the way I like to work.
- **A short architecture map:** the `server.js` entry point, one router per resource in `routes/`, the in-memory store, the tests, CI and the permission settings.

**Left out:**
- **Generic Claude Code commands** like `/exit` and `/clear`. They're the same in every project and don't help Claude work on this one.
- **Anything obvious from the code**, such as the list of endpoints or the seed users, which would only go out of date.
- **One-off tasks**, like the npm security fix I did once, and **anything sensitive**: no secrets, tokens or `.env` values.

**Two fixes along the way:** I renamed `Claude.md` to `CLAUDE.md`. macOS doesn't care about capital letters in file names, but Linux does, and that includes CI and a reviewer's machine. I also found that `.gitignore` didn't actually ignore `.env`. Its lines started with spaces, which git treats as part of the pattern, so I removed them.

## Which permission rules I added, and why

`.claude/settings.json` is committed so everyone working on the repo gets the same rules:

- **Allow `npm test`:** it's safe, read-only and run constantly, so asking for permission every time is just friction.
- **Deny `rm -rf *`:** a recursive delete can wipe out work that isn't committed yet, and it can't be undone.

**What could go wrong without the deny rule:** Claude could delete a folder recursively. The command might be well meant but aimed at the wrong place, or a cleanup step inside a longer command. This actually happened: while installing the GitHub CLI, Claude chained `rm -rf extracted` into a long install command, and the deny rule blocked the whole command before any of it ran. Claude then redid the install as small, separate steps with no delete.

## Verification

- [x] `claude --version` runs; Claude Code is installed and signed in.
- [x] `npm test` (4/4) and `npm run lint` pass on `main`.
- [x] `/memory` shows `CLAUDE.md` loaded as the project memory, so its instructions reach every new session. Claude's auto-memory folder for this project is still empty, which is expected: nothing has been worth saving between sessions yet.
- [x] `/permissions` shows the shared rules from `.claude/settings.json`: `Bash(npm test)` under Allow and `Bash(rm -rf *)` under Deny. There's no `.claude/settings.local.json`, so no personal rules override them.
