---
name: vfx-review
description: Make a particle effect, aura, flame, lightning, trail, beam, glow or flipbook for this Roblox game that passes a blind side-by-side review against a goal board before the user sees it - context, frame research, a written spec and goal board, layer-by-layer build, Play-mode capture strips, a fresh blind reviewer, at most three rounds. Use whenever an effect is asked for, or the user says one looks wrong, cheap, laggy, choppy, disconnected, flat, "like stickers", too bright or unlike the reference.
---

# vfx-review (effects review loop)

Why it exists: an effect once went through eight rejected rounds ("stickers", "laggy and choppy", "looks flat",
then "just get rid of it") while self-reviews kept approving. The reviewer saw stills where particles weren't
simulating, knew the intent, and compared with the last round instead of the goal. Each step below fixes one of
those. Roblox facts: `references/roblox_vfx_facts.md`; what was tried before: `vault/Pipelines/VFX and Particles.md`;
look rules: `vault/Style Bible.md` §5; where to find textures and packs: `vault/Pipelines/Asset Library.md`.

1. **Context.** Look at the subject from the game camera; name the anatomy the effect comes from (wing edges, crest,
   cracks, mouth). Read the VFX note (what was tried and rejected). Restate the goal in one sentence the user would
   agree with.
2. **Research as frames, not memory:** the real phenomenon (slow motion), stylised targets in the project's art
   direction, and Roblox work from the last 12 months (DevForum, YouTube, Creator Store packs). Frame strips with
   the `game-research` skill's `strip.py`. Search phrasings: DevForum `site:devforum.roblox.com <effect> vfx`; YouTube
   `roblox <effect> vfx showcase`, `stylized <effect> vfx breakdown`, `<real phenomenon> slow motion`.
3. **Spec + goal board before building:** `references/effect_spec.md` (goal, layers with one job each, anchors,
   motion and timing, colour over life, density at game distance, budget, MUST / MUST NOT). The goal board is one
   image: the reference frames with captions. Show the user first when the ask is ambiguous or a round was rejected.
4. **Build** one layer at a time (silhouette → body → flow → accents → aftermath). A motion fix that changes a liked
   look isn't a fix. Textures from the image generator (greyscale on black, the game tints them).
5. **Capture, then a blind review** (`references/vfx_review.md`): Play mode, the game angle and two others, every
   animation state, time strips (slow `TimeScale` for fast effects), daylight. A **fresh reviewer subagent** gets
   only the goal board, the MUST / MUST NOT list and our sheet (never our intent or numbers), describes what it
   sees first, names the effect cold, checks the failure catalogue, returns PASS / FAIL with frame evidence.
6. **Iterate** up to 3 rounds (fix the top finding, new fresh reviewer each time). PASS → show the user a short
   side-by-side clip + the sheet. Still failing → show it with the verdict and the open problems; never call a
   failing effect done.
7. **Record:** a library NOTE per outside asset used; the VFX note gets what worked and what was rejected (the user's
   words); new failure modes go to Lessons Learned.

## Tools (write them the first time in `.claude/skills/vfx-review/scripts/`)
| Script | What it must do |
|---|---|
| `collect_captures.py <dir> --since "<time>" --prefix <state-angle>` | Copy Studio's screenshots taken after a time into the work folder with clear names. |
| `board.py goal <out.jpg> --img <file> "<caption>" …` | A captioned grid of reference frames (also good for any captioned board, e.g. shop screens). |
| `board.py review <sheet.jpg> --goal goal.jpg --row "<label>" <files> … [--crop x0,y0,x1,y1]` | The goal on top, our frames below in rows (one row per camera + state, left to right = time). |
| `board.py clip <out.gif> --ours <frames> --ref <frames> --fps 8` | A side-by-side clip (also an .mp4) for the user. |
