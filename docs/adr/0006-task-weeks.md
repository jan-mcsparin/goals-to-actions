# ADR-006: A task is one row for life; each week it's in play is its own record

**Date:** 2026-11-13 · **Status:** Accepted

## Context

In the first model one task row carried the thing to do, the week it belonged to, the day it was planned for, a four-value status and a rollover counter. Rolling over rewrote the week and bumped the counter, so where a task had been was lost, and "skipped" meant two different things. The same model treated a one-off goal's steps as milestones, which needed its own exceptions: any-order check-in, dates that don't push each other, and a rule that checking off a task never moves a step.

## Decision

- **Two concepts, not three.** A task is something you do, finished by checking it off. A milestone is something you reach, settled by checking in with a result. A one-off goal's steps are tasks; milestones belong only to building and quitting goals.
- **A task is one row for its whole life**, with an origin (by hand, weekly target, recurring item) and a state (open, done, dropped, lapsed).
- **Each week a task is in play is a `task_weeks` row:** the week, the planned day (empty means the bank), whether it was placed by hand, and how the week ended for it (done, rolled over, lapsed, dropped, set aside).
- **There is no plan table.** A day's plan, the bank, the upcoming list and the rollover count are all read from these rows.
- **Origin decides what an unfinished task does at the end of the week.** Weekly-target and flexible recurring tasks lapse; fixed-day recurring tasks and tasks added by hand roll over.
- **Generated tasks are unique** per goal or recurring item, period and number, so filling a week twice creates nothing. A "planned through week" on the user records how far filling has got; there is no weeks table.

## Consequences

- The record of which days work actually happened on, and how often something slipped, exists from the start, which is what learning a person's rhythm needs.
- Missed sessions no longer pile up: a week of 1 of 3 is followed by a week of 3, not 5. This reverses "everything rolls over" in the first flows.
- A one-off goal's steps appear in the plan and are finished from Today. This reverses the Nov 10 decision that steps are milestones marked on the goal's page.
- A task can be pinned to a day in a later week. The app's own draft still exists only for the current week, because target and recurring tasks are created when their week starts.
- Reads join two tables where they read one before, and a task's state has to be kept in step with its rows in the same transaction.
- When and how a week gets filled (a job, or the first request of the week) is left to the API.
