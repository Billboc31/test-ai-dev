# Plan fix — add architecture decision rationale

The current plan is not acceptable yet because it selects major technologies without enough rationale.

## Required update to the plan

Update `runs/T001/plan.md` so it does not only list the selected stack, but also explains why each choice is appropriate for the calendar task manager project.

## Required ADR section

Add a section such as:

```text
## Architecture decision record
```

It must cover:

### Frontend

Chosen option:

```text
React 18 + TypeScript + Vite
```

Also compare with alternatives such as:

```text
Next.js
plain React/Vite without TypeScript
server-rendered UI
```

Explain why the selected option is suitable for a small interactive calendar/task application.

### Calendar library

Chosen option:

```text
react-big-calendar
```

Compare with:

```text
FullCalendar
custom calendar implementation
```

Explain trade-offs around:

- month/week/day views
- drag and drop
- localization
- styling/customization
- V1 simplicity

### Backend

Chosen option:

```text
Node.js 20 LTS + Express 5
```

Compare with:

```text
FastAPI
NestJS
Spring Boot
```

Explain why Express is acceptable for V1, or switch if another backend is more appropriate.

### Database

Chosen option currently:

```text
SQLite via better-sqlite3
```

Compare with:

```text
PostgreSQL
```

The plan must either:

1. justify SQLite as a local/demo V1 choice and document the limits and migration trigger to PostgreSQL,

or

2. switch to PostgreSQL if the roadmap implies production, multi-user, reminders, collaboration, or hosted deployment soon.

### Deployment

Chosen option:

```text
single process Express serving frontend build + API
```

Explain why this is appropriate for V1 and what would trigger later separation.

## Documentation impact

The implementation phase should update docs to include:

- final stack decision
- alternatives considered
- rationale
- known limits
- migration triggers

## Non-goals

- Do not add source code.
- Do not add `package.json`.
- Do not implement backend/frontend yet.
- Do not define the database schema.

## Acceptance criteria

The corrected plan is acceptable only if:

- stack choices are justified
- alternatives are documented
- SQLite is either justified with a migration path or replaced by PostgreSQL
- no application code is planned for T001
