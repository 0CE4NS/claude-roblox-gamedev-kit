---
type: guide
status: current
updated: <YYYY-MM-DD>
---

# Style Bible

**How everything looks: creatures and props, UI, world, effects, and how image jobs are briefed.** This note wins
over any skill or pipeline note on looks. Fill each `<…>` with the user's answers; keep the general rules unless the
user changes them, and give each rule a one-line *why*.

## 0. The look in one paragraph
<e.g. "Chunky low-poly creatures with big deliberate flat faces, 2-4 flat tones per creature, flat eyes, bright
saturated colours that read in daylight from 30 studs away; UI with thick outlines and bold headers like
<reference game>.">

**Reference games for the look:** <names + which parts>. **Never use as a style target:** <anything the user
dislikes>.

## 1. Creatures and props
### 1.1 Rules on every model
- **Budget:** ≤ <N> triangles per <creature / pet / prop>, every rendered piece counted (eyes, glow strips, FX
  meshes). Cut surface detail and rig, never the silhouette. *Why:* <server size, mobile>.
- **Close to 1:1 with the reference, proportions included:** head, body, limbs, tail measured as fractions of
  height on the model and the reference; more than ~10 % off is a fix. *Why:* drift is what makes a model look
  "off" even when every part is fine.
- **Big deliberate flat faces**, not soft rounded blobs. "Low detail" ≠ soft shapes.
- **Flat colour areas with smooth borders,** 2-4 tones plus an accent; never per-face colour snapped from a
  generated texture (sawtooth borders, splotches).
- **Eyes are flat plates** (or a clean painted area) in a shallow socket, round or almond, mirrored. Domed eyes pop
  out; painted eyes on a low mesh go pixel-stepped.
- **Symmetric by construction:** mirror the body, mirror weights, mirror effect seeds, no one-sided roll.
- **No holes from any angle, below included:** Roblox culls back faces, so anything open is see-through.
- **Ready for variants (mutations, colour swaps):** one mesh per colour area per joint, named
  `<Asset>__<Joint>__<Area>`, so the game can recolour areas.
- <Your surface rules: studs only on body masses? smooth claws, horns, eyes? outlines? materials?>

### 1.2 References (every asset, before any image or model)
Work from a **large, varied set**: 8-12 images of the target style (the same kind of creature or object, same body
type, same tier look, a similar palette, variant versions too) plus 6-10 real-world photos for anatomy and motion,
mixed in source, angle (the game camera first) and state (idle, moving, variants). 3-5 references was too few.
Rank them by similarity and drop near-duplicates (`game-research` style study). List them in the asset's NOTE with
a reason each.

**Follow the user's own references feature by feature.** List each reference's features in the image brief; never
add features of your own.

### 1.3 Size ladder
| Tier / kind | Height in studs | Notes |
|---|---|---|
| <common> | <…> | |
| <rare> | <…> | |

### 1.4 Build route
Decided per asset and logged in its NOTE; the routes and their why are in [[Asset Build Pipeline]]. A new kind of
asset (a body type not built before) gets a **bake-off**: two routes side by side, the user picks.

## 2. UI kit
- **Built from measured real stills,** not from rules alone: take stills of the reference game's UI, measure
  colours, outline widths, corner radius, font weight and spacing, then build to the numbers and judge side by side
  with the stills. *Why:* a rules-only UI kit looked generic and the colours were wrong.
- <Fonts, header colours per panel, button styles, icon style, motion rules.>
- Plain, short words in UI text.

## 3. World kit
- Areas are **composed**: a hero landmark, paths that lead somewhere, props in groups with a reason, readable from
  the player's camera. Look over each area from the player's view and fix clear clashes, but keep the cool
  elements; don't audit every prop.
- Collision: <which props collide (big walkable rocks, cliffs) and which don't (pets, small decor)>.
- <Palette per area, lighting, terrain treatment, signage.>

## 4. Image jobs (concept art, blueprints, views, textures, sprites)
**Rich inputs, a plan before drawing, a self-review, a report back.**

