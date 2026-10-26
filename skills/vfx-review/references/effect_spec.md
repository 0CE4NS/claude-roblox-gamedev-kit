# Effect spec (write before building; it is the review's yardstick)

```
EFFECT: <name> on <creature / object>, state(s): idle / active / special
GOAL (one sentence the user would agree with):
  e.g. "Warm fire rising off the inner wings and crest like heat the creature gives off - stylised, not photo-real,
  curving outward-and-back from each wing, attached through every flap."
USER WORDS / REFS: quotes and image paths from this ask and earlier rejections (vault VFX note)
ANATOMY: where it lives on the body (named parts) and where it must NOT be
REAL BEHAVIOUR (from strips): birth -> growth -> motion -> colour change -> death, lifetime, how it reacts when the
  host moves fast (streams back? stays put?)
STYLE (from stylised refs): shape language, number of tones, edge softness, frame rate feel
LAYERS (one row each; add / remove rows as needed)
  | role        | host / anchor | texture (source) | orientation | life | rate / count | size over life | colour over life | motion | lock / proxy |
  | silhouette  |               |                  |             |      |              |                |                  |        |              |
  | body        |               |                  |             |      |              |                |                  |        |              |
  | flow        |               |                  |             |      |              |                |                  |        |              |
  | accents     |               |                  |             |      |              |                |                  |        |              |
  | aftermath   |               |                  |             |      |              |                |                  |        |              |
DENSITY at game distance: ~N particles visible, covers ~X% of the creature, never hides the face
BRIGHTNESS: readable by day without blowing out (LightEmission / Brightness ranges)
BUDGET: emitters <= N, particles alive <= N, lights <= N, no frame drop with several on screen
MUST:
  - ...
MUST NOT:
  - ... (copy every past rejection for this effect: "stickers", "floats off during flaps", "too orange", ...)
CHECK VIEWS: game camera + side + front (+ below), states: ...
```

## Example (heat flames on a winged creature, written from rejected rounds)
- GOAL: stylised warm fire rising off the inner wings and crest as the creature's own heat; licks curve outward and
  slightly back on each wing, back on the crest; stays attached through the flap.
- MUST: licks born on the wing's upper edge (inner half) and crest; lean outward + back from any camera; ride the
  wing through the downstroke (no trailing gap); gold → orange → red over life, mostly gold; merge into tongues (not
  single specks); readable by day.
- MUST NOT: look pasted on / static ("stickers"); float off or lag during flaps; flat cards edge-on; one orange
  colour; fast flicker; full-white blow-out; cover the head.
- Engine facts that decide the build: locked particles on a Bone lock to the anchored root part (use proxy parts
  moved onto the bone each frame); camera-facing sprites can only lean in screen space.
