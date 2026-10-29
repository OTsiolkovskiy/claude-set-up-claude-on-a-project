# CLAUDE.md

A small Express API (users + health endpoints) with an in-memory data store. 

## Commands

- `npm run dev` — start the API with `node --watch` on http://localhost:3000 (`PORT` env var overrides)
- `npm test` — run all tests (Node's built-in `node:test` runner, not Jest/Mocha)
- `node --test tests/users.test.js` — run a single test file
- `node --test --test-name-pattern="<name>"` — run tests matching a name
- `npm run lint` — ESLint (`eslint:recommended`)

## Conventions

- CommonJS only (`require` / `module.exports`); `sourceType` is `script`, so no ES `import`.
- Tests use `node:test` + `node:assert` + `supertest`; do not add another test framework.
- Unused `req`, `res`, `next`, or `_`-prefixed args are fine.
- Route handlers return errors as `res.status(<code>).json({ error: "<message>" })`.
- Config comes from environment variables; add new ones to `.env.example`, never commit `.env`.

## Architecture

- `server.js` builds the Express `app`, mounts routers (`/users`, `/health`), and exports `app`. It only calls `listen` when run directly (`require.main === module`), so tests import `app` without opening a port.
- `routes/` holds one router file per resource, mounted in `server.js` under the matching path.
- Routes never touch data directly: all data access goes through `db/store.js`, an in-memory array that resets on restart. Swapping in a real DB should only require changing that module.
