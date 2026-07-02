# T022 — T022 - Add Playwright smoke tests for core task flows

**Source**: GitHub Issue #23

## Description

## Goal

Add end-to-end smoke coverage for the most important user flows.

## Depends on

- T017 - Implement task list and quick create UI
- T020 - Add task edit and delete interactions in the UI

## Scope

- Add Playwright setup.
- Add smoke test for opening the app.
- Add smoke test for creating a task.
- Add smoke test for editing and deleting a task.
- Document how to run E2E tests locally.

## Acceptance criteria

- Playwright tests can run locally.
- CI can run or skip E2E tests explicitly.
- Core task flow is covered.
- Tests are stable and do not depend on external services.
