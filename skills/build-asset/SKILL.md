---
name: build-asset
description: Build a creature, pet, character, egg or prop model for this Roblox game to the Style Bible (the project's look, triangle budget, mutation-ready colour areas) by one of the routes - hand-built in Blender, Tripo Studio, image-to-3D (TRELLIS / Hunyuan) + decimate + clean-up, the parts route, or a real body with designed parts and effects - with a bake-off for each new kind, then rig (Claude reviews and edits every skeleton), animate, run the reference-match, motion, colour and mesh-qa checks, deliver with a NOTE.md and show the user before it goes live. Use whenever the user asks to make, model, rebuild, rig or animate a pet, creature, egg or prop, turn concept art or views into a 3D model, use Tripo / TRELLIS / image-to-3D or an auto-rigger, or run a bake-off.
---

# build-asset

Idea → a model that looks like it belongs in the game, inside the triangle budget, animated, checked, delivered,
then shown to the user before it goes live. The look is **not** decided here: `vault/Style Bible.md` §1 (models),
§4 (image jobs) is the rulebook and wins over anything in this skill. Overview and the why:
`vault/Pipelines/Asset Build Pipeline.md`.

## 0. Before starting (every route)
1. Read `vault/Style Bible.md` §1 and §4, `vault/Project Home.md` → Now, and search `vault/Log/Lessons Learned.md`
   for the creature type and the route. If an earlier asset of the same kind exists, read its `NOTE.md` first and
   reuse its scripts.
2. Before any paid step (Tripo credits, GPU jobs), give the user the job list with each cost, the total and the
   balance, and wait for a yes.
3. Blender runs in the background: `"<blender.exe>" -b --factory-startup -P <script> -- <args>`, absolute paths.
4. Work in `assets/inbox/<Asset>/` (`refs/`, `design/`, `glb/`, `work/`, `kit/`, `qa/`); never in the repo root.

## 1. References
Skim widely, then work from a **larger, varied set** (3-5 is too few):
- **8-12 images in the target style:** the same animal or object, the same body type, the same tier look, a similar
  palette, variant versions too. From the reference-game studies (`references/`), the wiki, video stills, Google
  Images. Rank by similarity and drop near-duplicates (the `game-research` style study).
- **6-10 real-world photos / footage stills** for anatomy and motion: the anatomy card in `mesh-qa` →
  `references/anatomy_refs.md`.
- **The user's own references come first:** list each one's features and follow them feature by feature; never add
  your own.
- Mix angles (front, side, 3/4, the game camera at game distance) and states (idle, moving, variants). List them with
  a reason each in the asset's `NOTE.md`.

## 2. Design images (Style Bible §4)
Use the §4 prompt template exactly: labelled reference images (max ~5), full design brief, plan → generate →
self-review → report. Which images:
- **Hand-built:** blueprint sheets A (orthographic views with guide lines) + B (hero and details, colour strip).
- **Tripo:** an epic hero first; then orthographic FRONT / LEFT / BACK at one scale, stud-free, T-pose, wings
  spread, limbs held out (normalise the views to one scale on a flat background; RIGHT = LEFT mirrored).
- **Image-to-3D:** one clean, single, symmetric HERO (3/4 front, straight pose) + the blueprint for clean-up.
- **Keyframe sheet** (optional, for motion): the active loop in 4 keys + idle + special, drawn from real motion strips.
Read the report and look at the image before using it. Count limbs per side; look for extra appendages. Show the user
the hero before building if the design is new.

**No image generator?** Ask the user for views, use Tripo's Image tool, or trace the hand-built blueprint from
photos and your own sketch renders.

## 3. Build: pick the route
**Simple, compact creatures are hand-built** (unless the user picked otherwise). **A new kind** (a body type not
built before) gets a **bake-off:** two routes to the same check level, shown with the same renders and clips; the
user picks, the winner becomes that kind's route (record it in the Asset Build Pipeline note).

| Route | Steps |
|---|---|
| **Hand-built** | Trace the blueprint in Blender with low-segment lofts and primitives (12-sided head and body, 8-sided legs, a ring at every joint, blade shards for fur and feathers, plates for eyes and inner ears). One mesh per colour area per joint; cut colour areas along edge loops. Never sphere-merge + remesh + decimate. A trace-overlay check (render the model over the blueprint views) after each step. |
| **Tripo Studio** (`tripo-studio` skill) | Smart Mesh P2.0 multi-view, 4 variants at per-model polycounts under the budget (Tripo overshoots ~5 %) → export all → `mesh-qa` each, pick the cleanest → Smart UV → export (check the GLB has `TEXCOORD_0`) → weld UV seams, scale, face +Y → decimate only where triangles are wasted (feet, toes) → snap the midline and mirror the clean half → paint flat areas from the views (below) → rig. |
| **Image-to-3D + clean-up** (Modal, `vault/Tools/Modal and Open 3D Models.md`) | Generate from the hero → weld → drop inner shells → **decimate the whole closed mesh first** (start at ratio ~0.15) to the budget → compare raw vs low from the same cameras → symmetrize (`mesh-qa` symmetry tool) → flat colour areas → fix only what the checks flag. Never rebuild the whole body. Remesh (voxel / blocks) only as a variant on a closed chunky body. |
| **Parts** | A body from any route; **one wing / leg / fin made once and mirrored**; reusable feet and claws from a parts kit (save each good part in `tools/parts/` with its pivot and size); join with a designed shoulder / hip, not a blend band. |
| **Body + designed parts and effects** | A real body (any route above) + designed parts (crowns, fins, horns, glow strips) and procedural motion. **Never a whole body from code.** |

**Flat-area paint from views** (Tripo and image-to-3D; write `tools/paint_areas.py` the first time): project each
reference view onto the mesh, vote each surface point's colour area from the views that see it best, smooth the
votes as fields on a fine grid of surface points (not per vertex), mirror the fields, remove small islands
(respecting the mirror side), paint eyes analytically (round / almond, iris and highlight, mirrored), anti-alias
borders 2x, write a clean texture. Config per asset: palette with area names, eye centres and sizes, any region
rules (e.g. "horn colour above 90 % height").

**Mutation-ready split** (write `tools/area_split.py`): cut the mesh along the texture's area borders into one
untextured piece per area, snapping each cut per **edge** (never per face; whole-face snaps make T-junction
pinholes), lay designed plates for eyes and thin features (smile, nostrils) that are thinner than the triangles, name
pieces `<Asset>__<Joint>__<Area>`. The cuts add ~2 triangles per border face: leave room in the budget.

Whatever the route: the Style Bible's surface rules, symmetric, inside the triangle budget with every piece counted.

## 4. Rig and animate (`vault/Pipelines/Rigging and Animation.md`)
- **Study a reference rig of the same body type first** and match its chain density and motion layering.
- **Bones are Claude's job:** auto-riggers (Tripo Rig, TokenRig) are a starting point. Review every skeleton: pivots
  at the real joints (check against the anatomy card), names in the project's scheme, no missing or extra bones.
  A hand-placed symmetric skeleton (a script that places bones from measured landmarks) is often the better start.
- Weights: a function of position only (never per colour area), cut planes with even bands, loose shells take one
  weight set, wing / frill membranes in the DoubleSided part by weight share. Mirror weights.
- Clips: hand-keyed in Blender from the anatomy card's motion strips (or a keyframe sheet). Scene fps 30 before
  baking; reset the pose before saving; root bone as the pivot under the body.
- Motion rules: an idle and a real gait, the whole model moving in every clip, feet planted, mirrored limbs, light
  parts lag, smooth transitions. Enough bones for that: a chain in every tail, fin, ear and wing.

## 5. Checks (before anyone sees it)
1. **Mesh:** the `mesh-qa` skill in rest pose **and** posed frames of every clip, plus its in-game angle shots and
   the appeal check; at most 3 fix rounds, then the user gets options.
2. **Reference match:** render the model from the reference's own views (same framing) and overlay it; measure
   head, body, neck, limb, tail, fin and horn lengths as fractions of height on both; flag anything more than ~10 %
   off. Put the overlay sheet in the delivery.
3. **Motion check:** measure the max rotation / offset per bone over each clip, grouped by part (spine, neck, head,
   each limb, tail, fins, ears, wings). Every part moves in every clip (≥ ~5°), light parts lag. Starting minimums:
   idle head turn ≥ 10°, body bob ≥ 1.5 % of height, a weight shift; walk body rise and fall ≥ 3 % of height; fliers
   wing arc ≥ 60°. A clip where every bone stays under ~2° fails.
4. **Colour check:** render under game light and compare each area's brightness and saturation with the matching
   area on the references; flag anything more than ~15 % darker or duller.
5. **Play each animation by hand:** parts move with the right body part, no clipping, no joint gaps.

## 6. Deliver and show
In `assets/inbox/<Asset>/`: the `.blend`, the `.glb` (Roblox axes after export, feet on Y = 0), the area kit, the
`mesh-qa` report, the overlay sheet with the proportion numbers, the motion and colour numbers, turntable renders and
in-game angle shots beside the refs, clips, and a `NOTE.md` (`vault/Templates/Asset NOTE.md`: references, route and
tools step by step, numbers, checks, open issues).

**Every asset comes to the user before it goes live:** renders, clips and a short list of choices. Only after the
user's OK: upload, wire into the game, test in Play, ask before saving. Then update the Dashboard (with a picture),
Project Home → Now and the Decisions Log, and log new mistakes as one-line rules in Lessons Learned.
