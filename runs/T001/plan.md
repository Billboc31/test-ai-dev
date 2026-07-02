## Objective

Establish the foundational documentation for Test AI Dev — product vision, scope, high-level monolithic architecture, technical stack, folder-structure conventions, and the AI-agent context files under `docs/ai/`. This ticket produces documentation only; no application source code is introduced.

## Included

- `README.md`: add product vision, MVP feature list (tasks, calendar, agenda, priorities, categories, due/planned dates), future-ideas section, and a short "what this repo is" onboarding paragraph pointing to `docs/architecture.md` and `docs/development-workflow.md`.
- `docs/architecture.md` (new): describe the monolithic architecture — Express 5 REST API backend (Node 20 + TypeScript) with SQLite persistence, React 18 + TypeScript + Vite frontend, single deployable unit for MVP; include a simple diagram of the layers and the request/data flow; state architectural invariants (REST boundary, no shared DB clients across layers, no cross-layer imports).
- `docs/folder-structure.md` (new): document the intended top-level layout (`backend/`, `frontend/`, `docs/`, `docs/ai/`, `tickets/`, `runs/`, `.ai-dev-factory/`) and naming conventions for future tickets to follow.
- `docs/development-workflow.md` (new): describe local dev expectations (Docker Compose target, `npm` workspaces or per-package scripts), testing strategy (Vitest for unit, Playwright for E2E), and CI approach (GitHub Actions) at a *conceptual* level — no config files created.
- `.ai-dev-factory/project.yml`: update the `stack` field from `unknown` to a concrete identifier (e.g. `node-typescript-react`).
- `docs/ai/workflow.md` (new — currently missing per `global-context.md` TODO): document the ticket lifecycle gates (`PLAN_APPROVED`, `IMPLEMENTATION_APPROVED`, `MEMORY_APPROVED`), which role may modify what, and the artefact paths under `runs/<ticket-id>/`.
- `docs/ai/global-context.md`: update the Stack & Components table and Known Risks/TODOs section to reflect the chosen stack, the monolithic architecture, and the resolution of the `workflow.md` and `stack: unknown` TODOs.
- `docs/ai/project-life.md`: add a T001 entry recording the vision/architecture decision and the date.
- `docs/ai/decisions-log.md`: add ADR-style entries for (a) monolithic architecture for MVP, (b) chosen technical stack, (c) SQLite for MVP persistence.

## Excluded

- Any application source code — no `backend/`, `frontend/`, `package.json`, `tsconfig.json`, or scaffolding of any kind (deferred to T010+).
- Actual `docker-compose.yml`, `Dockerfile`, or `.github/workflows/*.yml` files — the workflow document only describes intent; infra files come in later tickets.
- Database schema design, table definitions, or migrations.
- Choice of specific libraries beyond those named in the ticket (ORM, validator, router, state manager, styling approach, testing helpers).
- Lint/format configuration (ESLint, Prettier) and coding-style rules.
- Deployment target, hosting provider, secrets management, and observability decisions.
- Authentication and multi-user concerns (the ticket does not require them for MVP scope).

## Acceptance criteria

- `README.md` contains the product vision and MVP feature list, and points a new contributor to the architecture and workflow docs.
- `docs/architecture.md`, `docs/folder-structure.md`, and `docs/development-workflow.md` exist and are internally consistent with the ticket's stated stack (Node 20 + TS + Express 5 + SQLite / React 18 + TS + Vite / Vitest + Playwright / Docker Compose + GitHub Actions).
- `docs/ai/workflow.md` exists and documents the three approval gates and which role can modify source code vs. `docs/ai/`.
- `docs/ai/global-context.md` no longer lists `stack: unknown` or the missing `workflow.md` as open TODOs; its Stack & Components section reflects the chosen technologies and the monolithic architecture.
- `docs/ai/decisions-log.md` contains at least three dated entries covering monolithic architecture, chosen stack, and SQLite for MVP.
- `docs/ai/project-life.md` contains a T001 entry describing this decision.
- `.ai-dev-factory/project.yml` `stack` field is updated to a concrete value (not `unknown`).
- No application source code, no `package.json`, no infra configuration files are added by this ticket.
- All new and modified files are committed on branch `ticket/T001-t001-define-product-vision-scope-and-architecture` and reachable via the T001 PR.
- A reviewer reading only the documentation above can restate the product vision, the architecture at a high level, and the ticket workflow without consulting the ticket text.
