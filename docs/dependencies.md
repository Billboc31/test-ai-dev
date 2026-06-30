---
generated_at: 2026-06-19
---

# Dependencies

## Current State

The repository has `stack: unknown` and contains no application source code. There are no package manifests, lock files, or runtime dependencies detected.

## Framework Dependencies

The ai-dev-factory framework itself has no software dependencies — it consists entirely of Markdown files and relies on:

| Dependency | Type | Notes |
|---|---|---|
| Git | Runtime | Branch-per-ticket workflow, PR-based reviews |
| Claude Code (or compatible LLM CLI) | Runtime | Agent invocation; specific model TODO: verify |
| Markdown renderer | Optional | For reading artefacts — any editor or viewer |

## Sandbox Pipeline Dependencies

The `deployer-fixer` role references `run_sandbox.py`, implying Python is used for the factory orchestration layer. TODO: verify — the script is not present in the current tree.

## When Application Stack Is Chosen

Once a stack is selected, update this file with:

- Package manager (npm, pip, cargo, etc.)
- Lock file status (committed? gitignored?)
- Key runtime dependencies
- Key dev dependencies (linters, test frameworks, type checkers)
- Any third-party services required locally (databases, queues, etc.)

## Dependency Rules (from `code-quality` skill)

- Avoid unnecessary dependencies
- Every new dependency must be justified in the ticket that introduces it
- Dependencies must not be introduced outside of an explicit ticket scope
