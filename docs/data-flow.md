---
generated_at: 2026-06-19
---

# Data Flow

## Ticket Lifecycle Data Flow

Each ticket produces a chain of artefacts. The diagram below shows what data each role reads and writes.

```
┌─────────────────────────────────────────────────────────────────────┐
│  INPUT                                                               │
│  tickets/<id>.md (human-authored)                                   │
└──────────────────────────┬──────────────────────────────────────────┘
                           │
                           ▼
┌──────────────────────────────────────────────────────┐
│  Risk Classifier                                      │
│  reads:  ticket, ai/roles/risk-classifier.md         │
│          ai/skills/risk-classification.md            │
│  writes: risk level + justification (in run output)  │
└──────────────────────────┬───────────────────────────┘
                           │
                           ▼
┌──────────────────────────────────────────────────────┐
│  Planner                                             │
│  reads:  ticket, global-context.md, project-life.md │
│          ai/roles/planner.md, relevant skills        │
│  writes: runs/<id>/plan.md                          │
└──────────────────────────┬───────────────────────────┘
                           │ PLAN_APPROVED (human gate)
                           ▼
┌──────────────────────────────────────────────────────┐
│  Coder                                               │
│  reads:  ticket, plan, global-context.md            │
│          ai/roles/coder.md, relevant skills          │
│  writes: source files (application code)            │
│          run summary (stdout)                        │
└─────────────┬────────────────────────────────────────┘
              │
     ┌────────┴────────┐
     ▼                 ▼
┌─────────────┐  ┌─────────────────┐
│  Reviewer   │  │  Tester         │
│  reads:     │  │  reads:         │
│    ticket   │  │    ticket       │
│    plan     │  │    plan         │
│    diff     │  │    implementation│
│    roles/   │  │    roles/tester │
│    reviewer │  │    relevant     │
│  writes:    │  │    skills       │
│    review   │  │  writes:        │
│    .md      │  │    test-report  │
└──────┬──────┘  └────────┬────────┘
       └─────────┬─────────┘
                 │ IMPLEMENTATION_APPROVED (human gate)
                 ▼
┌──────────────────────────────────────────────────────┐
│  Memory Updater                                      │
│  reads:  ticket, plan, review, test-report          │
│          ai/roles/memory-updater.md                 │
│          docs/ai/* (all memory files)               │
│  writes: docs/ai/project-life.md                   │
│          docs/ai/decisions-log.md                  │
│          docs/ai/global-context.md (if needed)     │
│          runs/<id>/memory-update.md                │
└──────────────────────────┬───────────────────────────┘
                           │ MEMORY_APPROVED
                           ▼
                    PR merged to main
```

## Conflict Resolution Data Flow

Triggered when a ticket branch has merge conflicts with main:

```
Factory tooling
  → writes conflict/context.md  (ticket + plan + reviews + diff + conflicted files + main commits)
  → invokes Conflict Resolver

Conflict Resolver
  reads:  conflict/context.md
  writes: each conflicted file (conflict markers removed)
          conflict/resolution.md  (decision log)
```

## Sandbox Validation Data Flow

Triggered on deployment failure (when sandbox pipeline is active):

```
run_sandbox.py
  → writes validation.json  (healthcheck, smoke status, ports, log path)
  → writes last 200 lines of run.log
  → invokes Deployer Fixer via AI_DEV_FACTORY_EXEC_CMD

Deployer Fixer
  reads:  validation.json, run.log, deploy.yml, allowed artifact list
  writes: ${SANDBOX_RUNTIME_ROOT}/fix-proposal.md  (diagnosis + diff + rationale + risks)

Human
  reviews fix-proposal.md
  applies patch manually if approved
```

## Shared Mutable State

Only one shared mutable state exists: **project memory** (`docs/ai/`).

- Written by: Memory Updater only, after `IMPLEMENTATION_APPROVED`
- Read by: every role at invocation time
- Must remain consistent with the validated, merged code on `main`

All other data (runs, tickets) is append-only. Source files are written by Coder only, within the scope of an approved plan.
