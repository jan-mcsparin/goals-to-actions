# Architecture decision records

Short records of decisions that shape what gets built: context, decision, and what it costs.

**When to add one:** the decision shapes what gets built, costs a lot to reverse, or would make someone ask "why?" later. Conventions and setup go in `CLAUDE.md` and the README instead.

**Format:** `NNNN-short-name.md` with Date, Status, Context, Decision, Consequences. Don't rewrite an accepted ADR's decision; add a dated "Revisited" section, or supersede it with a new ADR and mark the old one `Superseded by ADR-NNN`.

| ADR | Decision | Status |
| --- | --- | --- |
| [001](0001-pwa.md) | Installable, mobile-first web app (PWA) | Accepted |
| [002](0002-react-vite.md) | React with Vite, client-rendered | Accepted |
| [003](0003-fastapi-postgres.md) | Own API in FastAPI on Postgres; OpenAPI is the contract | Accepted |
| [004](0004-instant-open.md) | Today opens instantly from data on the phone | Accepted |
