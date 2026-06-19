# Plan review — T002 must use the T001 architecture decision

The current T002 plan is not acceptable because it contradicts the architecture decision made in T001.

## Blocking issue

T001 selected the backend stack as:

```text
Node.js 20 LTS + Express 5
SQLite via better-sqlite3
single-process Express serving API + Vite build later
```

But the current T002 plan assumes:

```text
Python 3.12 + FastAPI + SQLite
```

This would fork the project architecture immediately and make T001 useless.

## Required correction

T002 must consume the approved T001 architecture decision and bootstrap the backend using:

```text
Node.js 20 LTS
TypeScript
Express 5
SQLite-ready foundation, but no task schema yet
```

The plan must not redefine the stack.

## Expected backend bootstrap artifacts

The corrected plan should create or update files such as:

```text
package.json
tsconfig.json
src/app.ts
src/server.ts
src/config.ts
src/routes/health.ts
tests/health.test.ts
.gitignore
```

Exact filenames may vary, but the result must be an Express/TypeScript backend foundation, not FastAPI.

## Expected commands

Acceptance criteria should use Node commands, for example:

```text
npm install
npm run dev
npm run build
npm test
```

No Python, pytest, uvicorn, pyproject.toml, FastAPI or Pydantic settings should be introduced.

## Scope reminder

T002 should include:

- backend directory/source layout
- dependency manifest
- Express app entry point
- env-based configuration loading
- `GET /health` endpoint
- test scaffold with at least one passing health test
- documentation updates

T002 should exclude:

- task CRUD
- database schema
- ORM/models/migrations
- frontend
- authentication
- Docker/CI unless strictly necessary for backend bootstrap

## Review verdict

PLAN_FIX_REQUIRED until the plan is rewritten to use the T001 stack.