The updated plan is written. Here is what changed:

**Structure fixes:**
- Removed the malformed preamble ("The plan is written to…") — the file now starts cleanly with the plan itself
- All four mandatory headings are present in order: `## Objective`, `## Included`, `## Excluded`, `## Acceptance criteria`

**ADR section added (`## Architecture decision record`):**
- **Frontend**: React + TypeScript + Vite justified against Next.js and server-rendered alternatives
- **Calendar library**: `react-big-calendar` justified against FullCalendar (paid drag-and-drop licence) and a custom implementation; maintenance-mode risk noted
- **Backend**: Node.js + Express justified against FastAPI, NestJS, and Spring Boot
- **Database**: SQLite justified as a V1/local-demo choice with explicit known limits and a documented PostgreSQL migration trigger (multi-user, hosted deployment, reminders, concurrent writes)
- **Deployment**: single-process model justified with explicit split triggers

All review blocking concerns are addressed. No source code is planned for T001.
