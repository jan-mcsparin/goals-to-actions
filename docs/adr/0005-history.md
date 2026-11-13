# ADR-005: History is modelled where a feature reads it, and audited everywhere else

**Date:** 2026-11-13 · **Status:** Accepted

## Context

The first data model overwrote state: a task that rolled over just changed its week, and a milestone held only its latest result. ADR-003 expects later features to learn from completion history, and history that isn't captured can't be recovered. A trigger-driven audit table is a known, cheap way to keep every change, but it records that a column changed, not what the person did (a skip, a swap and a planner reflow all look like "planned day changed"), and querying JSON snapshots across schema changes gets painful.

## Decision

Two kinds of history, with one rule to tell them apart: **if a feature reads it, it's a real table or column; everything else is covered by the audit log.**

- **Product history is modelled.** Each week a task is in play (`task_weeks`), weekly target changes, check-ins, daily quitting answers and urge ratings are rows of their own.
- **Everything else is audited.** One `audit_log` table, filled by a Postgres trigger on every table from the first migration: the table, the row, the operation, the row before and after as JSON, and the time. The API sets the user and the name of the action ("skip task", "pause goal") for each transaction, so each audit row carries who and why.

## Consequences

- Nothing is lost from day one, at the cost of one table, one trigger function and a per-transaction setting.
- No screen or planner rule may read the audit log. When a feature needs something that only lives there (pause intervals, intensity over time), it gets a table, backfilled from the log.
- The audit log grows with every write. At one user that doesn't matter; revisit retention before there are many.
- Snapshots follow the schema of their time, so old rows won't match new columns. That's acceptable for recovery and debugging, and is the reason product history isn't kept there.
