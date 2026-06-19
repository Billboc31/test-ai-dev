---
generated_at: 2026-06-19
---

# Testing Strategy

## Overview

Testing in this project operates at two levels:

1. **Workflow validation** — verifying that AI-produced artefacts (plans, implementations, reviews) meet their structural and functional contracts. This is the primary testing layer today.
2. **Application testing** — unit/integration/e2e tests for actual source code. Not yet applicable (no source code exists).

## Workflow Validation (Active)

Each ticket passes through a mandatory testing gate before memory is updated:

### Tester Role (`ai/roles/tester.md`)

- Validates implementation against each acceptance criterion in the ticket
- Produces a structured test report: commands run, results, anomalies, pass/fail verdict
- Must distinguish critical failures from optional improvements
- Output saved to: `runs/<ticket-id>/test-report.md`

### Reviewer Role (`ai/roles/reviewer.md`)

- Verifies correctness relative to ticket requirements
- Checks scope compliance, code quality, architecture conformance
- Produces a structured review: `ai/templates/pr-review-template.md`
- Decisions: `APPROVED` or `REQUEST_CHANGES`

### Acceptance Criteria (Required in Every Plan)

Every plan must include verifiable acceptance criteria. A plan without them is rejected by the Planner role.

## Application Testing (Future)

Once source code exists:

- Test framework: TODO: verify — choose based on selected stack
- Test location: TODO: define (e.g., `tests/`, `__tests__/`, `src/**/*.test.ts`)
- Coverage expectation: TODO: define
- CI integration: TODO: define (no `.github/workflows/` detected)

## Testing Rules (from `testing` skill)

- Test expected behaviour, not just happy paths
- Test critical error paths where possible
- Only test within ticket scope — do not expand coverage speculatively
- Document testing limits explicitly
- Any bug fix must include a reproducing test or validation step
- Tests that become out-of-scope for the ticket should be tracked as follow-up tickets

## Sandbox Smoke Tests

The `deployer-fixer` role references sandbox smoke tests (`smoke_status` in `validation.json`). These validate deployment artefacts, not application logic. TODO: verify scope and location when the sandbox pipeline is set up.
