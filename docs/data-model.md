# Data model

Seven entities cover the prototype. An aspiration has many goals; a goal has many milestones and one weekly target; tasks belong to a goal, to a recurring item, or to nothing. This is the conceptual model. Tables, indexes and migrations are designed in the API repo (Postgres, ADR-003); refine this doc when they are (tracker task t402).

| Entity | Key fields | Dates | Belongs to |
| --- | --- | --- | --- |
| Aspiration | Name ("Be fit"), why, priority order | Created, last reviewed | — |
| Goal | Label ("Be strong"), measurable line, type (building, quitting, one-off), baseline, status (active, paused, reached, ended) | Start, optional deadline, last reviewed, ended | Optional aspiration |
| Milestone | Measurable line (bench 155…), result when checked, status (upcoming, hit, not yet) | Optional target date, checked-in date | Goal |
| Weekly target | Count or amount (3 sessions, 25 miles), unit | Effective from (so changes keep history) | Goal |
| Task | Title, optional value logged (miles, weight), status (in bank, planned, done, skipped), rollover count | Week it belongs to, planned day, optional due date, optional time of day (due by 7 a.m., or at 6 p.m.), completed date | Optional goal or recurring item |
| Recurring item (maintenance or an ongoing practice) | Name, repeat rule (every Tuesday, monthly on the 1st, once a week), fixed day or flexible within the window, active | Next due | Optional aspiration |
| Settings | Intensity (going hard, normal, easing off; it also sets how much free time is protected), week start day | Intensity set on | — |

Notes:

- **Every goal has a finish line it could reach** (its measurable line), but no forced end date. It stays active as long as it's needed, until it's reached or you end it. Ongoing practices that should never end are recurring items, not goals.
- **Tasks carry two kinds of date:** the day they're planned for, which the app moves freely, and an optional due date, which is a real constraint. A task can also have an optional time of day; tasks without one float, and the app plans them around the timed ones.
- **Quitting goals** need two more pieces: a daily yes/no log from the start date, rather than tasks, and an occasional urge rating that shows when the habit is broken.
- **The day plan isn't stored separately** for the prototype: it's the tasks whose planned day is that date.
