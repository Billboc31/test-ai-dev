# GLOBAL CONTEXT

# Global Context — Test Ai Dev

## Identity

- **project_id**: test-ai-dev
- **repository**: git@github.com:Billboc31/test-ai-dev.git
- **primary language/stack**: Not yet determined (`stack: unknown` in `.ai-dev-factory/project.yml`). The framework itself is pure Markdown.
- **bootstrapped**: 2026-06-18

## Product Vision

**test-ai-dev** will produce a **web application for personal task management with a calendar-centric experience**.

### Core idea

Users plan, schedule, and track their personal tasks through a calendar interface. The primary UX metaphor is the calendar, not a flat task list. Tasks are created with a due date/time and are always visible in their temporal context.

### V1 feature pillars

- **Calendar view**: tasks displayed in a calendar (day, week, month)
- **Task management**: create, edit, complete, and delete tasks; each task has a title and an optional due date/time
- **Scheduling**: assign a task to a specific date/time slot
- **Responsive design**: usable on desktop and mobile browsers
- **Personal use**: single-user, no authentication required for V1

### Explicitly out of scope for V1

- Social or sharing features
- Collaboration (multi-user, comments, assignments)
- Microservices or distributed architecture
- Enterprise workflow engines
- Third-party calendar sync (Google Calendar, iCal, etc.)

## Purpose

`test-ai-dev` is the operating environment for AI-driven software development using the **ai-dev-factory** framework. It defines the roles, skills, templates, and prompts that coordinate specialised AI agents (Planner, Coder, Reviewer, Tester, Memory Updater, Risk Classifier, Conflict Resolver, Deployer Fixer) in a bounded, auditable, and reversible workflow. The repository does not yet contain application source code — it is the scaffold that will govern whatever application is built inside it.

## Stack & Components

| Layer | Technology / Location |
|---|---|
| Framework | AI Dev Factory — Markdown-based agent workflow |
| Agent roles | `ai/roles/` — mission, must/must-not contract, expected output for each role |
| Cross-cutting skills | `ai/skills/` — reusable behavioural constraints applied to roles |
| Artefact templates | `ai/templates/` — canonical output formats (ticket, plan, review, memory update, etc.) |
| Generic prompts | `prompts/generic/` — ready-to-send LLM invocation payloads per role |
| Project memory | `docs/ai/` — persistent context shared across all agent invocations |
| Ticket queue | `tickets/` — incoming work items (currently empty) |
| Run artefacts | `runs/` — per-ticket outputs (plans, reviews, test reports, memory updates) |
| Factory config | `.ai-dev-factory/project.yml` — project identity and bootstrap metadata |
| Application source | TODO — no source code exists yet |

## Architecture

This repository implements a **workflow architecture**, not a software system architecture. Each ticket flows through a gated pipeline of stateless AI roles:

```
tickets/<id>.md
    │
    ▼
[Risk Classifier] → assigns AUTO_SAFE / CHAT_REVIEW_REQUIRED / HIGH_RISK
    │
    ▼
[Planner] → reads ticket + global-context → writes runs/<id>/plan.md
    │
    ▼  (PLAN_APPROVED — human gate)
[Coder] → reads ticket + plan → modifies source files
    │
    ├── [Reviewer] → writes runs/<id>/review.md
    └── [Tester]   → writes runs/<id>/test-report.md
    │
    ▼  (IMPLEMENTATION_APPROVED — human gate)
[Memory Updater] → reads all artefacts → updates docs/ai/
    │
    ▼  (MEMORY_APPROVED)
PR merged to main
```

**Key invariants:**
- Only **Coder** may modify source files; only after `PLAN_APPROVED`
- Only **Memory Updater** may modify `docs/ai/`; only after `IMPLEMENTATION_APPROVED`
- All other roles are read-only with respect to source and memory
- `docs/ai/` is the only shared mutable state between tickets
- One ticket = one branch = one PR = one coherent unit of work

## Conventions & Invariants

