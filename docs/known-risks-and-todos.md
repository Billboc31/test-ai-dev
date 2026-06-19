---
generated_at: 2026-06-19
---

# Known Risks and TODOs

## Missing Memory Files

These files are referenced by agent prompts but do not yet exist. Agents will fail or produce incomplete output without them.

| File | Referenced In | Action |
|---|---|---|
| `docs/ai/project-life.md` | `ai/templates/prompt-template.md` | Create at first ticket close |
| `docs/ai/workflow.md` | `ai/templates/prompt-template.md` | Create before first ticket |
| `docs/ai/decisions-log.md` | `ai/roles/memory-updater.md` | Create at first ticket close |

## Undetermined Stack

`project.yml` has `stack: unknown`. Until a stack is chosen:
- No application test commands can be defined
- No build/lint/typecheck commands exist
- Dependency management is undefined
- `docs/validation.md`, `docs/dependencies.md`, and `docs/deployment.md` contain placeholders

**Action**: Create a ticket to select and document the technology stack.

## Sandbox Pipeline Not Present

The `deployer-fixer` role references:
- `run_sandbox.py`
- `.ai-dev-factory/scripts/`
- `AI_DEV_FACTORY_EXEC_CMD`
- `deploy.yml`
- `${SANDBOX_RUNTIME_ROOT}`

None of these exist in the current tree. TODO: verify whether they are planned, in a separate repo, or to be created.

## No CI/CD Configuration

No `.github/workflows/` or equivalent CI config was found. Workflow gates are currently enforced by convention only (agent roles), not by automated pipelines.

**Risk**: Gates (PLAN_APPROVED, IMPLEMENTATION_APPROVED) can be bypassed if invocations are done out of order.

**Action**: Add CI checks or a workflow orchestrator to enforce state transitions programmatically.

## No Application Tests

The repository has no test files. The testing strategy documents the intent but nothing is runnable.

## Uncertain Items (TODO: verify)

- The Python version and dependencies required by `run_sandbox.py`
- Whether `deploy.yml` is a planned file or already exists elsewhere
- The exact model or LLM CLI used for agent invocations (`AI_DEV_FACTORY_EXEC_CMD`)
- Whether `runs/` artefacts follow a specific subdirectory convention (e.g., `runs/<ticket-id>/`)
- Language convention: French is used in role files, English in generic prompts — the expected language per ticket type is not explicitly documented

## Technical Debt Risks

From the skills and roles, the following are flagged as recurring risk patterns to watch for:
- Memory diverging from validated code (risk: `memory-management` skill)
- Scope creep in implementation (risk: `refactor-safety` skill)
- PR mixing multiple concerns (risk: `git-discipline` skill)
- Implicit architectural coupling (risk: `architecture-discipline` skill)
