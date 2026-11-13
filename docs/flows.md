# Goals to Actions: Core Flows

Source of truth for how the app behaves, flow by flow. Moved here from the claude.ai design doc on 2026-11-09; update it in the same change as any behavior change. Data model: `data-model.md`. Screens: `screens/README.md`.

Eight flows cover the prototype: two you do rarely (aspirations, goals), and six that make up the daily and weekly rhythm. Each lists the steps, what the app does for you, and where it branches. In the prototype, you enter aspirations, goals, milestones and targets yourself; the app's automation goes into filling the weekly bank and planning each day, where it pays off most.

## At a glance

![Core loop](screens/core-loop.png)

Aspirations and goals are set up rarely; after that, the bank, the daily plan, checking off and progress repeat every week, with rollovers feeding unfinished work back into the bank. Changing a goal from its page can also send you back to it when something has changed.

## Flow 1: Name an aspiration

Rare: on first use, or when something changes. It captures why you want what you want.

1. Tap **+** and pick **Aspiration**, or create one while setting a goal (flow 2).
2. Write it in your own words: "Be fit."
3. The app asks one question: "What would this give you?" Your answer becomes the why: "so I can keep skiing, biking and playing hockey at a high level for decades."
4. It goes last in priority. Move it up on its page if it should come first when aspirations compete for time.
5. Saved. The sheet stays open on "Saved." with **Add a goal for this**, which leads to flow 2 with the new aspiration selected, and **Done**.

Branches:

- **Skip the why:** the aspiration saves anyway. Add it later from the aspiration's page.
- **Start from a goal instead:** flow 2 can create the aspiration on the spot, or leave the goal without one.