- **Language**: Role/skill files are written in French. Generic prompts are in English. Artefacts may be either — choose one language per artefact and do not mix.
- **Ticket IDs**: `TXXX` format (e.g., `T001`)
- **Run artefact paths**: `runs/<ticket-id>/plan.md`, `runs/<ticket-id>/review.md`, `runs/<ticket-id>/test-report.md`, `runs/<ticket-id>/memory-update.md`
- **Never skip gates**: `PLAN_APPROVED` before coding, `IMPLEMENTATION_APPROVED` before memory updates — no exceptions
- **No secrets in files**: credentials must never be committed; no `.env` or hardcoded tokens
- **No direct pushes to `main`**: all changes via PRs
- **Scope discipline**: one ticket = one concern; do not fix adjacent issues, surface them as new tickets

## Workflow Notes

**Invoking an agent:**
```bash
# Example: run the Planner
# 1. Read: docs/ai/global-context.md, docs/ai/project-life.md, docs/ai/workflow.md
# 2. Read: ai/roles/planner.md + relevant ai/skills/*.md
# 3. Read: tickets/<ticket-id>.md
# 4. Invoke via: prompts/generic/planner.md
# 5. Write output to: runs/<ticket-id>/plan.md
```

**Creating a ticket:**
```bash
cp ai/templates/ticket-template.md tickets/T001-my-feature.md
$EDITOR tickets/T001-my-feature.md
```

**Memory files every agent must read at invocation:**
1. `docs/ai/global-context.md` — this file
2. `docs/ai/project-life.md` — living project history
3. `docs/ai/workflow.md` — workflow rules (TODO: create before first ticket)
4. `docs/ai/decisions-log.md` — architectural decisions log

**No build, lint, or test commands exist** — no application source code is present yet.

## Known Risks & TODOs

- `docs/ai/workflow.md` — still missing; must be created before the first ticket is processed
- `stack: unknown` — no technology stack has been selected; blocks defining build/test/deploy commands
- `run_sandbox.py` and `.ai-dev-factory/scripts/` — referenced by `deployer-fixer` role but not present in the repository
- `deploy.yml` — referenced by `deployer-fixer` role but not present
- `AI_DEV_FACTORY_EXEC_CMD` — environment variable for sandbox LLM invocations; value and security implications not yet documented
- No `.github/workflows/` — workflow gates are enforced by convention only, not automated CI
- No `.gitignore` confirmed for secrets/credentials files
- TODO: verify commit signing configuration
- TODO: define secrets management approach when stack is chosen

---

# ROLE

# Role — Planner

## Mission

Lire un ticket et produire un plan d’implémentation court, concret, borné et actionnable.

## Tu dois

- comprendre le ticket
- proposer les étapes minimales
- lister les fichiers à créer ou modifier
- identifier les risques
- expliciter le hors scope
- produire un plan Markdown versionnable
- signaler les hypothèses nécessaires

## Tu ne dois pas

- coder
- réécrire le ticket
- anticiper les tickets suivants
- élargir le scope
- masquer les incertitudes

## Sortie attendue

Un fichier de plan conforme à `ai/templates/plan-template.md`.

## Règles

- le plan doit rester court
- le plan doit être exécutable par un Coder sans ambiguïté
- toute hypothèse doit être explicite
- toute dérive de scope doit être refusée

## Structure obligatoire

Tout plan doit contenir au minimum **les sections suivantes** (titres
Markdown niveau 2 — `##`). Les variantes anglaises sont acceptées à l'identique :

| Français (recommandé)         | English equivalent       |
|-------------------------------|--------------------------|
| `## Contexte`                 | `## Context`             |
| `## Objectif`                 | `## Objective`           |
| `## Inclus`                   | `## Included`            |
| `## Hors scope`               | `## Excluded`            |
| `## Critères d'acceptation`   | `## Acceptance criteria` |

Choisis une langue par plan, ne mélange pas FR et EN dans un même plan.

Ces titres sont obligatoires même si une section est courte : un ticket
trivial peut produire un plan court, mais la structure doit rester stable.

