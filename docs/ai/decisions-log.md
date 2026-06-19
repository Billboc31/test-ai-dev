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

### ADR-002 — V1 technology stack: React + TypeScript + Express + SQLite (2026-06-19)

**Decision**: The V1 application stack is:

| Layer | Choice |
|---|---|
| Frontend | React 18 + TypeScript + Vite |
| Calendar component | `react-big-calendar` |
| Backend | Node.js 20 LTS + Express 5 |
| Database | SQLite via `better-sqlite3` |
| Deployment | Single process — Express serves the Vite build + REST API |

**Rationale by layer:**

**Frontend — React 18 + TypeScript + Vite vs alternatives:**
- *Next.js* was ruled out: SSR and file-system routing add complexity that is unnecessary for a personal single-user app with no SEO requirements. V1 is a local interactive app, not a public site.
- *Plain React without TypeScript* was ruled out: TypeScript catches contract mismatches between the calendar library, the task data model, and the API early. The overhead for a greenfield project is minimal.
- *Server-rendered UI (e.g., Rails views, Jinja templates)* was ruled out: the calendar UX requires fine-grained client-side interactivity (drag-and-drop, view switching) that is hard to deliver with server-rendered HTML.

**Calendar library — `react-big-calendar` vs alternatives:**
- *FullCalendar* was considered but rejected: the drag-and-drop and premium view features require a paid licence for commercial use. `react-big-calendar` is MIT-licensed and covers the required day/week/month views and basic event rendering.
- *Custom calendar implementation* was ruled out: a correct, accessible, and localised calendar component is several weeks of engineering work. Adopting an existing library is the correct V1 trade-off.
- Known limit: `react-big-calendar` is in maintenance mode as of 2024. If it becomes unmaintained, migration to FullCalendar (open-core) or another library is the documented exit path.

**Backend — Node.js 20 LTS + Express 5 vs alternatives:**
- *FastAPI (Python)* would require a separate language runtime from the frontend toolchain. Sharing a Node.js runtime simplifies local dev setup and CI.
- *NestJS* was ruled out: its decorator-heavy architecture and opinionated module system introduce boilerplate disproportionate to a small REST API with 3–5 endpoints.
- *Spring Boot (Java)* was ruled out: Java runtime overhead and longer startup time are not justified for a personal local-first app.
- Express 5 is the minimal correct choice: thin, well-understood, no framework lock-in, straightforward to extend if needed.

**Database — SQLite via `better-sqlite3` vs PostgreSQL:**
- SQLite is the correct V1 choice: the application is single-user with no concurrent writers, no multi-region deployment, and no external service dependency. A single `.db` file on disk is the simplest possible storage for a personal app.
- `better-sqlite3` is preferred over the async `sqlite3` package because the synchronous API eliminates a class of async-related bugs in a single-threaded Express context.
- **Known limits**: SQLite is not suitable for multi-user, hosted production, or concurrent write workloads.
- **PostgreSQL migration trigger**: migrate to PostgreSQL when any of the following applies: multi-user support, hosted/cloud deployment, reminder notifications requiring a background worker, or concurrent write contention observed in practice.

**Deployment — single process vs split:**
- For a personal single-user app, a single Express process serving both the compiled Vite frontend and the REST API is the lowest-overhead deployment model. No reverse proxy, container orchestration, or CDN is needed.
- Split trigger: separate frontend and backend when the app is deployed for multiple users, when independent scaling is needed, or when a CDN is required for frontend delivery.

**Consequences:**
- `stack: unknown` in `project.yml` is resolved; all documentation updated.
- No authentication is required for V1 (single-user constraint from ADR-001).
- First implementation ticket will scaffold `package.json`, the Express entry point, and the Vite config.
- PostgreSQL migration path is documented and will be addressed in a future ticket if the migration trigger is reached.

---

- (init) Adopted the AI Dev Factory agent layout (ai/, docs/ai/, prompts/).
