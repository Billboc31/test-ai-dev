# T014 — T014 - Define task domain model and database schema

**Source**: GitHub Issue #15

## Description

## Goal

Define the core task domain model for the calendar productivity app.

## Depends on

- T013 - Add SQLite database layer and migrations

## Scope

Create the first product table(s) for tasks.

Task fields should include at least:

- id
- title
- description
- status
- priority
- category
- due date
- planned start/end datetime
- created_at
- updated_at

## Acceptance criteria

- Task schema is created through migrations.
- Domain types/interfaces are defined.
- Basic repository/data-access tests are added.
- No HTTP CRUD API is implemented yet.
