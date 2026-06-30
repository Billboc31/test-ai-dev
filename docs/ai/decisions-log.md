# Decisions Log — Test Ai Dev

Architectural and process decisions (newest first). Maintained by the
memory-updater after each approved ticket.

## Decisions

### ADR-001 — V1 product scope: calendar-centric personal task manager (2026-06-19)

**Decision**: Build a single-user web application for personal task management with a calendar as the primary UX interface. Tasks have a title and an optional due date/time. Users can create, edit, complete, and delete tasks and view them in day/week/month calendar views.

**Rationale**: The simplest version of a useful calendar-task app that can be shipped and iterated on. Keeping it personal (no auth, no collaboration) eliminates a large class of backend complexity for V1.

**Consequences**:
- Social, sharing, and collaboration features are explicitly deferred to a future version.
- Microservices and enterprise workflow engines are out of scope.
- Technology stack remains to be chosen (see `stack: unknown`); it must support a responsive web UI.

---

- (init) Adopted the AI Dev Factory agent layout (ai/, docs/ai/, prompts/).
