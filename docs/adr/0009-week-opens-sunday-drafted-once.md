# ADR-009: A week opens on Sunday evening and is drafted once

**Date:** 2026-11-16 · **Status:** Accepted; the draft narrowed by ADR-013

## Context

ADR-006 and ADR-008 left open when a week is filled and when the planner runs. Working through the planner's open questions against a set of ordinary weeks turned up three things:

- **"The planner writes only to days after today" and "fill the week on its first request" leave a day empty.** Open the app on Monday and nothing can be drafted onto Monday. A job at midnight doesn't help: by then today is already Monday.
- **"The plan adjusts continuously" can be read as redrafting after every change.** Traced through, that makes Thursday change because of a check-off on Tuesday, turns "Back to bank" into "moved to Thursday", and fills the room freed by finishing early with more work.
- **The week should work like a sprint, without the ceremonies.** On Sunday the person wants to see next week planned, and to be able to close the current week themselves.

## Decision

- **A week opens at 6 p.m. on the Sunday before it, phone time.** The first request at or after that creates the week's target sessions, recurring tasks and tasks now due, and drafts them onto days. From then the week can be read and rearranged.
- **A week closes when its Sunday ends,** on the first request on Monday or later: unfinished tasks lapse or roll over by origin (ADR-006).
- **Or when the person wraps it up.** From the moment next week is open, "Wrap up this week" does the same close by hand, and next week becomes the current one. It is never required and never prompted.
- **No job.** Opening and closing run on the first request that finds them due. Both rules take the day and time as inputs, so a job can call them once the API is hosted.
- **A week nobody opened is skipped.** Nothing is created just to lapse.
- **The app drafts a week once.** The first draft may use any day from today on. After it, the planner only places work that newly arrives (a carry-over, a new goal's sessions, a raised target, a new recurring item), on days after today that have room. It never moves what is already on a day and never refills a slot that came free.
- **One exception: lowering intensity.** It lifts the app's own goal-work placements off later days that are now over the cap. Raising it moves nothing.
- **The current week gets its own marker.** "Planned through week" runs a week ahead from Sunday evening, so the user also carries "closed through week", and the current week is read from that.

## Consequences

- "Back to bank" means the bank: a task sent there stays until the person places it. Finishing early leaves the time free.
- `task_weeks` needs no new column. The planner only ever places tasks created or carried over in the same transaction, so it never has to tell a task the person sent to the bank from one that didn't fit.
- What fits where is worked out on every read (a bank bigger than the room left, a day over its cap), so the app can point at room without moving anything.
- The rule that keeps Today still is restated: after a week's first draft, the planner writes only to days after today, and never to a row placed by hand. The first draft is a plan appearing, not a plan changing.
- Opened first on a Wednesday, a week is drafted from Wednesday on, and a fixed-day item whose day has passed goes on Wednesday.
- Carry-overs arrive after next week's first draft unless the week was wrapped up or nobody opened the app on Sunday evening. They are placed on later days with room, or stay in the bank.
- Three weeks away reads as silence, not three weeks of lapsed sessions. The cost: a fixed-day item due in the gap (rent on the 1st) never becomes a task.
- Two weeks can be open at once, from Sunday evening to the close. Pausing a goal and adding a task have to act on both.
- The Week screen gains a Next week view and the wrap-up. "The app's own draft exists only for the current week" (ADR-006) no longer holds.

## Revisited 2026-11-19: only recurring work is placed

[ADR-013](0013-place-only-recurring-work.md) narrows the draft. When a week opens, the app places only its recurring work; tasks now due, a one-off goal's steps and carry-overs wait in the bank. Later arrivals that are recurring work are placed on days after today; carry-overs go to the bank. Lowering intensity no longer lifts anything off a day. Opening, closing, wrapping up and the two markers are unchanged.
