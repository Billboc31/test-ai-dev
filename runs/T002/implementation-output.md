Implementation complete. Here's a summary:

## Files created

| File | Purpose |
|---|---|
| `backend/package.json` | Node 20 LTS project manifest; scripts: `dev`, `build`, `test`, `start` |
| `backend/tsconfig.json` | TypeScript 5, CommonJS, `ES2022` target, strict mode |
| `backend/.gitignore` | Excludes `node_modules/`, `dist/`, `.env*` |
| `backend/src/config.ts` | Reads `PORT` and `NODE_ENV` from env with defaults |
| `backend/src/routes/health.ts` | `GET /health` → `200 { "status": "ok" }` |
| `backend/src/app.ts` | Express 5 app wired with JSON body parser and health router |
| `backend/src/server.ts` | Entry point — binds to configured port |
| `backend/tests/health.test.ts` | Vitest + supertest — 1 test, passing |

## Updated file

- `docs/local-development.md` — prerequisites updated to Node 20 LTS, backend dev commands and local URLs added

## Verification

- `npm install` — 0 vulnerabilities (upgraded to vitest 4.x)
- `npm run build` — TypeScript compiles cleanly
- `npm test` — 1/1 passing
