# VFX capture + blind review protocol

## Why the old reviews passed bad effects
| Failure | Fix in this protocol |
|---|---|
| Stills of particles that weren't simulating (Roblox skips emitters outside the player's real camera; a screenshot tool's camera parameters only move the camera for its shot) | hold the client camera on the effect every frame, wait 2-4 s, then capture with no camera parameters |
| Short-lived effects missing (capture lands 1-2 s after the call) | slow emitters with `TimeScale` (and the rig), or a debug freeze |
| One still can't show "laggy", "choppy", "disconnected during flaps", "rapid" | time strips per angle + the final clip |
| The reviewer knew the intent and read it into the image | blind reviewer: goal board + MUST list + our frames only |
| Compared with the last round ("better than before") | compared with the goal board only |
| The builder argued the reviewer down | a disagreement means a second fresh reviewer; two FAILs = FAIL |
| Judged at a flattering angle, by night, close up | game camera + 2 others, daylight, game distance + one close-up |

## Capture
1. Studio may be shared: check other sessions before Play, captures or camera moves; never stop someone else's
   Play test. Use a test place; don't save.
2. Start Play. On the client, hold the camera (`BindToRenderStep`, a Scriptable CFrame looking at the host) for each
   angle: **game camera** (behind-above at the usual follow distance), **side**, **front** (and from below if the
   effect is under the body). Daylight (ClockTime ~14) unless the effect is for night.
3. For each animation state the effect lives in: wait 2-4 s after moving the camera, note the time, take 6-8
   captures in a row. Fast effects: `TimeScale` 0.15-0.3 on the model's emitters (and slow the rig) so the strip
   spans one effect lifetime; say so on the sheet. Check the first capture actually saved.
4. Collect the captures and build the review sheet (goal on top, rows below; crop to the creature, keep some sky).
5. Restore everything: TimeScale 1, camera released, stop Play, reset the Edit camera last.

## The reviewer (fresh subagent each round)
Give it ONLY: the review sheet, the goal board, the MUST / MUST NOT list, the game camera distance, and this prompt.
Not the spec's numbers, not what changed, not earlier verdicts.

```
You are reviewing a Roblox particle effect. Open the two images: GOAL (top of the sheet / goal board) and OURS
(rows of frames, left to right = time; each row is one camera angle and animation state).
1. Before reading the goal: describe OURS plainly - shapes, where they start on the body, which way they move
   across the frames, how long they live, colours, brightness against the sky, density, whether they stay attached
   to the moving body. Use frame labels (row, t#).
2. Name it cold: if a player saw OURS, what would they call it in five words?
3. Now read the GOAL and the MUST / MUST NOT list. For each item: PASS or FAIL with the frame that proves it.
4. Check this catalogue and report every one you see: sticker / pasted-on sprite; floats off or lags behind the
   body; flat card seen edge-on; same shape repeated in lockstep; blown out to white or washed out in daylight;
   invisible at game distance; a wall of particles hiding the creature; too sparse / tiny; low-frame-rate flipbook;
   too fast; wrong colour family; wrong direction; hard pop-in / pop-out; reads as a different effect (smoke for
   fire, dust for sparkles).
5. Verdict: PASS only if no MUST NOT is hit, every MUST passes, and your cold name matches the goal. Otherwise FAIL
   and list the top 3 fixes in order of visual impact, each tied to a frame.
Answer as: DESCRIPTION / COLD NAME / CHECKLIST / CATALOGUE HITS / VERDICT / TOP FIXES.
```

## Deciding
- PASS → make the user clip; FAIL → fix the first TOP FIX only, re-capture, new reviewer.
- If you believe the reviewer is wrong, don't overrule it: run one more fresh reviewer on the same sheet. Two FAILs
  stand. A PASS you can't explain from the frames isn't a PASS either.
- Max 3 internal rounds, then show the user with the honest verdict.

## The user clip
Real-time if possible (TimeScale 1); if slowed, label it. Show it with the review sheet and one line per MUST. For a
real-time MP4 of Studio on Windows: `ffmpeg -f gdigrab -framerate 30 -t 6 -i title="<Studio window title>" out.mp4`
(check the frames aren't black).
