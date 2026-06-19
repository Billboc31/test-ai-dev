All 8 planned files are updated. Here is the implementation summary:

---

## Changes made (T001)

**Files modified:**

| File | Change |
|---|---|
| `.ai-dev-factory/project.yml` | `stack: unknown` → `react-ts-express-sqlite` |
| `docs/architecture.md` | Replaced "stack TBD" intent with the concrete selected stack and updated the structure diagram |
| `docs/project-overview.md` | Replaced the "Detected Stack" table with the actual stack |
| `docs/dependencies.md` | Replaced "when stack is chosen, update this" with the planned frontend, backend, and dev dependencies |
| `docs/deployment.md` | Added V1 deployment model (single Express process) with split triggers |
| `docs/local-development.md` | Updated prerequisites (Node.js 20 LTS + npm) and local URLs section |
| `docs/ai/global-context.md` | Updated stack line; removed `stack: unknown` from Known Risks; updated secrets management note |
| `docs/ai/decisions-log.md` | Added **ADR-002** with full rationale for each stack choice (frontend, calendar library, backend, database, deployment) and documented PostgreSQL migration trigger |

**No source code created.** `stack: unknown` is eliminated from all target documentation.
