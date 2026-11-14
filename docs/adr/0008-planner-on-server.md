# ADR-008: The planner runs on the server

**Date:** 2026-11-13 · **Status:** Accepted

## Context

Planning means filling a week's bank, rolling over and lapsing what wasn't finished, and drafting tasks onto days under the intensity cap. The click-through put planning and pacing rules in the front end's `domain/` modules, because there was no server to hold them. Three things point the other way now:

- The plan is data. A day's plan, the bank and the rollover count are read from `task_weeks` (ADR-006), and the server is the source of truth for it (ADR-004).
- "A plan exists even if you never open it." A plan made on the phone exists only once the phone has opened the app.
- The phone and the laptop must show the same plan. Two devices each drafting would have to agree, or be reconciled.

## Decision

- **The API owns planning:** filling a week, rollover and lapse, drafting days, and the cap.
- **The front end shows the plan and sends what the person did:** check off, back to bank, pull, swap, place on a day. Each is a small change to one or two rows, applied on the screen at once and sent to the API (ADR-004). None of them is planning.
- **Nothing is drafted on the device.** Offline, the phone shows the last plan it has and queues the person's changes.

## Consequences

- The planning rules are written once, in Python, and tested against Postgres.
- The front end's `domain/` keeps what it needs to draw a screen (labels, progress, order within a day), not the rules that decide what goes where. Its `CLAUDE.md` is updated to say so.
- After a change made on the phone, the server may redraft later days, and the phone sees that on its next refresh. Today still never changes on its own: the planner writes only to days after today and never to a task placed by hand.
- A new week's plan can't be made offline. Opening the app offline on a Monday shows last week's plan until it can reach the API.
- When the planner runs (on the first request of a week, after each change, or as a job) is settled in the planner's spec in the API repo.
