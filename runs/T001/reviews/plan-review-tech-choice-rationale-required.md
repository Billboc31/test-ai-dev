# Plan review — technology choice rationale required

The current T001 plan chooses a stack, but the major architecture decisions are not justified enough.

## Blocking concern

The plan currently selects:

```text
Frontend: React 18 + TypeScript + Vite
Calendar: react-big-calendar
Backend: Node.js 20 LTS + Express 5
Database: SQLite via better-sqlite3
Deployment: single process Express serving Vite build + API
```

This may be a valid V1 stack, but the plan must explain why these choices are appropriate for this project and what alternatives were considered.

Architecture decisions should not look arbitrary.

## Required fix

Update `runs/T001/plan.md` to include an Architecture Decision Record section covering at least:

### Frontend choice

Compare and justify:

- React + TypeScript + Vite
- alternatives considered, such as Next.js or a simpler static frontend

### Calendar library choice

Compare and justify:

- `react-big-calendar`
- alternatives considered, especially FullCalendar

The decision should explain why the chosen calendar library is appropriate for drag/drop, month/week views, localization, styling, and V1 simplicity.

### Backend choice

Compare and justify:

- Node.js + Express
- alternatives considered, such as FastAPI, NestJS, or Spring Boot

The decision should explain why the backend fits a small calendar/task V1 and the planned deployment model.

### Database choice

Compare and justify:

- SQLite via `better-sqlite3`
- PostgreSQL

The plan must either:

1. justify SQLite clearly as a V1/local-demo choice with a documented migration path to PostgreSQL,

or

2. switch the decision to PostgreSQL if the product roadmap already implies multi-user, sync, reminders, collaboration, or production deployment.

### Deployment choice

Explain why a single process is acceptable for V1 and what would trigger a later split.

## Acceptance additions

- The plan includes an ADR-style rationale for every major stack decision.
- Alternatives are documented.
- If SQLite remains selected, the plan documents its limits and a future PostgreSQL migration trigger.
- No source code is added in T001.
- Documentation updates must record both the final decision and the rationale.

## Review verdict

PLAN_FIX_REQUIRED until stack choices are justified and alternatives are documented.
