# Notes

## What I put in CLAUDE.md, and what I left out

I started from the course's five headings: Project status, Commands, Conventions, Architecture, and Maintenance & health checks. I later removed the Maintenance & health checks section, along with a macOS-specific shell note, so four headings remain. Each section only holds what Claude can't work out quickly from the code, or would otherwise ask me about.

**Kept in:**
- **A one-line description** of the project, plus the rule not to change the app code unless asked, because the course work is the Claude setup around it.
- **Commands ready to copy and paste**, including the less obvious ones: running a single test file, and a `curl` health check against a running server.
- **Conventions a linter can't enforce:** CommonJS instead of ES modules, data access only through `db/store.js`, the JSON error format with status codes, `node:test` and `supertest` instead of Jest, and working on a branch with a pull request rather than committing to `main`.
- **A short architecture map:** the `server.js` entry point, one router per resource in `routes/`, the in-memory store, the tests, CI and the permission settings.

**Moved out of the project:** my working preferences (language, tone, one question at a time) now live in my personal `~/.claude/CLAUDE.md`. Claude Code loads that file in every project on my Mac, so sessions still start the way I like to work, but it isn't committed. The project's `CLAUDE.md` stays about the code and doesn't push my preferences onto anyone else who works on the repo.

**Left out:**
- **Generic Claude Code commands** like `/exit` and `/clear`. They're the same in every project and don't help Claude work on this one.
- **Anything obvious from the code**, such as the list of endpoints or the seed users, which would only go out of date.
- **One-off tasks**, like the npm security fix I did once, and **anything sensitive**: no secrets or tokens.

**One fix along the way:** I renamed `Claude.md` to `CLAUDE.md`. macOS doesn't care about capital letters in file names, but Linux does, and that includes CI and a reviewer's machine.

## Which permission rules I added, and why

`.claude/settings.json` is committed so everyone working on the repo gets the same rules:

- **Allow `npm test`:** it's safe, read-only and run constantly, so asking for permission every time is just friction.
- **Deny `rm -rf *`:** a recursive delete can wipe out work that isn't committed yet, and it can't be undone.
- **Deny reading `.env`:** that file holds local secrets, and anything Claude reads ends up in the conversation. This rule only covers Claude's Read tool, so a shell command like `cat .env` isn't blocked by it. It's a guard against accidents, not a lock.
- **Ask before `git push`:** a push puts work on GitHub where others can see it, so I want to approve each one. Commits stay local and easy to undo, so they don't need this.
- **Deny `git push --force`:** a force push overwrites history on GitHub and can wipe out other people's commits. Deny beats ask, so this is blocked outright even though ordinary pushes only need my approval. The rule matches the command's start, so `git push -f` or `git push origin main --force` aren't caught by it. Those still fall under the ask rule, though, so I'd see them before they ran.

**What could go wrong without the deny rule:** Claude could delete a folder recursively. The command might be well meant but aimed at the wrong place, or a cleanup step inside a longer command. This actually happened: while installing the GitHub CLI, Claude chained `rm -rf extracted` into a long install command, and the deny rule blocked the whole command before any of it ran. Claude then redid the install as small, separate steps with no delete.

## Verification

- [x] `claude --version` runs; Claude Code is installed and signed in.
- [x] `npm test` (4/4) and `npm run lint` pass on `main`.
- [x] `/memory` shows `CLAUDE.md` loaded as the project memory, so its instructions reach every new session. Claude's auto-memory folder for this project is still empty, which is expected: nothing has been worth saving between sessions yet.
- [ ] `/memory` also lists my personal `~/.claude/CLAUDE.md` with my working preferences (check in a fresh session).
- [x] `/permissions` shows the shared rules from `.claude/settings.json`: `Bash(npm test)` under Allow, `Bash(git push:*)` under Ask, and `Bash(rm -rf *)`, `Bash(git push --force:*)` and `Read(./.env)` under Deny. There's no `.claude/settings.local.json`, so no personal rules override them.
