# Brief: scheduling and other weeks

**Status:** designed with Jan on 2026-11-21, handed over 2026-11-23. This is the "what"; issues, specs and plans are Claude Code's, in both repos.

**Issues (2026-11-23):** API [#50](https://github.com/jan-mcsparin/goals-to-actions-api/issues/50) (on a day), [#51](https://github.com/jan-mcsparin/goals-to-actions-api/issues/51) (any week, fullness, late check-offs), [#52](https://github.com/jan-mcsparin/goals-to-actions-api/issues/52) (Coming up); front end [#79](https://github.com/jan-mcsparin/goals-to-actions-web-frontend/issues/79) (Add, on a day), [#80](https://github.com/jan-mcsparin/goals-to-actions-web-frontend/issues/80) (any week), [#81](https://github.com/jan-mcsparin/goals-to-actions-web-frontend/issues/81) (past days), [#82](https://github.com/jan-mcsparin/goals-to-actions-web-frontend/issues/82) (move and remove), [#83](https://github.com/jan-mcsparin/goals-to-actions-web-frontend/issues/83) (Coming up). Order: #50 → #79, #51 → #80 and #82, #81, #52 → #83. Specs (2026-11-23): [`api/specs/on-a-day.md`](../../api/specs/on-a-day.md) for #50 and [`web-frontend/specs/079-on-a-day.md`](../../web-frontend/specs/079-on-a-day.md) for #79, decisions settled; the rest are written in order. Building comes after step 5 of the front-end hookup.

**Design:** [`../screens/scheduling.png`](../screens/scheduling.png), and the "Scheduling and other weeks" section of [`../screens/README.md`](../screens/README.md). Jan also has a click-through prototype on a claude.ai design canvas ("Goals to Actions prototype", private to him); the image and this brief are what to build from.

## What and why

The app sees about one week ahead. A task due later waits in the upcoming list, Week shows this week (and next week from Sunday evening), and there's no way to say "the dentist is Dec 12 at 3 p.m., put it there," or to look at last week. That's a standard organizer feature, and its absence was an oversight.

Five pieces, smallest first:

1. **On a day:** put a task on any day from today on, with an optional time, and it stays there.
2. **Any week on Week:** ‹ › arrows to step back to past weeks and forward to later ones.
3. **Late check-offs:** open a past day and tick off what you did but didn't mark.
4. **Coming up:** a page showing the next 12 weeks, each with how full it looks and what's set or due in it.
5. **Fullness:** a small bar for each week and for each day of an opened week, so you can see at a glance how full things are.

## What the person can do

### On a day (Add sheet)

- The Task tab's placement becomes one **When** choice: **On today · In the bank · On a day**. On today is preselected from Today, In the bank elsewhere (as now).
- **On a day** shows a **Day** (today or later) and an optional **Time** ("at" a time). The due date field is hidden: the day is the constraint.
- Saving says where it went: "Set for Sat, Dec 12."
- **What it means:** the task is placed by hand on that day. In this week or an open next week, that's an ordinary hand placement. In a later week it's the "pinned to a later week" row `data-model.md` already allows ("the model allows it; the app doesn't offer it yet"). When that week opens, the planner places recurring work around it and never moves it.
- A future day's set things can be **moved** to another day (a date picker) or **removed** (dropped, with nothing counting against you).

### Any week (Week)

- The header gets **‹ ›** arrows, the week's title ("This week", "Last week", "Next week", else "Week of Dec 7") and its range. They replace the This week | Next week toggle. **Wrap up this week** stays on Next week, from 6 p.m. Sunday, as now.
- **How far:** back to the first week with any rows; forward 12 weeks, matching Coming up.
- Under the title, every week shows its **counts line and fullness bar** (below), then **This week** (when not on it) and **Coming up ›**.
- **This week and past weeks:** Progress, days and the bank as now. Each day also gets a **day bar** (below). Past days stay dimmed; "Full" and "done" stay as they are.
- **A week that hasn't opened:** a note ("Only what's set to a day shows here. Recurring work is placed when this week opens, on the Sunday evening before."), no Progress, no bank, no day bars. Each day lists what's already known: tasks set to it, fixed-day recurring items due that day (the planner knows them), milestone check-in dates and goal deadlines. Empty days read "Nothing set yet".
- Tapping any day opens it, as now.

### A past day (late check-offs)

- A note at the top: "This day has passed. You can still tick off what you did."
- Everything that was on that day shows, done or not. An unfinished task reads "Moved on to the next week · tap if you did it" (rolled over) or "Tap if you did it" (lapsed).
- **Tapping checks it off as done on that day.** This uses the late check-off rules `data-model.md` already has: a rolled-over task's check-off lands on its row in the week it moved to, so it leaves this week's bank; a lapsed one turns its row and task to done. The difference is that a late check-off can now come from any past day the person opens, not only one queued offline. Unchecking works the same way round.
- Nothing else is offered on a past day (no swap, no pull, no placing), as now.

### Coming up

- A page reached from **Coming up ›** on every week's header and from a new line at the bottom of Today: "Coming up: Haircut · Wed at 4:30 p.m. ›" (the next thing set to a day or due after today; left out when there's none).
- Title "Coming up", subtitle "The next 12 weeks, starting next week". One row per week, two lines:
  - **Line 1:** the range in bold; then, only when the week has any, a hollow ring and "1 check-in", "2 deadlines" or "1 check-in · 1 deadline"; the week's fullness bar; ▸ or ▾. The marker text is the one thing that may trim with an ellipsis on a narrow phone; the range and bar never do.
  - **Line 2:** the counts line (below).
