---
generated_at: 2026-06-19
---

# Domain Model

## Core Entities

### Ticket

A unit of work — one feature, bug fix, refactor, or infrastructure change. Stored as a Markdown file in `tickets/`.

**Template**: `ai/templates/ticket-template.md`

| Field | Description |
|---|---|
| ID | `TXXX` format (e.g., T001) |
| Contexte | Background and motivation |
| Objectif | What this ticket achieves |
| Périmètre / Inclus | What is in scope |
| Exclus | What is explicitly out of scope |
| Travail attendu | Expected deliverables |
| Contraintes | Constraints the implementation must respect |
| Critères d'acceptation | Verifiable acceptance criteria |
| Commandes utiles | Useful commands for implementation or validation |
| Notes | Additional notes |

### Plan

An implementation plan produced by the Planner for a specific ticket. Stored as `runs/<ticket-id>/plan.md`.

**Template**: `ai/templates/plan-template.md`

Required sections: Contexte, Objectif, Inclus, Fichiers concernés, Étapes, Risques, Hors scope, Vérifications prévues, Critères d'acceptation.

### Run

The collection of artefacts produced during the execution of a ticket. Stored under `runs/<ticket-id>/`.

| Artefact | File | Produced By |
|---|---|---|
| Plan | `plan.md` | Planner |
| Review | `review.md` | Reviewer |
| Test report | `test-report.md` | Tester |
| Memory update | `memory-update.md` | Memory Updater |
| Conflict context | `conflict/context.md` | Factory tooling |
| Conflict resolution | `conflict/resolution.md` | Conflict Resolver |
| Fix proposal | `fix-proposal.md` | Deployer Fixer |

### Workflow Status

The lifecycle state of a ticket at any moment.

| State | Meaning |
|---|---|
| `PLAN_APPROVED` | Plan validated; Coder may proceed |
| `PLAN_FIX_REQUIRED` | Plan needs revision before coding |
| `IMPLEMENTATION_APPROVED` | Implementation validated; Memory Updater may proceed |
| `IMPLEMENTATION_FIX_REQUIRED` | Implementation needs revision |
| `MEMORY_APPROVED` | Memory updated; ticket complete |
| `MEMORY_FIX_REQUIRED` | Memory update needs revision |

### Risk Level

The automation level assigned to a ticket by the Risk Classifier.

| Level | Meaning | Human Gate |
|---|---|---|
| `AUTO_SAFE` | Low-impact local change | Optional |
| `CHAT_REVIEW_REQUIRED` | Architecture / workflow / memory change | Required |
| `HIGH_RISK` | Destructive / security-sensitive | Mandatory |

### Role

An AI agent persona with a defined mission, must/must-not contract, and expected output. Stored in `ai/roles/`.

| Role | File |
|---|---|
| Planner | `ai/roles/planner.md` |
| Coder | `ai/roles/coder.md` |
| Reviewer | `ai/roles/reviewer.md` |
| Tester | `ai/roles/tester.md` |
| Risk Classifier | `ai/roles/risk-classifier.md` |
| Memory Updater | `ai/roles/memory-updater.md` |
| Conflict Resolver | `ai/roles/conflict-resolver.md` |
| Deployer Fixer | `ai/roles/deployer-fixer.md` |

### Skill

A cross-cutting behavioural constraint that can be applied by any role. Stored in `ai/skills/`.

| Skill | File |
|---|---|
| Architecture Discipline | `ai/skills/architecture-discipline.md` |
| Code Quality | `ai/skills/code-quality.md` |
| Debugging | `ai/skills/debugging.md` |
| Documentation | `ai/skills/documentation.md` |
| Git Discipline | `ai/skills/git-discipline.md` |
| Memory Management | `ai/skills/memory-management.md` |
| Refactor Safety | `ai/skills/refactor-safety.md` |
| Risk Classification | `ai/skills/risk-classification.md` |
| Security | `ai/skills/security.md` |
| Testing | `ai/skills/testing.md` |
| Workflow Discipline | `ai/skills/workflow-discipline.md` |

### Project Memory

Persistent context shared across all ticket executions. Stored in `docs/ai/`.

| File | Content | Status |
|---|---|---|
| `global-context.md` | Stable project identity and invariants | Present |
| `project-life.md` | Living history of implemented changes | Present |
| `decisions-log.md` | Architectural decision log | Present |
| `workflow.md` | Workflow rules and conventions | **Missing** — create before first ticket |
