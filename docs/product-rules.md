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
  - **One-off:** an optional deadline plus a short list of steps. The steps are its milestones ("Step 3 of 6"): you mark a step reached on the goal's page; checking off a task doesn't.
- **Milestones:** benchmarks with a measurable line and optional date. They prove improvement from a **baseline**. The goal itself is the final milestone, so a goal with none of its own reads "0 of 1".
- **Recurring items:** maintenance or ongoing practices, fixed-day (trash every Tuesday) or flexible (laundry weekly). Can sit under an aspiration.
- **Tasks:** belong to a goal, a recurring item, or nothing. Optional due date and optional time of day ("due by" / "at").

## Planning

1. **Weeks start Monday.** At the start of each week the bank fills with goal tasks, recurring items, rollovers and upcoming tasks now due.
2. The app drafts the whole week: **maintenance first**, then goal work, leaving free time. A day is never packed full.
3. **Free time is protected** by a cap on tasks per day, set by intensity. No per-day capacity setting, no time estimates.
4. **The plan adjusts continuously.** No finalize time; you can look ahead and edit any day.
5. **Today only changes because of something you did** (skip, swap, finish early, pause a goal). Never on its own.
6. **Anything you put on a day stays there.** The app plans around it. The one exception is your own: pausing a goal, or retiring an aspiration and pausing its work, sets that work's unfinished tasks aside for the week.
7. Timed tasks anchor the day; untimed tasks float around them.
8. **A plan exists even if you never open it.**
9. If the bank holds more than the week can, or a day is overloaded, say so and suggest what to move; don't quietly cram.

## Tone and prompts

- **Ask, don't direct.** The only unprompted questions: "Still matter?" (a task carried over for two weeks: keep, schedule, drop) and "This one slipped" (an opt-in deadline passed).
- **No review prompts.** Reviews happen from a goal's page when the person wants.
- **No pace prompts.** Intensity (Going hard / Normal / Easing off) is set by the person, defaults to Normal, and stays until changed.
- **No shame.** Rollovers carry no penalty. Falling short is **"not yet"**: lead with progress since baseline, then offer keep / move / adjust.
- When a goal is reached, lead with what was achieved, then ask what's next, and offer to keep the practice as a recurring item.
- **Deadlines are opt-in.**

## Realistic by default

Targets start from the baseline and ramp gradually. In the prototype the person enters targets; any later suggestions must follow this rule.

## Out of scope for the prototype

Native iOS, other users and sign-up, AI suggestions, calendar and watch integrations, partial credit on tasks, one task serving two goals, and the full safety principles (required before anyone else uses the app).