- **The first two weeks start open; the rest closed.** Tapping a row opens or closes it. Not remembered between visits.
- **An open week lists its dated things**, by day: the date once per day ("Wed 25"), then a dot, the title and what it is ("at 4:30 p.m.", "Due", "Step · Plan the ski trip", "Milestone 3 of 4 · opens the goal"), and a chevron. A week with none says "Nothing set to a day". Then **See the week ›**.
  - **Listed:** tasks set to a day in that week, tasks due that week (from the upcoming list or the bank), a one-off goal's steps due that week, milestone check-in dates, and goal deadlines (opt-in ones only).
  - **Not listed:** recurring work. It shows on each day of the week instead.
  - **Where a tap goes:** a task opens its day; a check-in or deadline opens the goal's page.

### Fullness: the counts line and the bars

- **What's counted is tasks**, by where they come from, which is what the planner and the day cap already count. No time estimates.
  - **Goal work** (dark blue): tasks from weekly targets. "8 for 3 goals": sessions, and how many goals they're for. Count targets only, as the Week progress rows (shared decision 16: amounts left off for now).
  - **Upkeep** (mid blue): tasks from recurring items, with or without an aspiration.
  - **Added** (pale blue): everything added by hand, a one-off goal's steps included. (The design image still says "set"; the label is "added", settled Nov 23.)
