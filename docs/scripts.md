---
generated_at: 2026-06-19
---

# Scripts

## Overview

The factory's automation scripts are located under `.ai-dev-factory/scripts/`. This directory is not yet populated (bootstrapped 2026-06-18). Script documentation will grow as the project matures.

## Known Scripts (Referenced, Not Yet Present)

### `run_sandbox.py`

Referenced in `ai/roles/deployer-fixer.md`.

**Purpose**: Orchestrates sandbox validation runs — deploys the application in an isolated environment, runs health checks and smoke tests, and invokes the Deployer Fixer AI role when tests fail.

**Invocation**: Called by CI or manually. Specific invocation command TODO: verify.

**Inputs**:
- Deployment artefacts under `.ai-dev-factory/scripts/`
- `deploy.yml` — project deploy profile
- `AI_DEV_FACTORY_EXEC_CMD` environment variable — LLM command used to invoke the Deployer Fixer

**Outputs**:
- `validation.json` — structured validation result (healthcheck_status, smoke_status, failing_step, proxy_urls, ports, timestamps, log_path)
- `run.log` — full execution log
- `${SANDBOX_RUNTIME_ROOT}/fix-proposal.md` — fix proposal written on failure (for human review)

**Safety**: Fix proposals are never applied automatically. The script writes them for human review only.

## Allowed Deployment Artefacts

Per the `deployer-fixer` role constraints, the only files that may be modified by automated fix proposals are:

- `.ai-dev-factory/scripts/*`
- `deploy.yml` (deploy profile)

Application source files are strictly out of scope for automated script patches.

## Adding New Scripts

When adding scripts to `.ai-dev-factory/scripts/`:

1. Create a ticket — scripts are infrastructure changes and require the full ticket lifecycle
2. Classify risk: scripts that modify state or invoke external systems are at minimum `CHAT_REVIEW_REQUIRED`
3. Document purpose, inputs, outputs, and safety constraints in this file
4. Ensure no secrets are embedded — use environment variables
5. Scripts must not perform destructive operations without explicit guard conditions

## Future Scripts (TODO)

Likely automation to add as the project matures:
- Ticket scaffolding helper (copy template + assign ID)
- Run directory initialisation
- Memory consistency checker
- CI integration script (lint, test, deploy gates)
