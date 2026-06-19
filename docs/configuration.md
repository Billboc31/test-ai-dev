---
generated_at: 2026-06-19
---

# Configuration

## Factory Configuration

### `.ai-dev-factory/project.yml`

```yaml
name: test-ai-dev
stack: unknown          # update when stack is chosen
bootstrapped_at: 2026-06-18T20:01:06Z
```

This file is the project identity record. Update `stack` once a technology is selected.

## Project Memory Configuration

### `docs/ai/global-context.md`

Loaded by every agent at invocation time. Currently contains:

```
project_id: test-ai-dev
repo: git@github.com:Billboc31/test-ai-dev.git
```

Add stable invariants here (architecture rules, naming conventions, domain terms) as the project matures.

## Expected Memory Files (Not Yet Created)

The prompt template (`ai/templates/prompt-template.md`) instructs agents to read:

| File | Purpose | Status |
|---|---|---|
| `docs/ai/global-context.md` | Project identity + invariants | Present |
| `docs/ai/project-life.md` | Living history of decisions | **Missing** — create at first ticket |
| `docs/ai/workflow.md` | Workflow rules and conventions | **Missing** — create at first ticket |
| `docs/ai/decisions-log.md` | Architectural decisions log | **Missing** — create at first ticket |

## Environment Variables

### Sandbox / Deploy Pipeline

The `deployer-fixer` role references `AI_DEV_FACTORY_EXEC_CMD` — the shell command used to invoke the AI model during sandbox runs:

| Variable | Purpose | Required |
|---|---|---|
| `AI_DEV_FACTORY_EXEC_CMD` | Command to invoke the LLM for fix proposals | When sandbox validation is active |

TODO: verify — no `.env` or shell config files detected. Confirm actual env var names when the deploy pipeline is set up.

## Secrets

No secrets detected in current repository content. Follow the security skill rules:
- Never hardcode secrets in files
- Never log sensitive data
- Store secrets in environment variables or a secrets manager (TODO: define approach when stack is known)
