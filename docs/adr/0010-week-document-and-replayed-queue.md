# ADR-010: The phone keeps the week as one document and replays unsent changes over it

**Date:** 2026-11-16 · **Status:** Accepted

## Context

ADR-004 requires Today to open from data on the phone, check-offs to show at once, and changes made offline to be sent later. The API's shared decisions make each write safe to repeat (device-made ids, creates and actions that change nothing the second time, last write wins). Nothing yet said how a queue of writes behaves as a sequence, what the phone stores, or what happens to a change that arrives after the week it was made in has closed (ADR-009).

The usual hard part of an offline design is patching stored data when a change is made, then un-patching it when the save fails.

## Decision

**What the phone stores**

- **One document per open week:** the API's week read, exactly as the server sent it. Today, any day and the bank are drawn from it. The week is the only plan read; the API has no separate reads for a day or the bank.
- **A queue of unsent changes,** in order, each a complete request.
- **The last response of every other read,** so any screen opens instantly and can be read offline.

**What the screen shows**

- The stored document with the unsent changes replayed on top. The document itself is never edited on the phone.
- A change that is confirmed keeps being replayed until a read that started after its confirmation has arrived, so the screen never flickers back.
- A change that is dropped simply stops being replayed.

**What can be queued**

- Check off, uncheck, place on a day, send to the bank, swap, add a task, and a quitting goal's daily answer and urge rating.
- Everything else needs a connection: its result depends on the server's planning (a new goal, a target, pause, delete, wrap-up, intensity). Offline it fails with the ordinary "Couldn't save that" message. Online it waits for the queue ahead of it.

**How changes are sent**

- One at a time, in order; the next only after the last is answered.
- When the app opens or returns to the front, when the network comes back, and after each new change. There is no background sync on iOS.
- Success: the change leaves the queue. Not found (404) or no longer allowed (409): it is dropped silently. A network failure, a timeout or a server error: it stays, sending stops, and it is tried again later.
- When the queue empties, the week document is fetched again.
- While changes are waiting after a failed send, Today says so in one quiet line.

**What the API guarantees**

- A queueable write states its end result, never a toggle or a relative move, so sending it twice gives the same result.
- A plan edit (place, send to bank, swap) names the week it was made in, and is refused if the task has moved on to another week.
- A create is never refused over where it goes. A task added to a day that is no longer allowed is saved in the current week's bank.
- A check-off or an uncheck that arrives after its week has closed still counts, and the task gets the ending that week would have given it.
- A queueable write's request body only changes by addition, so a change queued by an older version of the app still sends.

## Consequences

- No version numbers, no change feed and no conflict responses. Two devices changing one task: the last to arrive wins, whichever was tapped first.
- Each queueable change needs one replay rule, over one document shape. They live in the front end's `domain/`, are plain functions, and do nothing when their task is no longer in the document.
- A day the phone has never fetched still opens, because the whole week came with the first fetch. Opened offline on a Monday, Today shows next week's plan if the app was opened after it was drafted on Sunday evening, and is empty until online otherwise.
- Counts on screens outside the plan (a goal's page) don't include unsent changes until they are sent.
- A change the server refuses disappears from the screen without a message. That is right when the person made it moot elsewhere, and it is why creates are never refused: nothing typed is lost.
- A server error on one change holds up the ones behind it. Nothing is lost, and the quiet line says changes are waiting.
- Sign-in must leave the queue in place when a session ends, and send it after signing in again.
- To check when this is built: how readily iOS clears an installed app's stored data, and asking the browser for persistent storage.
