---
type: guide
status: current
updated: <YYYY-MM-DD>
---

# Working together

**How the user wants work done: the full rules.** The memory files are one-line pointers to this note. Look rules
live in the [[Style Bible]]; this note is about how we work. The rules below are a proven starting set; change any
of them to fit the user, and add a one-line *why* to each change.

## Decisions and check-ins
1. **Milestone reviews** with pictures or clips and a short list of choices. **Every new asset comes to the user
   before it goes live.** <Everything else Claude decides / the user wants to approve: …>.
2. **On feedback** (any model, UI or map): reproduce the user's exact view first (angle, crop, zoom); change only
   what was asked and list every change; restate a directional request ("make it more epic") in one sentence before
   building; handle the whole message in one round. *Why:* fixing one thing at a time while checking from other
   angles broke something else every round.
3. **Small reviewed steps with evidence** (a screenshot or a measurement) instead of big multi-system batches.
   *Why:* big batches end as a mess that has to be reverted.
4. **Fix it properly:** if an asset or texture doesn't work, regenerate it rather than tune around it.
5. **When the user approves a look, freeze it** and change only what they name. *Why:* every "small tweak" after an
   approved look made it worse.
6. **Look, don't audit:** check each area from the player's view and fix clear clashes, but keep cool elements and
   skip per-prop "why is this here" audits; time-box any single element.
7. **A fresh, harsh critic at milestones:** a new subagent with no history looks at the result cold, then against
   the references, and gives severity-ranked findings plus a "keep these" list.

## Studio and the live game
8. **Ask before saving any place** (the user answers each time). Never save, publish or touch the data of the live
   game <place id>; that is the user's.
9. **Reset the Edit camera** as the last Studio action (screenshots can leave it Scriptable). Before Play or
   screenshots, check that no other chat is using Studio; never stop someone else's Play test.
10. **Check sounds and private assets in Play**, not Edit (Edit uses the owner's login and hides failures).
11. **Collision:** <pets / small decor don't collide; big props players can reach do, per the map design>.

## Assets and tools
12. **Sourcing:** <what's pre-approved: image generator, Toolbox / Creator Store, web downloads, the user's own
    files>. Log where everything came from ([[Asset Library]]). Anything taken from another game's files: ask per
    asset (direct / modify / reference only).
13. **Uploading to Roblox:** <pre-approved / ask first>. Saving or publishing a place is never pre-approved.
14. **The image generator makes images** (concept art, sheets, views, textures, sprites); Claude does the design and
    the 3D. <Adjust to the user's tools.>
15. **≤ <N> triangles** per <asset kind>, every piece counted.
16. **Check before showing:** the `mesh-qa` gate in every pose, then play every animation and check motion by hand;
    a see-through, inner-shell, inverted-face or floater FAIL blocks integration. Every model matches its reference
    close to 1:1, proportions included, and moves across the entire model in every clip.
17. **Paid tools:** <Modal $… a month; Tripo … credits>. Before the first paid action in a task, give the job list
    with each cost, the total and the balance, and wait for a yes. A yes covers that list only.

## Usage
18. **Quality first, never ration:** no skipped reviews or checks to save usage. Stop and save state (nothing
    running, [[Project Home]] → Now updated) by **~97 %** of the usage limit.
19. **One chat per task:** the user starts each chat from a short brief ([[Task Brief]]) with their own references;
    bounded side jobs go to subagents inside the chat. Each chat commits only its own changes. Near the context limit,
    write a resume note (Project Home → Now or the asset's NOTE) so a fresh chat can continue. Keep contexts lean:
    filter long tool output, send broad searches to subagents, look at contact sheets rather than every render.

## Housekeeping
20. **Ask before deleting;** big files go to the Recycle Bin, never hard-deleted.
21. **Windows:** never rewrite source with PowerShell `Get-Content` / `Set-Content` (BOM + garbled characters); use
    the Edit tool or a Python script.
22. **Keep the vault current:** it is Claude's second brain ([[Home]]); the [[Dashboard]] is the user's view and
    **shows the work with pictures**: a "Right now" table, what waits on the user, a "New" list with a thumbnail per
    item. Update it at every delivery and with in-progress shots.
23. **Plain, short words** in notes and UI text.
24. <The user's own rules: how they talk, what annoys them, what they love.>

Related: [[Dashboard]], [[Project Home]], [[Decisions Log]], [[Blender and Roblox Studio]], [[Style Bible]]
