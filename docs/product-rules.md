# Product rules

The rules every screen and every piece of planning logic must follow. Short on purpose; the reasoning lives in the claude.ai project's design principles, and the behavior details in `flows.md`. If code and this file disagree, fix one of them in the same change.

## North star

Help a person keep moving their life forward and improving over years, while also being happy. Not a short, grandiose burst of progress. Success is measured over months and years.

## The model

- **Aspiration:** who you want to be, plus why. Has several goals (Be fit: Be strong, Have endurance, Look good). Never ends, but can be **retired**: set aside with its history, and brought back anytime. Ordered by priority.
- **Navigation follows the model:** Aspirations tab → aspiration → goal → milestones. Goals with no aspiration sit under "Other goals".
- **Keep life running** is the special case beside the aspirations: what has to happen but rolls up to no goal or aspiration (laundry, the trash, a library book, the insurance).
- **Goal:** a label plus a **measurable line** it could cross. No forced end date; it stays active until reached or ended. Type is picked first, with one tap, and never inferred:
  - **Building:** flexible weekly target (e.g. 3 lifting sessions).
  - **Quitting:** start date, daily yes/no, occasional urge rating. Reached when the urge has stayed low, not after N days.
  - **One-off:** an optional deadline plus a short list of steps. The steps are tasks ("Step 3 of 6"): checking one off, on Today or on the goal's page, in any order, is what moves the count. A one-off goal has no milestones.
- **Milestones:** benchmarks with a measurable line and optional date, on building and quitting goals. They prove improvement from a **baseline**. The goal itself is the final milestone, so a goal with none of its own reads "0 of 1".
- **A task is something you do; a milestone is something you reach.** There is no third kind of thing.
- **Recurring items:** maintenance or ongoing practices, fixed-day (trash every Tuesday) or flexible (laundry weekly). Can sit under an aspiration: then it is planned as goal work, and with none it is maintenance.
- **Tasks:** belong to a goal, a recurring item, or nothing. Optional due date and optional time of day ("due by" / "at").
- **Drop and delete are different.** Dropping a task is deciding not to do it; it stays in your history. Deleting is for mistakes, and is offered on tasks, milestones, goals, aspirations and recurring items.

## Planning

1. **Weeks start Monday.** When a week opens, its bank fills with goal tasks, recurring items, rollovers and upcoming tasks now due.
   - **Next week opens on Sunday evening.** Its recurring work is placed then, and you can look at it and arrange the rest.
   - **You can wrap the week up yourself** from Sunday evening; otherwise it closes when Sunday ends. Wrapping up is never asked for.
   - **Missed sessions don't pile up.** Unfinished weekly-target tasks and flexible recurring items lapse; the new week or period brings fresh ones. Tasks you added yourself and fixed-day items (rent on the 1st) roll over.
2. **The app places recurring work; you place the rest** (ADR-013). Fixed-day items go on their day. A goal's weekly sessions and flexible recurring items are spread evenly across the week. Everything else (tasks you added, a one-off goal's steps, tasks now due, carry-overs) waits in the bank for you to put on a day.
3. **Free time is protected** by a cap on tasks per day, set by intensity: 3, 4 or 6. The cap is a guide: Week shows a day that reaches it as full, and the app never moves or refuses anything because of it. No per-day capacity setting, no time estimates.
4. **The app places a week's recurring work once; after that the plan is yours.** No finalize time; you can look ahead and edit any day. New recurring work (a new goal's sessions, a raised target, a new recurring item) is placed as it arrives, on later days. The app never moves what is on a day, except the tasks of a recurring item you change, which follow the change; and it never refills a slot you freed: a task sent back to the bank stays there, and finishing early leaves the time free.
5. **Today only changes because of something you did** (skip, swap, finish early, pause a goal, wrap up the week). Never on its own.
6. **Anything you put on a day stays there.** The app plans around it. The one exception is your own: pausing or ending a goal, turning a recurring item off, or retiring an aspiration and pausing its work, sets that work's unfinished tasks aside until it comes back.
7. Timed tasks anchor the day; untimed tasks float around them.
8. **A plan exists even if you never open it.**
9. If a day reaches the cap, Week shows it as full. Nothing is crammed or moved.
10. **On a day is a hand placement** (designed Nov 21; built in the API, the Add sheet to come). A task can be put on any day from today on, with an optional time; it stays there, and the planner places recurring work around it when its week opens. A task added by hand can be moved to a day in another week, or removed.
11. **Fullness is counted in tasks, by origin** (designed Nov 21; counts built in the API, the screens to come): goal sessions, upkeep (recurring items) and tasks added by hand. A week is measured against a typical week of 25, fixed so weeks compare; a day against the intensity cap, so its bar agrees with "Full". Check-ins and deadlines are never counted. Like the cap, fullness is a guide: nothing is refused, moved or warned about.
12. **A week that hasn't opened shows only what's known** (designed Nov 21; built in the API, the screens to come): what's set to a day, fixed-day recurring items, milestone dates and deadlines, and expected counts. Never a guessed plan.

## Tone and prompts

- **Ask, don't direct.** The only unprompted questions: "Still matter?" (a task carried over for two weeks: keep, schedule, drop) and "This one slipped" (an opt-in deadline passed: push it back, or end the goal for now).
- **Quitting goals never count silence against you.** A day with no answer is unknown, not a slip. The urge rating (low, some, strong) sits quietly beside the daily yes/no on some days and can be ignored; it isn't a prompt.
- **No review prompts, and no Review.** A goal is changed (target, pause, end) from its page when the person wants.
- **No pace prompts.** Intensity (Going hard / Normal / Easing off) is set by the person, defaults to Normal, and stays until changed.
- **No shame.** Rollovers carry no penalty. Falling short is **"not yet"**: lead with progress since baseline, then offer keep / move / adjust.
- When a goal is reached, lead with what was achieved, then ask what's next, and offer to keep the practice as a recurring item.
- **Deadlines are opt-in.**

## Realistic by default

Targets start from the baseline and ramp gradually. In the prototype the person enters targets; any later suggestions must follow this rule.

## Out of scope for the prototype

Native iOS, sign-up and open sign-up, AI suggestions, calendar and watch integrations, partial credit on tasks, one task serving two goals, and the full safety principles (required before anyone the founder doesn't know uses the app, or before the app suggests much on its own). A few invited testers the founder knows can use it before then (ADR-012, revisited 2026-11-19).
