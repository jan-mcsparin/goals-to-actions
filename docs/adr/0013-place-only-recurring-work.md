# ADR-013: The app places only recurring work; the rest waits in the bank

**Date:** 2026-11-19 · **Status:** Accepted

## Context

ADR-008 and ADR-009 have the server draft the whole week onto days when it opens: maintenance first, tasks with a due date by their date, then goal work by priority, all under the intensity cap. Writing up the planner slice showed what that costs. It is the slice with the most rules, and every guess it makes about which day a task belongs on is one the person may have to undo. For a first version used by one person, placing a task by hand is cheaper than undoing a wrong placement.

Some work has an obvious place, though. A fixed-day item has one right day. Repeating work like workouts only needs spreading across the week.

## Decision

- **The app places recurring work, and nothing else:**
  - a fixed-day recurring task goes on its day;
  - a building goal's weekly sessions and a flexible recurring item's tasks are spread evenly across the week (3 a week: Monday, Wednesday, Friday).
- **Everything else waits in the bank** for the person to place: tasks added by hand, a one-off goal's steps, tasks now due, and carry-overs.
- **The cap is a guide, not a rule.** Intensity still sets 3, 4 or 6 tasks a day, and Week shows a day over it as full (later made "once it reaches it": `flows.md`, decided Nov 19). The app never moves or refuses anything because of it, and changing intensity moves nothing.
- **When placement happens is unchanged from ADR-009,** except for carry-overs. Recurring work is placed when a week opens, and after that as it arrives (a new goal's sessions, a raised target, a new or changed recurring item), on days after today. Carry-overs go to the bank.
- **Smart placement can come later** as something the person asks for, such as a "suggest a plan" button. The bank-first design leaves room for it.

## Consequences

- The planner slice shrinks to the placement of recurring work and the full-day mark on the week's read. There is no due-date placement, no fitting under the cap, no taking turns between goals and no lifting work off days when intensity drops.
- Monday's Today holds recurring work and whatever the person placed. Placing the rest is a few taps on Sunday evening, on Next week, or as the week goes.
- The rules that keep Today still are unchanged: the app never writes to a task placed by hand, and after a week's first placement it writes only to days after today.
- "The bank holds more than the week can" is no longer worked out or suggested about. The bank is the person's list to place from, and nothing in it is late.
- ADR-008 still holds: what the app does place, it places on the server.
