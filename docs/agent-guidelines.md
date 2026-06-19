---
generated_at: 2026-06-19
---

# Agent Guidelines

## Core Principle

Every AI agent operating in this repository must be **bounded, auditable, and reversible**. No autonomous action should be taken that cannot be reviewed and rolled back.

## Always Read Before Acting

Every agent invocation must begin by reading:

1. `docs/ai/global-context.md` — project identity and stable invariants
2. `docs/ai/project-life.md` — living history
3. `docs/ai/workflow.md` — workflow rules (create if absent)
4. The relevant role file: `ai/roles/<role>.md`
5. The relevant skill files: `ai/skills/<skill>.md`
6. The ticket: `tickets/<ticket-id>.md`
7. The plan (if past planning phase): `runs/<ticket-id>/plan.md`

## Role Responsibilities

| Role | File | May modify source? | May modify memory? |
|---|---|---|---|
| Risk Classifier | `ai/roles/risk-classifier.md` | No | No |
| Planner | `ai/roles/planner.md` | No | No |
| Coder | `ai/roles/coder.md` | **Yes** | No |
| Reviewer | `ai/roles/reviewer.md` | No | No |
| Tester | `ai/roles/tester.md` | No | No |
| Memory Updater | `ai/roles/memory-updater.md` | No | **Yes** |
| Conflict Resolver | `ai/roles/conflict-resolver.md` | **Yes** (conflict files only) | No |
| Deployer Fixer | `ai/roles/deployer-fixer.md` | Deploy artifacts only | No |

## Workflow Gates — Never Skip

```
Ticket → Risk Classification → Plan → PLAN_APPROVED
      → Implementation → IMPLEMENTATION_APPROVED
      → Memory Update → MEMORY_APPROVED
```

- Coder must not act before `PLAN_APPROVED`
- Memory Updater must not act before `IMPLEMENTATION_APPROVED`
- Reviews (Reviewer + Tester) must not be bypassed regardless of risk level

## Safe Change Policy

**Always safe (no confirmation needed):**
- Reading any file
- Writing to `runs/` (artefacts)
- Writing to `tickets/` (new tickets)

**Requires PLAN_APPROVED:**
- Any modification to source files
- Adding/removing dependencies

**Requires IMPLEMENTATION_APPROVED:**
- Any modification to `docs/ai/` (project memory)

**Requires explicit human confirmation (HIGH_RISK):**
- Deleting files
- Database migrations
- Security-sensitive changes
- Any destructive or irreversible operation

## Files Agents Must Not Modify Without Explicit Ticket Scope

- `docs/ai/global-context.md` — stable invariants, change only when a decision changes the project identity
- `.ai-dev-factory/project.yml` — factory config
- Any file outside the ticket's stated scope

## Scope Discipline

- One ticket = one concern = one branch = one PR
- Do not fix adjacent issues discovered during implementation — create a new ticket
- Do not refactor code outside the ticket's stated files
- Reject scope expansion silently: surface it as a risk or new ticket

## Output Conventions

- All artefacts are Markdown files
- Plans: `runs/<ticket-id>/plan.md`
- Reviews: `runs/<ticket-id>/review.md`
- Test reports: `runs/<ticket-id>/test-report.md`
- Memory updates: `runs/<ticket-id>/memory-update.md`
- Conflict resolutions: `runs/<ticket-id>/conflict/resolution.md`
- Fix proposals: `${SANDBOX_RUNTIME_ROOT}/fix-proposal.md`

## Language Convention

Plans and artefacts may be written in French or English — choose one language per artefact and do not mix. The repository default is French for role/skill files and English for generic prompts.

## What Agents Must Never Do

- Hardcode secrets or credentials
- Log sensitive data
- Merge to `main` directly
- Apply fix proposals automatically
- Modify memory before `IMPLEMENTATION_APPROVED`
- Skip a required review gate
- Make a `git reset --hard`
- Modify files outside ticket scope without explicit justification
