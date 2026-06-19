---
generated_at: 2026-06-19
---

# Local Development

## Prerequisites

- Git (branch-per-ticket workflow)
- Claude Code CLI (for AI agent invocations)
- TODO: verify — no package manager or runtime detected yet (`stack: unknown`)

## Repository Setup

```bash
git clone git@github.com:Billboc31/test-ai-dev.git
cd test-ai-dev
```

No install step is needed for the framework itself — it is pure Markdown.

## Running a Ticket Through the Workflow

### 1. Create a ticket

```bash
# Copy the template, fill in the sections
cp ai/templates/ticket-template.md tickets/T001-my-feature.md
$EDITOR tickets/T001-my-feature.md
```

### 2. Classify risk

Invoke the Risk Classifier role with the ticket as input:

```
Role: risk-classifier
Files to read: ai/roles/risk-classifier.md, ai/skills/risk-classification.md, tickets/T001-my-feature.md
```

### 3. Plan

Invoke Planner using the generic prompt:

```
Prompt: prompts/generic/planner.md + ticket content
```

Output is saved to: `runs/<ticket-id>/plan.md`

### 4. Implement (after PLAN_APPROVED)

Invoke Coder:

```
Prompt: prompts/generic/coder.md + ticket + plan
```

### 5. Review and Test

Invoke Reviewer and Tester in sequence:

```
Prompt: prompts/generic/review.md + ticket + diff
Prompt: prompts/generic/tester.md + ticket + implementation
```

### 6. Update Memory (after IMPLEMENTATION_APPROVED)

```
Prompt: prompts/generic/memory-updater.md + ticket + plan + review
```

## Key Local Paths

| Path | Purpose |
|---|---|
| `docs/ai/global-context.md` | Loaded by every agent — project identity |
| `docs/ai/project-life.md` | Living project history |
| `docs/ai/workflow.md` | Workflow rules reference (TODO: create) |
| `docs/ai/decisions-log.md` | Architectural decisions log |
| `runs/` | Execution artefacts per ticket |
| `tickets/` | Incoming work items |

## Local URLs

TODO: verify — no web server detected.