- **Check-ins and deadlines are never counted.** They aren't tasks; they show with a hollow ring instead of a filled dot.
- **The counts line:** "● 8 for 3 goals · ● 5 upkeep · ● 2 added", each dot the colour of its segment, numbers in dark text (the paler blues aren't readable as text). It never wraps.
- **Week bar:** 64 × 10 px, three segments in that order, against **a typical week of 25**: a fixed number, independent of intensity, so weeks compare with each other. Past 25 it's simply full: no red, no warning. Spoken as "About 16 things, against a typical week of 25".
- **Day bar** (days of opened weeks only): 40 × 8 px, the same segments, against **the day cap from intensity** (3, 4 or 6), so a full bar and the existing "Full" mark always agree.
- **Which numbers:** for this week and past weeks, the week's rows. For a week that hasn't opened, the expected tasks: weekly-target sessions of the goals active that week, the recurring items due that week (as the planner would place them), and the hand-added tasks set to or due in that week.
- Colours: `#1E4E8C`, `#4F86C6`, `#A9C5E8`, outline `#9DB4D3`. Blue, not green: green means done in the progress bars.

## Rules (also in `product-rules.md`)

- **On a day is a hand placement.** It stays put; the planner plans around it.
- **Fullness is a guide, like the cap.** Nothing is refused, moved or warned about because a week or day looks full.
- **Weeks that haven't opened show only what's known.** No guessed plan that would change later.
- Tone: no "overdue", "missed" or "behind" anywhere, including late check-offs ("tap if you did it").

## Suggested work

A suggestion for issues and specs; split or merge as the code suggests. The API pieces are additions to existing slices, not new tables.

**API**

1. **Pin a task to a later day.** Create a task with a planned day in any future week (a hand-placed row in a week that hasn't opened), move it, drop it. Opening a week keeps those rows. Covers On a day beyond the open weeks.
2. **Read any week.** Past weeks with their rows and outcomes; a week that hasn't opened with its pinned rows, its fixed-day recurring occurrences, milestone dates and deadlines. Every week read carries the counts by origin (actual or expected) and, for opened weeks, per day.
3. **Late check-off from a past day.** Check off and uncheck a task for a day in a closed week, as the late-arrival rules already do, whenever it's sent. Confirm the rolled-over and lapsed cases.
4. **Coming up read.** The next 12 weeks in one call: each week's expected counts, its dated items and its markers. It could share code with 2.

**Front end**

1. **On a day in the Add sheet** (needs API 1).
2. **Week: arrows, any week, counts line and bars.** Replaces the toggle; Wrap up stays (needs API 2).
3. **Past days: late check-offs** (needs API 3).
4. **Future days: set items with Move and Remove** (needs API 1, 2).
5. **Coming up, with links from Week and Today** (needs API 4).

**Suggested order:** API 1 → FE 1, then API 2 → FE 2 and 4, then API 3 → FE 3, then API 4 → FE 5. The first pair is the most useful on its own: things can be put on a future day even before the views to see them exist.

**Docs to update as they're built:** `flows.md` (flows 3, 4 and 5 already note this), `screens/README.md`, `data-model.md` (the pinned row is offered now), `product-rules.md`.

## Settled with Jan, 2026-11-23

1. **The third count's label is "added".** "Set" also counted hand-added tasks in the bank and tasks that are only due, neither of them set to a day.
2. **How far back the arrows go:** to the first week with any rows. Capped only if a long history turns out slow to read.
3. **Other weeks are read online only.** The phone stores the current week and an open next week (ADR-010), so Today and Week open instantly as before. Past weeks, later weeks and Coming up are read when shown; offline they say so gently. ADR-010 gets a note when this is built.
4. **When:** after step 5 of the front-end hookup (specs 017, 037, 076 with API #49, and 038), which this design doesn't change, and before infra and hosting.

## Notes for the specs

- **Remove vs. Delete (FE 4).** Spec 038 keeps Drop out of a task's menu so it never sits beside "Delete (made by mistake)". Remove on a future set item is a drop, in that menu. FE 4's spec settles how the two read side by side.
- **Moving a fixed-day task for one week (FE 4).** A holiday moves trash day from Tuesday to Wednesday: the task moves for that week and the item keeps its day (Jan, 2026-11-25). The API refuses it today ("This one stays on its day", `keep_on_its_day` in `api/app/tasks/rules.py`), so it needs an API change with FE 4. Its ⋯ menu holds only Delete until then (spec 038).
- **Quitting goals.** The quit-day row isn't a task: fullness never counts it. Once past weeks open (FE 3), a past day may show the quit row too, sent online only.
- **A week nobody opened** can now hold a pinned row. Closing already works from the user's markers, not from rows, so a task pinned in a skipped week carries into the week the person comes back to. API 1 tests it and corrects `api/specs/week-turnover.md`.

## Open questions

1. **Notifications for set things** (a reminder before the dentist) aren't part of this. Notifications are off app-wide for now.

## Not in this

A month grid, hour-by-hour time blocks, and syncing with Apple Calendar. They're worth revisiting once the app is hosted; for now the app plans days, not hours. A time of day on recurring items (shared decision 17) stays for later too.