| Step | Rule |
|---|---|
| Inputs | Every attached image labelled by its role in the prompt ("image 1 = <reference>: copy its <facet style>, not its <anatomy, colours>"), plus a full design brief (template below). Most tools take only ~5 images: pick the best. |
| Thinking | The image tool works in steps: (a) study the references and write a short plan **before** generating; (b) generate; (c) review its own result against the plan and the references; (d) regenerate if anything is off, up to 3 tries, keep the best. |
| Report | A `<name>_report.md` next to the images. |
| Use | Read the report **and** look at the image before using it. Count limbs per side and look for extra appendages before showing a concept. |

**Which images per route:**
- **Hand-built:** a blueprint. Sheet A: orthographic FRONT, BACK, LEFT, RIGHT, TOP, UNDERSIDE in equal slots, one
  scale, plain #D9D9D9, a faint grid and guide lines at ground / belly / shoulder / chin / eye / top-of-head heights.
  Sheet B: a big HERO (3/4 front, slightly above), close-ups of the head, feet, eyes and each signature part, and a
  colour strip naming each area with its hex.
- **Image-to-3D (TRELLIS, Hunyuan):** one clean, single, symmetric HERO image (plus the blueprint for clean-up).
- **Tripo multi-view:** orthographic FRONT, LEFT, BACK at one scale (RIGHT = LEFT mirrored), in a **rig-friendly
  pose**: T-pose, wings spread, limbs held out from the body, body straight. Draw the epic hero first, the input
  views after.
- **Straight, neutral pose for anything that becomes 3D:** body straight, fins / wings / tails spread; the curve
  comes from the rig and clips. A curvy reference makes a curvy model that's hard to rig.

Prompt template (copy, fill every `<...>`, delete nothing):
```
Generate images only. Do not write or change any code or project files other than the plan, the images and the
report named below.

TASK: <one line: e.g. an orthographic blueprint of ONE creature, "<Name>", that a 3D artist will rebuild as a
low-poly Roblox model>. The design is decided; follow it exactly, do not redesign.

ATTACHED IMAGES, by role:
1 = <reference>: copy its <facet style, horn shape>; not its <anatomy, colours>.
2 = <...>

STYLE: <the look in one paragraph from Style Bible §0: facet size, number of tones, which parts smooth, which
textured>.
DESIGN BRIEF:
- Silhouette: <one line>.
- Proportions: head ≈ <x> % of height; legs <short / medium / long>; <other>.
- Colours by area: Primary <#hex> (<parts>), Secondary <#hex> (<parts>), Accent <#hex> (<parts>), Detail <#hex>,
  Glow <#hex or none> (<where>), Eyes <#hex> pupil + white highlight.
- Pose: <straight, symmetric, limbs clear of the body; T-pose / wings spread for complex creatures>.
- Views: <the layout for this route, from the list above>; one scale; small labels only; plain #D9D9D9; no floor,
  shadow or particles.
- Counts to check in every view: <legs, wings, ears, toes, tail pieces, horns ...>.
WORK IN STEPS:
(a) Before drawing, write <folder>\<name>_plan.md: what you take from each attached image, the proportions, the
    facet layout, the colour per area, what each view must show.
(b) Generate the images: <folder>\<name>_<view>.png ...
(c) Review the result against the plan and every reference: silhouette, part counts per view, symmetry, colours,
    views consistent with each other.
(d) If anything is off, regenerate (up to 3 tries) and keep the best.
REPORT: write <folder>\<name>_report.md with the plan, the final image paths, what each view shows, the colour hexes
used, part counts per view, every deviation from this brief and why, the self-review against each reference, which
try you kept and why. Summarise the report in your final message.
```

Fix a concept with an edit pass: attach the draft, list only the fixes, "keep everything else". Repeat pose
instructions plainly ("NOT turned toward the camera") when the tool keeps turning a head.

## 5. Effects (VFX)
- Effects start on the body (named anatomy), move with it, read in daylight at game distance and never hide the
  face. Details and Roblox facts: [[VFX and Particles]]; the review loop: the `vfx-review` skill.
- <Your effect style: stylised or realistic, colour ramps, brightness limits.>

## 6. Replaced rules (kept so nobody revives them)
- <old rule> → replaced by <new rule> (<date>, why).

Related: [[Asset Build Pipeline]], [[Design]], [[Working Together]], [[VFX and Particles]], [[Codex]]
