---
generated_at: 2026-06-19
---

# Architecture

## High-Level Structure

This repository is a **meta-project**: it defines the process and agents that will build the actual application, rather than containing the application itself. The architecture is therefore a *workflow architecture*, not a software system architecture.

```
tickets/          ← work items (Markdown, one file per ticket)
    │
    ▼
[Risk Classifier] → assigns AUTO_SAFE / CHAT_REVIEW_REQUIRED / HIGH_RISK
    │
    ▼
[Planner] → reads ticket + global-context + skills → produces runs/<id>/plan.md
    │
    ▼  (PLAN_APPROVED gate)
[Coder]   → reads ticket + plan + skills → edits source files, produces run summary
    │
    ▼  (human or auto review gate)
[Reviewer]        → reads ticket + plan + diff → produces runs/<id>/review.md
[Tester]          → runs validation → produces runs/<id>/test-report.md
    │
    ▼  (IMPLEMENTATION_APPROVED gate)
[Memory Updater]  → reads ticket + plan + review → updates docs/ai/
    │
    ▼  (MEMORY_APPROVED gate)
PR merged to main
```

## Directory Map

```
.ai-dev-factory/        factory bootstrap config + deployment scripts (scripts/ not yet present)
ai/
  roles/                one file per agent role — mission, must/must-not, expected output
  skills/               cross-cutting behavioural constraints (applied by roles)
  templates/            canonical Markdown output schemas
docs/
  ai/                   project memory (global-context, project-life, decisions-log)
  *.md                  generated documentation files
prompts/
  generic/              pre-built prompts for each role — the actual LLM invocation payloads
runs/                   per-ticket execution artefacts (plans, reviews, test results)
tickets/                incoming ticket files
```

## Key Architectural Invariants

- Every role has a single well-defined output format (enforced via templates).
- Memory (`docs/ai/`) is written **only** after `IMPLEMENTATION_APPROVED`.
- Roles are stateless — they read from ticket + plan + memory, produce one artefact.
- Skills are composable constraints — any role can apply any skill.
- Prompts in `prompts/generic/` are the canonical invocation path; `ai/roles/` files are the reference spec.
- One ticket = one Git branch = one coherent unit of work.

## Coupling Rules

- Roles must not read each other's internal state — only the shared artefacts (ticket, plan, review).
- Memory (`docs/ai/`) is the only shared mutable state between tickets.
- Source files are modified only by Coder; all other roles are read-only with respect to source.
