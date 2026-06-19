---
generated_at: 2026-06-19
---

# Validation

## Current State

The project has no application source code yet (`stack: unknown`). Validation commands for the target application are **not yet defined**.

## Framework-Level Validation

The ai-dev-factory framework validates workflow artefacts structurally (Markdown schema), not via a test runner. The following checks are enforced by agent roles:

| Check | Enforced By | How |
|---|---|---|
| Plan has required sections | Planner + Reviewer | Sections: Contexte/Context, Objectif/Objective, Inclus/Included, Hors scope/Excluded, Critères d'acceptation/Acceptance criteria |
| Implementation matches ticket scope | Reviewer | PR review against ticket + plan |
| Acceptance criteria pass | Tester | Explicit per-criterion pass/fail in test report |
| Memory consistency | Memory Updater | Cross-references ticket, plan, and review before writing |

## Sandbox Validation

The `deployer-fixer` role references a `run_sandbox.py` script and a `validation.json` output with:

- `healthcheck_status`
- `smoke_status`
- `failing_step`
- `proxy_urls`, `ports`, `timestamps`, `log_path`

This suggests a sandbox validation pipeline exists (or is planned) under `.ai-dev-factory/`. TODO: verify — `run_sandbox.py` and the scripts directory are not present in the current tree.

## When Application Code Exists

Once a stack is chosen, add the relevant commands here:

```bash
# TODO: fill in once stack is known
# e.g. for Node.js:
npm test
npm run lint
npm run typecheck
npm run build

# e.g. for Python:
pytest
ruff check .
mypy .
```

## Confidence

- Framework-level validation rules: **high confidence** (derived from role + skill files)
- Application test commands: **not applicable yet** (no source code)
- Sandbox pipeline: **low confidence** (referenced in deployer-fixer but scripts not present)