Ne jamais produire uniquement un résumé.
Ne jamais produire un compte rendu d’implémentation.

## Interdictions absolues

Tu ne dois jamais écrire :
- "implémentation terminée"
- "syntaxe valide"
- "changements appliqués"
- "voici ce qui a été fait"

Tu dois produire uniquement un plan futur, pas un compte rendu passé.

---

# SKILL: workflow-discipline

# Skill — Workflow Discipline

## Objectif

Faire respecter le lifecycle officiel des tickets et PR IA.

## Règles

- respecter l’ordre des étapes du workflow
- ne pas bypass les reviews obligatoires
- maintenir les statuts cohérents
- conserver les artefacts versionnés
- séparer plan, implémentation et mémoire

## Refuser si

- une review obligatoire est sautée
- la mémoire est mise à jour avant validation implémentation
- le workflow officiel est contourné

---

# SKILL: architecture-discipline

# Skill — Architecture Discipline

## Objectif

Préserver la cohérence architecture du projet dans le temps.

## Règles

- respecter les invariants documentés
- éviter les couplages implicites
- éviter les dépendances inutiles
- éviter les refactors transversaux non demandés
- documenter toute nouvelle règle structurante
- privilégier les changements locaux et bornés

## Refuser si

- le scope dérive
- plusieurs couches sont modifiées sans justification
- des conventions existantes sont cassées
- la mémoire projet devient incohérente

---

# SKILL: documentation

# Skill — Documentation

## Objectif

Maintenir une documentation utile, concise et alignée avec le code réel.

## Règles

- documenter les décisions importantes
- éviter les documentations vagues
- garder la mémoire projet cohérente
- expliciter les invariants architecture
- préférer Markdown simple et versionnable

## Refuser si

- la documentation diverge du comportement réel
- la mémoire contient des suppositions non validées
- des décisions importantes ne sont pas tracées

---

# TASK

The ticket follows.
# Generic Planner Task Read the ticket below and produce a detailed implementation plan. 

## Required output structure (strict) Your reply **MUST** be a Markdown document containing **exactly** these four level-2 headings, in this order, spelled exactly as shown:
## Objective
## Included
## Excluded
## Acceptance criteria
These headings are mandatory even for trivial tickets. A short plan is acceptable — an unstructured plan is not. - ## Objective — one or two sentences describing what the change achieves. - ## Included — concrete changes (files, functions, logic, tests). - ## Excluded — what is explicitly out of scope for this ticket. - ## Acceptance criteria — verifiable conditions a reviewer can check. ## Invalid output Your reply is **invalid** if any of the four headings above is missing, renamed, mistyped, or replaced by a synonym (e.g. ## Goal, ## Scope, ## In scope, ## Out of scope, ## Plan, ## Tasks are **not** accepted). An invalid reply will be rejected by the automated validator and the ticket will be retried. You **MUST NOT** write: - "implementation done" - "changes applied" - "here is what was done" - any past-tense report of work already performed You produce a *future* plan, not a status report. ## Minimal valid example (for a trivial ticket)
markdown
## Objective
Rename the helper `foo()` to `bar()` in `utils.py` to align with the new
naming convention. Behaviour is preserved.

## Included
- `utils.py`: rename `foo` → `bar`, update the docstring.
- `tests/test_utils.py`: update the single import and assertion.

## Excluded
- Renaming callers in other modules (tracked in a follow-up ticket).
- Any logic change inside `foo` / `bar`.

## Acceptance criteria
- `utils.py` no longer defines `foo`.
- `pytest tests/test_utils.py` passes.
- No other file references the old name.

The ticket follows.



# T002 — T002 - Bootstrap backend foundation

**Source**: GitHub Issue #5

## Description

Create the backend project foundation based on the architecture selected in T001. Include project structure, health endpoint, configuration loading and test scaffolding. No task business logic yet.

---

## Contexte de retry injecté par run_ticket.py

## Output précédent

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

---

## Review

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

---

## Instructions de fix

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