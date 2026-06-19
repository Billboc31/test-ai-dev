Plan written to `runs/T002/plan.md`. Here is the output:

---

## Objective

Bootstrap the backend project foundation: directory layout, dependency manifest, application entry point, environment-based configuration loading, a single `/health` endpoint, and a passing test suite skeleton. No task business logic is introduced.

## Included

**Assumption — stack (explicit, must be confirmed before PLAN_APPROVED):**
T001 run artefacts are absent from `runs/T001/` and `stack: unknown` is still the recorded state in `docs/ai/global-context.md`. This plan assumes T001 selected **Python 3.12 + FastAPI + SQLite** as the backend stack. If T001 chose a different stack, this plan must be revised before the Coder may proceed.

Files to create:
- `backend/`, `backend/__init__.py`, `backend/main.py` — FastAPI app factory
- `backend/config.py` — Pydantic-settings model loaded from env vars, no hardcoded secrets
- `backend/routers/health.py` — `GET /health` → `{"status": "ok"}` HTTP 200
- `backend/tests/test_health.py` — pytest + httpx ASGI transport, one test
- `pyproject.toml` — deps: `fastapi`, `uvicorn[standard]`, `pydantic-settings`; dev: `pytest`, `pytest-asyncio`, `httpx`
- `.gitignore` additions — `__pycache__/`, `*.pyc`, `.venv/`, `.env`, `*.sqlite3`

Files to update:
- `docs/local-development.md` — backend install/run commands
- `.ai-dev-factory/project.yml` — `stack` field updated from `unknown`

## Excluded

- Task CRUD endpoints, database schema, ORM models, migrations
- Frontend scaffolding, authentication, Docker, CI pipeline
- Any business logic

## Acceptance criteria

1. `uvicorn backend.main:app` starts with no errors, no env vars required
2. `GET /health` → HTTP 200 `{"status": "ok"}`
3. `pytest backend/tests/` exits 0 with ≥1 passing test
4. No secrets or `.env` committed
5. `.gitignore` covers Python artifacts and SQLite files
6. `docs/local-development.md` documents install and run commands
7. `stack` field in `.ai-dev-factory/project.yml` is no longer `unknown`

---

**Action required before `PLAN_APPROVED`:** Confirm the stack assumption. T001 produced no documented decision — if Python/FastAPI is correct, this plan is ready. If T001 chose a different stack, flag `PLAN_FIX_REQUIRED` with the correct stack and the plan will be revised.
