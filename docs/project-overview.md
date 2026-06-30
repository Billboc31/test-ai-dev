---
generated_at: 2026-06-19
---

# Project Overview

## What Is Being Built

**test-ai-dev** is building a **personal task management web application** with a calendar-centric UX.

Users plan, schedule, and track their personal tasks through a calendar interface. The primary metaphor is the calendar — tasks always appear in their temporal context — rather than a flat task list. The application targets individual productivity and is intentionally simple for V1.

### V1 Feature Pillars

| Feature | Description |
|---|---|
| Calendar view | Tasks displayed in day / week / month calendar views |
| Task management | Create, edit, complete, and delete tasks; each task has a title and an optional due date/time |
| Scheduling | Place a task on a specific date/time slot |
| Responsive design | Usable on desktop and mobile browsers |
| Personal use | Single-user, no login required for V1 |

### Explicitly Out of Scope for V1

- Social or sharing features (no followers, no shared calendars)
- Collaboration (no multi-user editing, comments, or assignments)
- Third-party calendar sync (Google Calendar, iCal, Outlook)
- Microservices or distributed back-end architecture
- Enterprise workflow engines or approval chains

---

## How This Project Is Built

`test-ai-dev` is a scaffold for AI-driven software development using the **ai-dev-factory** framework. It provides a structured, auditable workflow in which AI agents play specialised roles — planning, coding, reviewing, testing, memory management, conflict resolution, and deploy fixing — each governed by explicit contracts (roles + skills) and producing versioned Markdown artefacts.

The repository does not contain application source code yet. It is the *operating system* for a project: the rules, roles, templates, and prompts that coordinate AI agents safely.

## Detected Stack

| Layer | Technology |
|---|---|
| Language | Not yet determined (`stack: unknown` in `project.yml`) |
| AI agents | Claude Code (inferred from ai-dev-factory tooling) |
| Workflow artefacts | Markdown files versioned in Git |
| Package manager | TODO: verify — none detected yet |

## Main Runtime Components

| Component | Location | Purpose |
|---|---|---|
| Agent roles | `ai/roles/` | Define what each AI role must/must-not do |
| Cross-cutting skills | `ai/skills/` | Reusable behavioural constraints applied to roles |
| Artefact templates | `ai/templates/` | Canonical output formats (tickets, plans, reviews, memory) |
| Generic prompts | `prompts/generic/` | Ready-to-send prompts invoking each role |
| Project memory | `docs/ai/` | Persistent context shared across all agent invocations |
| Ticket queue | `tickets/` | Incoming work items (currently empty) |
| Run artefacts | `runs/` | Per-run outputs (plans, reviews, test reports, memory updates) |
| Factory config | `.ai-dev-factory/project.yml` | Project identity and bootstrap metadata |

## Workflow States

Each ticket moves through a gated lifecycle:

```
PLAN_APPROVED → (Coder) → IMPLEMENTATION_APPROVED → (Tester/Reviewer) → MEMORY_APPROVED
      ↓                           ↓                           ↓
PLAN_FIX_REQUIRED       IMPLEMENTATION_FIX_REQUIRED    MEMORY_FIX_REQUIRED
```

## Risk Levels

- `AUTO_SAFE` — low-impact local change, may run without human review
- `CHAT_REVIEW_REQUIRED` — architecture or workflow change, requires human approval
- `HIGH_RISK` — destructive or security-sensitive, mandatory human gate
