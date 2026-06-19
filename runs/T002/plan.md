The plan is written to `runs/T002/plan.md`. It:

- Uses Node.js 20 LTS + TypeScript + Express 5 exclusively (no Python/FastAPI)
- Covers all required artifacts: `package.json`, `tsconfig.json`, `src/app.ts`, `src/server.ts`, `src/config.ts`, `src/routes/health.ts`, `tests/health.test.ts`, `.gitignore`, and a `docs/local-development.md` update
- Scopes to the health endpoint + config loading + test scaffold only — no CRUD, no DB, no auth
- Includes 8 verifiable acceptance criteria using `npm` commands (Vitest + Supertest)
- Is structured with exactly the four required headings

Ready for `PLAN_APPROVED`.
