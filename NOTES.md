# Notes

## CLAUDE.md: what I kept and what I cut

I started from the `/init` draft and kept only what helps Claude work on this project without asking questions:

- **Description**: one line on what the API is.
- **Commands**: `npm run dev`, `npm test`, `npm run lint`, plus how to run a single test file or filter by test name, since these are the commands I actually use.
- **Conventions**: rules Claude could easily get wrong on its own: CommonJS only (no ES `import`), tests with `node:test` + `supertest` (no Jest/Mocha), the error response format `{ error: "<message>" }`, and config only through env vars with new keys added to `.env.example`.
- **Architecture**: `server.js` as the entry point that exports `app` (so tests don't open a port), one router per resource in `routes/`, and all data access through `db/store.js`.

I deliberately left out the generic intro line that `/init` adds, the note that this is a course starter project, and the pointer to the CI workflow. Claude either knows these already or can see them in the repo, and they would only take up context. There are no secrets, no contents of `.env`, no pasted docs and no one-off tasks in the file.

## Permission rules

- **allow** `Bash(npm test:*)`: running tests is safe and frequent, so there is no reason to confirm it every time.
- **ask** `Bash(git push:*)`: pushing changes the remote, so I want to review it each time.
- **deny** `Read(./.env)`: `.env` holds real secrets. Without this rule Claude could read credentials into the conversation.
- **deny** `Bash(git push --force:*)`: a force-push can overwrite remote history and other people's work, which is hard to undo.

While checking `/permissions` I also found a `PowerShell` deny rule I had added by accident. On Windows it would block Claude from running any command, so I removed it.

## Verification

- `claude --version` works and I'm signed in.
- In a fresh session, `/memory` opened `./CLAUDE.md`, so the file is loaded.
- `/permissions` lists my allow, ask and deny rules.
- Asked "How do I run the tests here?", Claude answered `npm test` and the single-file and name-filter commands, citing CLAUDE.md.
- Asked "read the .env file", Claude was blocked by the deny rule and offered `.env.example` instead.
