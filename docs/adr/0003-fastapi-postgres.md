# ADR-003: Own API in Python with FastAPI, on Postgres

**Date:** 2026-11-02 · **Status:** Accepted (updated 2026-11-09)

## Context

Building the backend is part of the point: practice, a showcase of backend and architecture work, and full freedom to customize or scale. A hosted backend (Supabase) would be faster but leave little to show. The founder likes both Go and Python. The data is relational (aspirations → goals → milestones; tasks, recurring items). Later features likely include AI: natural-language entry, suggestions, and learning the person's rhythm from completion data. The front end and back end live in separate repos and deploy independently.

## Decision

- A self-built API in Python with FastAPI, typed throughout (type hints and Pydantic), backed by Postgres.
- **The OpenAPI schema is the contract** between front end and back end. The front end's API types (and ideally its client) are generated from the schema FastAPI produces, never written by hand.

## Consequences

- Best ecosystem for the AI features: model SDKs, examples, and data/ML libraries for learning patterns from completion history.
- FastAPI gives typed request validation and API docs with little code.
- Needs a Python runtime or container to deploy, versus Go's single binary; performance differences don't matter at this scale.
- Go was the close alternative (simpler deployment, strong backend signal); the AI headroom was the tiebreaker.
- Generated types mean an API change shows up as a type error in the front end instead of a runtime bug, even across separate repos. Regenerating them becomes a step whenever the API changes.
- Still to decide when building: data access library and migrations, auth approach, and hosting.

## Revisited 2026-11-13

The data access library and migrations are decided in [ADR-007](0007-data-access-and-migrations.md): psycopg 3 with hand-written SQL and no ORM, and plain SQL migrations with dbmate. Sign-in and hosting are still open.
