---
generated_at: 2026-06-19
---

# Dependencies

## Current State

The repository contains no application source code yet. Package manifests (`package.json`) will be introduced in the first implementation ticket. The stack has been selected (T001) and is documented below.

## Planned Application Dependencies

These dependencies will be introduced once implementation begins. Exact versions to be pinned at first install.

### Frontend (`client/` or `frontend/`)

| Package | Purpose |
|---|---|
| `react` `react-dom` | UI framework |
| `typescript` | Type safety |
| `vite` | Build tool and dev server |
| `react-big-calendar` | Calendar component (day/week/month views) |

### Backend (`server/` or root)

| Package | Purpose |
|---|---|
| `express` (v5) | HTTP server and REST API |
| `better-sqlite3` | SQLite driver (synchronous, zero native deps beyond SQLite3) |

### Dev dependencies (planned)

| Package | Purpose |
|---|---|
| `@types/react` `@types/express` | TypeScript type definitions |
| `eslint` | Linting |
| `vitest` or `jest` | Unit testing |

## Framework Dependencies

The ai-dev-factory framework itself has no software dependencies — it consists entirely of Markdown files and relies on:

| Dependency | Type | Notes |
|---|---|---|
| Git | Runtime | Branch-per-ticket workflow, PR-based reviews |
| Claude Code | Runtime | Agent invocation |
| Markdown renderer | Optional | For reading artefacts — any editor or viewer |

## Sandbox Pipeline Dependencies

The `deployer-fixer` role references `run_sandbox.py`. TODO: verify — the script is not present in the current tree.

## Dependency Rules (from `code-quality` skill)

- Avoid unnecessary dependencies
- Every new dependency must be justified in the ticket that introduces it
- Dependencies must not be introduced outside of an explicit ticket scope
