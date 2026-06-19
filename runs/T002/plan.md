The plan has been written to `runs/T002/plan.md`. It contains exactly the four required headings in order:

- **## Objective** — bootstrap a Node.js 20 LTS + TypeScript + Express 5 backend with health endpoint and test scaffold; no task logic
- **## Included** — 8 new files under `backend/` (`package.json`, `tsconfig.json`, `.gitignore`, `src/config.ts`, `src/routes/health.ts`, `src/app.ts`, `src/server.ts`, `tests/health.test.ts`) plus an update to `docs/local-development.md`; one explicit hypothesis about directory structure
- **## Excluded** — task CRUD, DB/ORM, frontend, auth, Docker/CI, and all Python/FastAPI artefacts
- **## Acceptance criteria** — 8 verifiable conditions using `npm install`, `npm run build`, `npm run dev`, and `npm test`; one criterion explicitly checks no Python/FastAPI references exist
