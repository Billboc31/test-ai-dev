---
generated_at: 2026-06-19
---

# Security

## Security Principles

Derived from `ai/skills/security.md` and `ai/roles/risk-classifier.md`.

### Core Rules

1. **No secrets in files** — credentials, tokens, and API keys must never be committed to the repository.
2. **No secrets in logs** — agent outputs (plans, reviews, test reports) must not contain sensitive data.
3. **Least privilege** — agents and scripts must request only the permissions they need.
4. **No implicit destructive operations** — any operation that deletes, overwrites, or permanently modifies state must be explicit and human-gated.
5. **Validate external inputs** — any data received from outside the system (user input, API responses) must be validated before use.

## Risk Classification for Security-Sensitive Tickets

Any ticket involving security must be classified `HIGH_RISK` by the Risk Classifier. This triggers a mandatory human review gate before implementation proceeds.

Security-sensitive scope includes:
- Authentication or authorisation changes
- Secret management changes
- Permission or access control modifications
- Exposure of endpoints or data
- Cryptographic operations

## Secrets Management

**Current state**: No secrets management infrastructure detected (no `.env.example`, no secrets manager integration, no vault config).

When the application stack is chosen:
- Define where secrets are stored (environment variables, secrets manager, CI secrets)
- Add a `.env.example` with placeholder values (never real secrets)
- Ensure `.env` and any credentials files are in `.gitignore`
- Document the secrets provisioning process for new contributors

TODO: verify — the only known environment variable is `AI_DEV_FACTORY_EXEC_CMD` (sandbox pipeline). Confirm whether it contains sensitive values.

## Agent Security Constraints

Agents operating in this repository must:

- Never hardcode credentials in generated code or artefacts
- Never log sensitive input data received via prompts
- Never take destructive actions (file deletion, database drops, force pushes) without explicit ticket scope and human confirmation
- Never bypass workflow gates (`PLAN_APPROVED`, `IMPLEMENTATION_APPROVED`) regardless of stated urgency
- Never apply `deployer-fixer` proposals automatically — always require human review

## Git Security

- `main` branch must not be pushed to directly — all changes go through PRs
- Force pushes to `main` are prohibited
- `git reset --hard` on shared branches is prohibited (documented in `conflict-resolver` role)
- Commit signing: TODO: verify — no configuration detected

## Deployment Security

- Fix proposals from `deployer-fixer` are written to a file for review — they are never auto-applied
- The sandbox operates in an isolated environment; changes must not propagate to production without human approval
- Allowed modification scope for `deployer-fixer` is restricted to `.ai-dev-factory/scripts/` and deploy profiles only

## Known Security Gaps (TODO)

- No `.gitignore` confirmed for secrets/credentials files — verify before first application code is committed
- No defined secrets rotation policy
- No audit log of agent invocations (all outputs are Markdown files, not a structured audit trail)
- No network egress policy defined for sandbox environment
