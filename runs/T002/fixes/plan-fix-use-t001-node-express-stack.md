# Plan fix — rewrite T002 using the T001 Node/Express stack

The current T002 plan assumes Python/FastAPI, but T001 selected a Node/Express backend.

## Required plan change

Rewrite `runs/T002/plan.md` to bootstrap the backend foundation using the stack selected in T001:

```text
Node.js 20 LTS
TypeScript
Express 5
SQLite via better-sqlite3 later
single-process deployment model
```

T002 must not choose or assume a different backend stack.

## Files to plan

The corrected plan should include artifacts like:

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

If a different folder structure is chosen, explain it, but it must remain Node/Express/TypeScript.

## Dependencies

Plan appropriate dependencies for a minimal Express backend, for example:

```text
express
```

and dev/test tooling such as:

```text
typescript
ts-node-dev or tsx
vitest or jest
supertest
@types/express
@types/node
```

Do not introduce:

```text
FastAPI
uvicorn
pytest
pyproject.toml
pydantic-settings
```

## Health endpoint

The backend must expose:

```text
GET /health -> 200 { "status": "ok" }
```

with at least one automated test.

## Commands

Acceptance criteria should include Node commands:

```text
npm install
npm run dev
npm run build
npm test
```

## Documentation

Update docs/local-development.md and any relevant AI context docs to reflect the Node/Express backend foundation.

## Non-goals

Do not implement:

- task CRUD
- task schema
- database migrations
- frontend
- authentication
- Docker/CI unless strictly required

## Acceptance criteria

The corrected plan is acceptable only if:

- it uses the T001 stack
- no Python/FastAPI files are planned
- health endpoint and test scaffold are included
- no task business logic is introduced
