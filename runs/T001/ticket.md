# T001 — T001 - Define product vision, scope, and architecture for Test AI Dev

**Source**: GitHub Issue #24

## Description

# Goal

Define the overall vision, product scope, architecture, and technical foundations for the Test AI Dev project.

This ticket establishes the shared understanding that all future tickets will rely on.

## Product vision

Build a modern, simple, and responsive personal productivity application focused on:

- task management
- calendar planning
- agenda visualization
- prioritization
- personal organization

The application should feel lightweight, modern, and easy to use.

## High-level features

Initial MVP:

- create/edit/delete tasks
- calendar view
- agenda view
- priorities
- categories
- due dates
- planned dates

Future ideas:

- reminders
- recurring tasks
- drag-and-drop planning
- notifications
- productivity statistics
- AI-assisted planning

## Architecture goals

The project should:

- remain simple and maintainable
- use a monolithic architecture for the MVP
- expose a REST API backend
- use a modern React frontend
- be fully testable locally
- support automated development by AI Dev Factory agents

## Technical stack

Backend:
- Node.js 20
- TypeScript
- Express 5
- SQLite

Frontend:
- React 18
- TypeScript
- Vite

Testing:
- Vitest
- Playwright

Infrastructure:
- Docker Compose
- GitHub Actions CI

## Deliverables

- README updated with product vision.
- Architecture documented.
- Folder structure conventions documented.
- Development workflow documented.
- AI context documentation created under `docs/ai/`.

## Acceptance criteria

- A new contributor understands the project after reading the documentation.
- Architecture and conventions are documented.
- Future tickets can safely depend on this ticket.
- Documentation is committed to the repository.

## Notes

All subsequent tickets (T010+) depend conceptually on this ticket and should align with the documented architecture and product vision.
