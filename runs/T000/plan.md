I have enough information to write the plan. The key finding: `docs/ai/global-context.md` exists and describes the AI Dev Factory workflow but contains **no product vision**. The other docs files (project-overview.md, architecture.md, domain-model.md, etc.) exist but were bootstrapped empty or with placeholders.

## Objective

Add the project's product vision — a calendar-centric personal task management web app — to the existing documentation and AI context files, without introducing any application code.

## Included

**`docs/ai/global-context.md`** — add a `## Product Vision` section (and sub-sections for core ideas, V1 scope, and out-of-scope) immediately after `## Identity`. This file is the primary context source for all AI agents and is explicitly named in the ticket acceptance criteria.

**`docs/project-overview.md`** — populate with the product description: what the application does, who it is for, the calendar-centric UX model, core feature pillars for V1, and explicit V1 exclusions. This file is the natural human-readable entry point.

**`docs/architecture.md`** — populate with the intended high-level architecture: a web application (client + server), responsive design constraint, and the deliberate choice to avoid microservices and enterprise workflow engines for V1.

**`docs/domain-model.md`** — populate with the core domain concepts: Task (title, due date/time, status), Calendar (time-based view of tasks), Scheduling (placing a task on a date/time slot). No code or data-schema detail — conceptual level only.

**`docs/ai/decisions-log.md`** — add an initial ADR entry recording the V1 product scope decision (calendar-centric personal task management, social/collaboration features deferred) and the stack-unknown state.

**`docs/ai/project-life.md`** — add the first entry describing T000: initial documentation pass, product vision established, no application code present.

## Excluded

- No changes to `ai/roles/`, `ai/skills/`, `ai/templates/`, or `prompts/` — those are framework files, not project documentation.
- No creation of `docs/ai/workflow.md` — that is a separate concern already tracked as a known risk; it is not product vision documentation.
- No selection of a technology stack — `stack: unknown` remains; stack selection is a future ticket.
- No changes to `.ai-dev-factory/project.yml`.
- No application source code of any kind.
- No changes to `docs/local-development.md`, `docs/deployment.md`, `docs/testing-strategy.md`, `docs/security.md`, `docs/configuration.md`, `docs/scripts.md`, `docs/data-flow.md`, `docs/dependencies.md`, `docs/validation.md`, `docs/known-risks-and-todos.md` — these all depend on a stack choice that has not yet been made.

## Acceptance criteria

- `docs/ai/global-context.md` contains a `## Product Vision` section describing the calendar-centric task management app, its core ideas, and explicit V1 exclusions.
- `docs/project-overview.md` is non-empty and describes the product purpose and V1 scope clearly enough that a new developer understands what is being built.
- `docs/architecture.md` is non-empty and describes the intended high-level architecture (web app, responsive, no microservices) at a conceptual level.
- `docs/domain-model.md` is non-empty and names and defines the core domain concepts (Task, Calendar, Scheduling).
- `docs/ai/decisions-log.md` contains at least one entry recording the V1 product scope decision.
- `docs/ai/project-life.md` contains an initial entry for T000.
- No file under `ai/roles/`, `ai/skills/`, `ai/templates/`, or `prompts/` is modified.
- No application source code is added anywhere in the repository.
- No new file is created except where an existing placeholder file is missing content (all target files already exist in `docs/`).