**Managing aspirations afterwards** (from the aspiration's page, reached from the Aspirations tab):

- **Edit** the name and why at any time.
- **Reorder:** move up or move down. Priority order drives which goal work comes first when the week is tight, and the order on Week and in Today's by-goal view.
- **Retire:** the sheet leads with what the aspiration has seen (goals reached, goals active, recurring items), then asks one thing: pause its active goals and recurring items (default), or keep them going without an aspiration. Pausing them takes their unfinished tasks off this week's days and out of the bank, including ones you placed by hand; finished tasks stay. The aspiration moves to Retired, history intact. Nothing counts against you.
- **Bring back** a retired aspiration: active again, last in priority. Goals it paused stay paused until you resume them, and recurring items it paused stay off for now.

## Flow 2: Set a goal and its milestones

Rare: a few times a year per aspiration. It defines concretely what reaching the aspiration means to you. One aspiration can have several goals.

1. **Pick the goal type** first: building, quitting or one-off. It's one tap, and it shapes the questions that follow. Nothing else shows until it's picked, and it can be changed before saving. The app never guesses it from the sentence.
2. From an aspiration (**+ Add a goal for Be fit** on its page) or on its own (**+**, then **Goal**), type one sentence: "Be strong: bench 175, squat 350, deadlift 400."
3. The app splits it at the first colon into a **label** ("Be strong") and a **measurable line** (the numbers), and shows both back as you type. With no colon, the sentence is the label, and it asks for the line: "How will you know you've got there?" Every goal needs a finish line it could reach. There's no forced end date, though: a goal stays active as long as it's needed, until it's reached or you end it.
4. **Baseline:** "Where are you now?" (bench 145, squat 275, deadlift 315). Realistic targets depend on this.
5. **Milestones:** you add checkpoints, each with a measurable line and an optional date (week 8: bench 155, squat 300, deadlift 345). A date falls between the goal's start and its deadline. The goal itself is the final milestone. In the app the short Add form saves the goal first and opens its page: "Saved. Add milestones when you're ready." ("steps" for a one-off goal). Milestones are added there; a one-off goal has steps instead, which are tasks. Back from that page goes to the goal's aspiration, or to the Aspirations tab when it has none, whichever screen the + was tapped over. App suggestions come later.
6. **Optional deadline** for the goal, whatever its type. Opt-in only, and never before the goal's start date.
7. **Weekly target:** you enter it, and change it on the goal's page. It's either a count ("3 lifting sessions a week") or an amount over a number of sessions ("25 miles over 4 runs"). A count is met by tasks done; an amount by the values logged on them. A change takes effect this week, so "1 of 3" can become "1 of 4" midweek; earlier weeks keep the target they had. App suggestions come later.
8. The app puts this week's tasks for the goal into the bank (flow 3).

Branches:

- **No aspiration yet:** the app asks "What's this for?" You can link one, create one, or skip.
- **Quitting goal:** instead of a weekly target, set a start date (today unless changed) and a daily all-or-nothing commitment. The finish line is the habit being broken, not a set number of days: you've quit when you don't really want one anymore.
  - **The daily answer** is held or slipped. A day with no answer is unknown, not a slip, so a week away isn't seven failures. It isn't a task: it never enters the bank or counts against the day's cap.
  - **The urge rating** is low, some or strong. It sits beside the daily yes/no on some days and can be ignored; it's never a prompt.
  - **Reached** when you say so. The app can point out that the urge has stayed low for a while, but never marks the goal reached itself.
- **One-off goal:** set an optional deadline and a short list of steps. The steps are tasks that belong to the goal ("Step 3 of 6" is tasks done out of the goal's tasks), done in any order, since steps rarely depend on each other. Checking one off on Today finishes the step; there's nothing to mark a second time. A step that takes several sittings stays one unchecked task until it's finished, or you split it. A one-off goal has no milestones.

## Flow 3: Fill the task bank

Mostly automatic, once a week. The bank holds everything to get done this week, before it's assigned to days.

1. At the start of each week, the app fills the bank with:
   - goal tasks from weekly targets (3 lifting sessions),
   - steps from one-off goals that are due this week (a step is a task, so it arrives like any task with a due date),
   - recurring maintenance due this week (laundry, rent),
   - anything that rolled over from last week (flow 7), and tasks from the upcoming list that are now due.
2. Anytime, you can add a task by hand (**+**, the Task tab): a title, and optionally the goal it serves, a due date and a time of day ("by" or "at" a time). If it's due this week or earlier, or has no due date, it goes straight into this week's bank, or onto today if you choose. **On today** is preselected when you add it from the Today screen, not from a day opened on Week, even when that day is today. A new task never goes onto any other day. If it's due later, it waits in an upcoming list and moves into the bank at the start of the week it's due (or a week or two early, for things that need lead time). A short line says where it went: "Added to this week's bank.", "Saved for the week of Nov 23." or "Added to today."
3. **Maintenance is set up once:** a name, a repeat and how fixed it is. Fixed-day items happen on a set day ("take out the trash every Tuesday," "rent on the 1st"), and the plan always puts them there. Flexible items just need doing sometime in the window ("laundry once a week," "clean the bathroom every two weeks"), and the app places them on whichever day fits. Only the things that actually slip go in; automatic habits stay out. The same kind of recurring item can also keep up a practice after its goal ends (lifting 3 times a week, under Be fit).
   - **Adding one** (**+**, the Recurring tab, or **+ Add recurring** on an aspiration's page, which selects that aspiration): a name, then **Fixed day** or **Flexible**. Fixed: every week on a weekday, or every month on the 1st to the 28th, so every month has the day. Flexible: once a week, 2 to 6 times a week, every two weeks, or once a month. The repeat reads back as "Every Tuesday", "Monthly on the 1st", "3 times a week", "Every two weeks".
   - **Under an aspiration** is optional. With none, the item belongs to Keep life running.
   - **When it's first due:** a fixed item on its next day, today included when today is that day; a flexible item from the day it's added. "Added. It will show up in the plan when it's next due."
   - Yearly repeats, "every N weeks" and a time of day ("Thursdays at 8 p.m.") aren't offered yet.

Branches:

- **Later weeks:** the data model lets a task sit on a day in a later week, but the app doesn't offer it yet; a later task waits in the upcoming list. The app's own draft exists only for the current week.
- **A task with no goal:** fine. It's maintenance or a one-time life task ("fill out my ballot"), usually with a due date, and still part of the day. Things that come around once a year can be yearly recurring items instead, once those are offered.
- **More in the bank than the week can hold:** the app says so, and suggests what to push to next week or drop.

## Flow 4: Plan today

The heart of the app, and the most automated part. Planning should mostly be a glance and an "OK."

1. **Start of the week:** the app drafts the whole week from the bank. It places maintenance on its due days first, spreads goal work across the week (learning your rhythm over time), leaves free time, and follows your intensity setting (flow 8).
2. **Throughout the week:** the plan updates continuously as tasks get done, skipped or moved, so you can look ahead at any day and adjust it. Today's plan changes only because of something you did.
3. **Whenever you open it** (often the next morning): today's plan is shown in three groups: keeping life running, moving forward, and free time. Free time is the time tasks don't fill; it holds no tasks.
4. You can:
   - **accept it** by doing nothing,
   - **swap** a task for one from the bank,
   - **skip** a task, which sends it back to the bank (shown as "Back to bank"),
   - **pull extra** work forward from the bank.
5. Any change reflows the rest of the week's draft.

Branches:

- **You never open the plan:** it still stands. Skipping planning doesn't mean having no plan.
- **A day is overloaded:** the app says so and suggests what to move, instead of quietly packing it full.
- **You edit a future day:** it stays there, and the app plans the rest of the week around it.

## Flow 5: Do and check off

Daily, and the most frequent interaction. It has to take one tap.

1. Each task on today's plan shows the goal it serves and, where there is one, the aspiration behind it ("Lift: bench day · Be strong · Be fit").
2. Tap to mark it done. Tap again to undo a mis-tap.
3. For tasks with a number attached, an optional quick value (miles, weight, minutes): one field, skippable.
4. Pacing updates right away: "2 of 3 lifting sessions, 3 days left." For an amount target it's the total logged: "14 of 25 miles".

Branches:

- **Did something that wasn't planned:** log it as done. It counts the same as planned work.
- **Partly done:** for the prototype, a task is done or not. Partial credit can come later if it's missed.

## Flow 6: See progress and check in at milestones

Whenever you want to look, plus a check-in at each milestone. It shows improvement, not just effort.

1. **Per goal:** this week's pacing, milestones hit, and how far you've come from your baseline. On the goal's page:
   - **Where you started:** "Started at" (the baseline) and "Latest" (the most recent check-in result), side by side as text. Nothing is calculated, since both are free text.
   - **The timeline** runs from Start through each milestone to the goal itself, with "1 of 6 hit" beside it. Each milestone shows its week ("Wk 9 · Nov 30"; the week the goal starts is Wk 1), or "No date".
   - **Next** is the first milestone that isn't hit. One marked "not yet" stays Next until it's hit, so a check-in never passes over a milestone. Once every milestone is hit, the goal is Next.
2. **Overall:** each aspiration with its goals, so you can see the whole picture at a glance.
3. **Milestone check-in:** whenever you choose, tap Check in under Next check-in: "How did it go?", an optional result, then **Hit it** or **Not yet**. Milestones are checked in in order, so only Next can be. Each check-in is kept, so results have a history. A one-off goal has no check-ins: its steps are tasks, checked off like any other.
4. **Milestone hit:** a short "Milestone hit: …" line, and Next moves to the following milestone right away, so progress points forward. With every milestone hit, the goal itself is Next; hitting it marks the goal reached, exactly as End goal → I reached it does.

Branches:

- **Not yet:** the milestone is marked not yet with your result, and it stays Next. A card on the goal's page leads with progress since your baseline ("You started at … Now: …"), then offers: **keep the date**, **move it**, or **adjust the milestone** (its line and date). Each puts it back to upcoming. The card stays until you pick one, and never appears anywhere else.
- **Moving a date pushes the later ones out:** when a milestone's date moves later, every later milestone with a date moves by the same number of days, never past the goal's deadline (one that would lands on the deadline). Undated ones stay undated, and moving a date earlier pulls nothing in.
- **A milestone's date passes:** nothing changes by itself, and there's no badge. It reads like any other date until you check in.
- **No date on the milestone:** Next check-in reads "Whenever you choose."

## Flow 7: Rollovers and light reviews

Mostly automatic. The app asks short questions only when something suggests it should, and never makes you do a review.

**Weekly rollover (end of each week):**

1. Unfinished tasks you added yourself, a one-off goal's steps included, move into next week's bank, with no penalty. So do fixed-day recurring items: rent missed on the 1st still has to be paid.
2. Unfinished weekly-target tasks lapse: a week of 1 of 3 is followed by a week of 3, not 5. A flexible recurring item lapses at the end of its period (laundry not done this week is replaced by next week's). Nothing counts against you, and the miss stays in your history.
3. When a task has been carried over for two weeks, the app asks: "Still matter?" Keep it, reschedule it, or drop it. After Keep, it asks again only once the task has carried over two more weeks. A dropped task stays in your history; it isn't deleted.

**Changing a goal (only when you choose to; the app doesn't prompt):**

4. From a goal's page: change the weekly target, **Pause** or **Resume** it, or **End goal**. Pausing is immediate: this week's unfinished tasks for the goal leave the days and the bank (done ones stay), and it drops out of Week's progress. Resuming returns those tasks to this week's bank. A goal whose aspiration is retired can be resumed too; it keeps the aspiration and shows under Other goals until the aspiration is brought back.
5. Neither goals nor aspirations have a Review (decided Nov 10). A review's choices (adjust the target, pause, end; or edit, move, retire for an aspiration) already sit on their pages, and nothing would show when one was last reviewed.

**When a goal ends** (reached, deadline passed, or you end it):

6. **End goal** leads with what you achieved: where you started, the latest result, and milestones hit. Then you say which it is, **I reached it** or **End it for now**, since only you can tell whether the line was crossed. Ending moves no tasks.
7. **Reached:** the goal counts complete; milestones you never checked in were passed on the way, and read so. A quiet **Reopen** undoes it, in case you reached it by mistake. "What's next for Be fit?" This guards against the drop-off after a big finish. If the practice behind the goal has become part of your life, the app offers to keep it going as a recurring item under the aspiration. The item remembers the goal it came from.
8. **Not reached:** the page reads "Not yet." with the same lines. **Go again** makes it active, with its target and milestones as they were, ready to adjust or rethink. It's the same goal with its history, not a new attempt.

Branch:

- **An opt-in deadline passes:** "This one slipped." Two choices: **push it back** to a new date, or, when the date was firm and has passed, **end the goal for now**, exactly as End goal → End it for now does, with Go again still there. The card shows while the goal is active with its deadline behind it.

**Deleting (for mistakes):**

9. A task, milestone, goal, aspiration or recurring item made by mistake can be deleted. It disappears everywhere; deleting a goal takes its tasks, milestones and records with it. This isn't dropping: a dropped task was a decision and stays in history. No screen offers delete yet.

## Flow 8: Set intensity

Anytime, and only when you choose. The app never prompts about pace.

1. Pick **Going hard**, **Normal** or **Easing off**.
2. From the next draft, the plan adjusts: more or less goal work, more or less free time. Maintenance stays the same.
3. For the prototype, the setting stays until you change it. Whether it should be able to expire on its own is left open.

## Edge cases and open questions

Edge cases the prototype needs an answer for:

- **First run, nothing set up:** start with one sentence, either a goal or a maintenance item, and get a plan for tomorrow right away.
- **Away or sick for a while:** pause one goal or everything, so the gap doesn't count as missed and nothing piles up while you're gone.
- **A whole week with no tasks done:** rollover still happens quietly; the first open afterwards shows how far you've come, not what you missed.
- **One task serving two goals:** for the prototype, each task belongs to one goal.
- **Travelling between time zones:** a task's day and time carry no zone, so Tuesday at 6 p.m. is Tuesday at 6 p.m. wherever you are. Today is your phone's date. The week never goes backwards: fly west across Sunday midnight after the new week has started and you stay in the new week. Crossing the date line skips or repeats a date; a skipped day's tasks are simply unfinished.

Open planner rules (the data model works either way):

- **Unplanned work logged for a building goal:** does it use up one of the week's open sessions, or sit beside them? Leaning towards using one up, so "3 of 3 done" never leaves a session in the bank.
- **A one-off goal's step with no date:** when does it enter a week? Leaning towards waiting on the goal's page until you pull it in or give it a date, so six steps don't flood the bank at once.

Settled questions (kept so the reasoning isn't lost):

- Decided: no separate per-day capacity setting for the prototype. Intensity is the one dial; add per-day capacity only if days keep feeling overpacked. No time estimates per task.
- Decided: free time is protected by capping how many tasks go on a day, set by intensity (normal by default).
- Decided: no set finalize time. The plan updates continuously, so you can look ahead anytime and adjust; today's plan changes only because of something you did.
- Decided: anything you explicitly schedule on a day stays there, and the app plans around it.
- Decided: the week starts on Monday.
- Decided: intensity defaults to normal and stays as set until you change it; no automatic expiry for now.
- Decided: the app asks "Still matter?" once a task has been carried over for two weeks, however many times it moved. Saying no drops it, with no penalty; ignoring the question just lets it keep rolling over.
- Decided: no review prompts, and no Review on a goal either (Nov 10). Changing a goal is something you do when you want to, from its page.
- Decided: a not-yet milestone stays Next until it's hit; a check-in never passes over one. Moving its date later pushes the later dated milestones out by as much, capped at the deadline (Nov 10).
- Decided: a one-off goal's steps can be done in any order (Nov 10). They are tasks, not milestones (Nov 13, replacing the Nov 10 decision that they were milestones marked on the goal's page).
- Decided: a reached goal counts complete, with milestones never checked in shown as passed on the way, and can be reopened (Nov 11).
- Decided: a goal whose aspiration is retired can be resumed on its own; it shows under Other goals until the aspiration is brought back (Nov 10).
- Decided: a new fixed-day recurring item whose day is today is due today, not next week or next month (Nov 11).
- Decided: unfinished weekly-target tasks and flexible recurring items lapse; hand-added tasks and fixed-day items roll over (Nov 13, replacing "everything rolls over").
- Decided: a slipped deadline is pushed back or ends the goal; "recommit" and "rethink" are gone, and there's no "missed" status (Nov 13).
- Decided: a quitting goal's unanswered day is unknown, not a slip; the urge rating has three levels and is never a prompt (Nov 13).
- Decided: drop and delete are different; delete is for mistakes and is offered on everything (Nov 13).
- Decided: a recurring item is on or off; paused by a retired aspiration and turned off by hand are the same state (Nov 13).
- Decided: aspirations have no Review. Its choices repeated Edit, move up or down and Retire, which sit on the aspiration's page anyway.
