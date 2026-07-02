## Objective

Produce the baseline documentation, folder structure, and local development instructions that establish `test-ai-dev` as a calendar-based task management application project, so subsequent tickets can implement product features against a clear, documented foundation. No product code is written in this ticket.

## Included

- `README.md` (create or overwrite at repo root):
  - Short product description: calendar-based task management app.
  - Validated technical stack (see `docs/adr/` below) and rationale summary.
  - Prerequisites (runtime versions, package manager).
  - Local development instructions: install, run, build, lint, test commands (may reference "TBD once stack is scaffolded" only if a command truly cannot exist yet — otherwise fill in).
  - Repository layout overview (link to `docs/architecture.md`).
  - Link to `docs/ai/global-context.md` and the ai-dev-factory workflow.

- `docs/ai/global-context.md` (update — file already exists):
  - Replace the "primary language/stack: not yet determined" section with the validated stack.
  - Replace the "TODO — no source code exists yet" application-source row with the chosen source layout.
  - Update **Known Risks & TODOs** to remove items resolved by this ticket (stack unknown, missing workflow.md if created here).
  - Add product vision paragraph: calendar-based task management, simple, monolithic, responsive.

- `docs/ai/product-vision.md` (new): one-page product vision — target user, core value, MVP surface (calendar view + task CRUD), explicit non-goals (multi-tenant, native mobile, real-time collab, etc.).

- `docs/architecture.md` (new): high-level architecture overview — monolithic app, chosen stack, folder structure, data flow, responsive UI approach. Cross-links to ADRs.

- `docs/adr/` (new directory) with initial Architecture Decision Records in a lightweight format (title, status, context, decision, consequences):
  - `0001-technical-stack.md` — records the chosen stack (framework, language, database, styling).
  - `0002-monolithic-architecture.md` — records the decision to stay monolithic and responsive.
  - `0003-repository-layout.md` — records the top-level folder structure.

- `docs/development.md` (new): expanded local development guide referenced from README — env setup, running tests, running the linter, formatting, troubleshooting.

- `docs/ci-quality.md` (new): CI/lint/test expectations — commands that must pass locally and in CI, coverage or quality thresholds if any, formatter rules. This is documentation only; no CI workflow file is created in this ticket unless the stack scaffolding produces one by default.

- Initial folder scaffolding (empty directories with a `.gitkeep` file or a one-line `README.md`) matching the ADR-0003 layout, e.g.:
  - `src/` (or the stack-idiomatic equivalent) with placeholder subfolders for `features/`, `components/`, `lib/`, `server/` as decided in the ADR.
  - `tests/` with a placeholder.
  - No product code inside.

- `.ai-dev-factory/project.yml`: update `stack:` from `unknown` to the chosen value; keep all other fields intact.

- `.gitignore` (create or update): standard ignores for the chosen stack, plus `.env*`, editor/OS files. Do not commit any secrets file.

- `docs/ai/workflow.md` (new — only if not already created by another in-flight ticket): document the gated pipeline exactly as summarised in `global-context.md`, so agents have a canonical source. Skip if already present.

- `docs/ai/decisions-log.md`: append an entry summarising the decisions taken (stack, monolith, layout) with links to the ADR files. Create the file if it does not yet exist.

## Excluded

- Any product feature implementation (no calendar view, no task model, no API routes, no auth).
- Any real UI screen beyond an at-most trivial placeholder that the stack's `create-app` generator produces by default.
- Framework/tool installation scripts beyond what the chosen stack's own bootstrap command produces.
- CI workflow files under `.github/workflows/` — documented as expectations only; wiring CI is a separate ticket.
- Deployment configuration, hosting choice, secrets management implementation (documented as follow-ups if needed).
- Database schema, ORM configuration, migrations.
- Choosing or configuring an auth provider.
- Testing framework configuration beyond documenting the intended commands.
- Renaming or restructuring existing `ai/`, `prompts/`, `tickets/`, `runs/` directories.

## Acceptance criteria

- `README.md` exists at the repo root, describes the product in one paragraph, lists prerequisites, and shows the commands a new contributor runs to install, start, lint, and test the project locally.
- `docs/ai/global-context.md` no longer contains `stack: unknown` or the "TODO — no source code exists yet" row; both reflect the decisions made in this ticket.
- `docs/adr/0001-technical-stack.md`, `docs/adr/0002-monolithic-architecture.md`, and `docs/adr/0003-repository-layout.md` exist, each with the ADR sections *Context*, *Decision*, *Consequences*, and status `Accepted`.
- `docs/architecture.md` exists and matches the folder structure actually present in the repository after this ticket.
- `docs/development.md` and `docs/ci-quality.md` exist and are linked from `README.md`.
- `docs/ai/product-vision.md` exists and states the product is a calendar-based task management app, monolithic and responsive, with explicit non-goals.
- The top-level folder structure documented in ADR-0003 is present on disk (each directory contains at least a `.gitkeep` or placeholder `README.md`).
- `.ai-dev-factory/project.yml` shows the validated stack value.
- `.gitignore` is present and covers secrets (`.env*`) and stack-appropriate build artefacts.
- No file under `src/` (or the equivalent source folder) implements a product feature — only placeholders or generator-default scaffolding.
- `git grep -n "TODO"` in `docs/ai/global-context.md` shows no remaining TODO about stack selection or workflow.md creation.
- A reviewer can, from a clean clone, follow `README.md` and reach a running placeholder app and passing lint/test commands.
