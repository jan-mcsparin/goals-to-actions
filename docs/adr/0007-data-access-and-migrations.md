# ADR-007: Hand-written SQL on psycopg 3, with plain SQL migrations

**Date:** 2026-11-13 · **Status:** Accepted

## Context

ADR-003 chose FastAPI on Postgres and left the data access library and migrations open. The data model (`../data-model.md`) is now settled, with an audit trigger on every table (ADR-005) and planner work that is mostly set-based: filling a week, rolling over, lapsing, reading a day's plan. The backend is also partly a showcase of backend work.

Three ways to talk to Postgres were compared: the SQLAlchemy ORM, SQLAlchemy Core (the query builder alone), and the psycopg 3 driver with hand-written SQL. The ORM's main convenience is generating migrations from its models; with migrations written as plain SQL, those models become a second copy of the schema kept in step by hand.

## Decision

- **No ORM.** The API uses psycopg 3 with hand-written SQL. Rows are read straight into Pydantic models.
- **All database access sits in repository modules.** Routes and planning logic call those, and never hold SQL themselves.
- **Migrations are plain SQL files run by dbmate,** each with an up and a down section. dbmate runs from its Docker image, so it adds no Python dependency, and the `schema.sql` it writes is committed.
- **Synchronous to start.**
- **Local Postgres runs in Docker Compose,** on the current major version, and tests run against it, never SQLite.
- **Schema conventions:**
  - Status and type columns are text with a check constraint, not Postgres enum types, so adding or renaming a value is an ordinary migration.
  - Every query is scoped by owner in the API, and foreign keys include the owner, so the database refuses a link to another user's row. Row-level security waits until there is a second user.
  - Unique rules ignore deleted rows (partial indexes). The API does the cascade when something with children is deleted.
  - Built-in Postgres features only, no extensions, so the hosting choice stays open.

## Consequences

- The schema is defined in one place, the migrations, and triggers and constraints sit beside the tables they belong to.
- More code for plain create, read and update: each insert and update is written out for about 15 tables.
- A wrong column name in a SQL string is caught by tests, not by the type checker. Running tests against real Postgres is what makes that acceptable.
- Queries built from optional filters are awkward in hand-written SQL. If they turn up, SQLAlchemy Core can be added for those alone.
- A down migration restores structure, not data: rolling back a dropped column brings it back empty.
- Moving to async later is mechanical, because psycopg 3's sync and async APIs match and the database code is in one layer: `async` and `await` in the repository modules and their callers. Worth doing when slow network waits (AI calls) arrive.
- Still to decide: sign-in and hosting.
