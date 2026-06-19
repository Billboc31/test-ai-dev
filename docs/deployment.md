---
generated_at: 2026-06-19
---

# Deployment

## Current State

No application deployment pipeline exists yet (`stack: unknown`, no source code). The information below covers the factory's deployment infrastructure as inferred from the `deployer-fixer` role.

## Sandbox Validation Pipeline

The factory includes a sandbox validation system invoked during CI or pre-deploy checks:

- **Orchestrator**: `run_sandbox.py` (referenced in `deployer-fixer` role — TODO: verify location, not present in current tree)
- **Invocation**: triggered via `AI_DEV_FACTORY_EXEC_CMD` environment variable
- **Output**: `${SANDBOX_RUNTIME_ROOT}/fix-proposal.md` written on failure

### Validation Result Schema (`validation.json`)

| Field | Description |
|---|---|
| `healthcheck_status` | Whether the deployed service responded to health checks |
| `smoke_status` | Whether smoke tests passed |
| `failing_step` | Which step in the pipeline failed |
| `proxy_urls` | URLs exposed by the sandbox |
| `ports` | Ports bound in the sandbox |
| `timestamps` | Start/end times |
| `log_path` | Path to the full run log |

### Fix Proposal Flow

When smoke tests fail:
1. `run_sandbox.py` invokes the `deployer-fixer` role via `AI_DEV_FACTORY_EXEC_CMD`
2. Fixer receives: `validation.json`, `deploy.yml`, allowed artifact list, last 200 lines of `run.log`
3. Fixer produces: `fix-proposal.md` with diagnosis, unified diff, rationale, and risks
4. **Fix proposals are never applied automatically** — human review required

### Allowed Deployment Artifacts

The fixer may only modify files under `.ai-dev-factory/scripts/` and deploy profile files (`deploy.yml`). Application source files are out of scope for the fixer.

## Application Deployment

TODO: define once stack and infrastructure are chosen. Document:
- Build command
- Deploy target (cloud provider, container registry, etc.)
- Environment promotion strategy (staging → production)
- Rollback procedure

## Safety Rules

- Never merge to `main` from within a sandbox
- Never apply fix proposals automatically
- Never run destructive operations outside the sandbox environment
- All deployment changes must go through the standard ticket → plan → implement → review → merge lifecycle
