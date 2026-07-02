# T013 — T013 - Add SQLite database layer and migrations

**Source**: GitHub Issue #14

## Description

## Goal

Add the SQLite persistence foundation.

## Depends on

- T011 - Bootstrap backend Express TypeScript foundation

## Scope

- Add SQLite database connection module.
- Add migration mechanism.
- Add development database setup.
- Add test database setup.
- Document persistence conventions.

## Acceptance criteria

- Backend can initialize the database.
- Migrations can be applied repeatably.
- Tests can run against an isolated test database.
- No product tables beyond migration infrastructure unless required for validation.
