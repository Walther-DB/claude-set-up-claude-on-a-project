# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project status

A small Express REST API for managing users (list, fetch, create), used as the starter project for the Claude Code course. Later course levels build on this same repo.

- Do not change the app code (`server.js`, `routes/`, `db/`, `tests/`) unless explicitly asked — the course work is the Claude setup around it.
- Environment: macOS with zsh; use BSD-compatible flags for tools like `sed` (`sed -i ''`).

## Commands

```bash
npm install                       # install dependencies
npm run dev                       # start the API on http://localhost:3000, restarts on file save
npm test                          # run all tests (Node's built-in runner, finds tests/*.test.js)
node --test tests/users.test.js   # run a single test file
npm run lint                      # ESLint over the whole repo; no output means clean
curl -s localhost:3000/health     # check a running server responds: {"status":"ok"}
```

Set the port with `PORT` (see `.env.example`); it defaults to 3000.

## Conventions

How to work with Walther
- **Language:** Dutch and English — match his usage in each message; British spelling for English.
- **Tone:** warm, direct, and honest — like a trusted colleague, not a consultant.
- **Technical depth:** non-technical language by default, but never strip out important technical details — explain it clearly instead.
- **Format:** concise prose over bullet-point walls; use structure only when it genuinely helps.
- **Options:** give a clear recommendation rather than a neutral list.
- **Ambiguity:** ask one clarifying question at a time.

Code
- Use CommonJS (`require` / `module.exports`), not ES modules — ESLint is set to `sourceType: "script"`.
- Use double quotes and semicolons, matching the existing files.
- Read and write data only through the functions in `db/store.js`; never touch the `users` array from a route.
- Send errors as JSON `{ "error": "<message>" }` with the right status: 400 for invalid input, 404 for not found, 201 for a successful create.
- Write tests with `node:test`, `node:assert` and `supertest`, not Jest. Import `app` from `server.js`; never start a real server in a test.

Git
- Never commit to `main` directly: work on a branch (e.g. `fix/...`, `feature/...`) and merge through a pull request.
- Run `npm test` and `npm run lint` before committing.

## Architecture

- `server.js` creates the Express app, adds JSON body parsing and mounts one router per resource (`/users`, `/health`). It only calls `listen()` when run directly (`require.main === module`) and exports `app`, so tests can use it without opening a port.
- `routes/` holds one file per resource, each exporting an `express.Router()`. A new resource gets its own file here, mounted in `server.js`.
- `db/store.js` is an in-memory stand-in for a database, seeded with two users. Data resets on every restart.
- `tests/users.test.js` covers the HTTP endpoints through `supertest`.
- `.github/workflows/ci.yml` runs `npm install`, `npm run lint` and `npm test` on Node 22 for every push and pull request.
- `.claude/settings.json` holds the shared permission rules (allows `npm test`; asks before `git push`; denies `rm -rf *`, `git push --force` and reading `.env`).

## Maintenance & health checks

Run when asked, or before opening a pull request:

1. `npm test` and `npm run lint` both pass.
2. `npm audit` reports 0 vulnerabilities; fix with `npm audit fix`, and if that is blocked, update the parent package within its `package.json` range.
3. Keep this file in step with the code: when a route, script or folder changes, update Commands and Architecture.
