---
generated_at: 2026-06-19
---

# Architecture

## Application Architecture (V1)

The application is a **web application** with a thin React client and a lightweight Express server. No microservices. No enterprise workflow engine.

**Selected stack**: React 18 + TypeScript + Vite (frontend) · Node.js 20 LTS + Express 5 (backend) · SQLite via `better-sqlite3` (database) · single-process deployment.

### Structure

```
Browser (client)
  └── React 18 + TypeScript (Vite build)
        └── Calendar UI                  ← react-big-calendar; day / week / month views
              ├── Task list panel        ← sidebar or overlay for the selected period
              └── Task detail form       ← create / edit / complete / delete a task

Server (Node.js 20 LTS + Express 5)
  └── Task CRUD endpoints  (/api/tasks)  ← REST API
  └── SQLite database                    ← better-sqlite3; single file on disk
  └── Static file serving               ← serves the Vite production build

Single process: Express handles both the API and the frontend static assets.
```

### Design constraints

| Constraint | Rationale |
|---|---|
| Responsive design (mobile + desktop) | Core requirement; the app must be usable on any device |
| Single-user, no auth for V1 | Eliminates auth/session complexity; personal productivity focus |
| No microservices | Simplicity; V1 is a single deployable unit |
| No third-party calendar sync | Out of scope; reduces external API dependencies |

---

## Workflow Architecture (AI Dev Factory)

This repository is also a **meta-project**: it defines the process and agents that will build the actual application. The workflow architecture is described below.

```
tickets/          ← work items (Markdown, one file per ticket)
    │
    ▼
[Risk Classifier] → assigns AUTO_SAFE / CHAT_REVIEW_REQUIRED / HIGH_RISK
    │
    ▼
[Planner] → reads ticket + global-context + skills → produces runs/<id>/plan.md
    │
    ▼  (PLAN_APPROVED gate)
[Coder]   → reads ticket + plan + skills → edits source files, produces run summary
    │
    ▼  (human or auto review gate)
[Reviewer]        → reads ticket + plan + diff → produces runs/<id>/review.md
[Tester]          → runs validation → produces runs/<id>/test-report.md
    │
    ▼  (IMPLEMENTATION_APPROVED gate)
[Memory Updater]  → reads ticket + plan + review → updates docs/ai/
    │
    ▼  (MEMORY_APPROVED gate)
PR merged to main
```

## Directory Map

```
.ai-dev-factory/        factory bootstrap config + deployment scripts (scripts/ not yet present)
ai/
  roles/                one file per agent role — mission, must/must-not, expected output
  skills/               cross-cutting behavioural constraints (applied by roles)
  templates/            canonical Markdown output schemas
docs/
  ai/                   project memory (global-context, project-life, decisions-log)
  *.md                  generated documentation files
prompts/
  generic/              pre-built prompts for each role — the actual LLM invocation payloads
runs/                   per-ticket execution artefacts (plans, reviews, test results)
tickets/                incoming ticket files
```

## Key Architectural Invariants

- Every role has a single well-defined output format (enforced via templates).
- Memory (`docs/ai/`) is written **only** after `IMPLEMENTATION_APPROVED`.
- Roles are stateless — they read from ticket + plan + memory, produce one artefact.
- Skills are composable constraints — any role can apply any skill.
- Prompts in `prompts/generic/` are the canonical invocation path; `ai/roles/` files are the reference spec.
- One ticket = one Git branch = one coherent unit of work.

## Coupling Rules

- Roles must not read each other's internal state — only the shared artefacts (ticket, plan, review).
- Memory (`docs/ai/`) is the only shared mutable state between tickets.
- Source files are modified only by Coder; all other roles are read-only with respect to source.
