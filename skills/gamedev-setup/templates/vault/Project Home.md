---
type: index
status: current
updated: <YYYY-MM-DD>
---

# Project home

**What state each part of the game is in and what happens next.** The user's view of the same is the
[[Dashboard]]; history is the [[Decisions Log]].

## Now
**The agents' status note: what's true now, what's next, what waits on the user. Keep it to about one screen.
When an item is done, move a line to [[Decisions Log]] and delete it here.**

- **How work runs:** one chat per task, started by the user with a short brief ([[Task Brief]]). Bounded side
  jobs go to subagents inside the chat. Each chat commits only its own changes and asks before saving the place.
  Before Play or screenshots, check no other chat is using Studio.
- **Studio:** working copy `<build/Game.rbxl>`, **last saved <date, what>**. Save only after the user says yes.
- **In the game (the saved place):** <what's built and working>.
- **Not committed:** <files or features still uncommitted, if any>.
- **Running:** <each chat that's working: what, where its notes are>.
- **Open bugs:** <list>.
- **Waiting on the user:** <decisions only they can make>.
- **Next:** <the next task, or "the user picks" with candidates>.
- **Budget:** <paid tools: spent / cap, credits left; nothing running>.

**Working notes:** <how to push scripts into Studio, how to run tests, other handy commands>.

## Parts of the game
| Part | Owner note | State |
|---|---|---|
| <Lobby / Island 1 / Pets / Shop / UI…> | <[[Note]]> | <idea / building / in the game / parked> |

Related: [[Dashboard]], [[Design]], [[Decisions Log]], [[Working Together]]
