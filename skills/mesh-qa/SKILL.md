---
name: mesh-qa
description: Measured quality gate for every 3D model in this Roblox game (see-through holes, floaters, inner shells, double walls, inverted faces, colour splotches, jagged borders, pose stretch, asymmetry, the triangle budget), with a symmetry-fix tool, in-game angle shots, a fresh appeal critic and a three-round limit. Run it before any mesh is shown, uploaded or delivered - especially generated (Tripo, TRELLIS, Hunyuan) output - and whenever the user mentions holes, gaps, clipping, splotches, lopsided, floating bits or triangle count. The build-asset skill calls it.
---

# mesh-qa

Every model problem a user catches by eye (holes "somewhere", jagged colour borders, splotches, lopsided bodies,
glitchy stretch, floating bits, too many triangles) is measurable. This skill measures them before anyone looks,
shows the evidence as zoomed crops, and says PASS / WARN / FAIL. Hand-picked renders let big holes survive version
after version, and hid fin undersides. Don't eyeball: run the gate.

## Standing rules
- **Triangle budget:** ≤ <N> per <asset kind> (Style Bible §1). Everything rendered counts (glow strips, eye plates,
  FX meshes). Cut surface detail and rig, never the silhouette.
- **Symmetric:** generated meshes are always a little lopsided. Fix it right after generation, before anything is
  built on it, and mirror weights, effects and motion later.
- **Clean areas:** colour areas are clean shapes with smooth borders, never paint snapped per face.
- **No visible holes in any pose from any side, below included.** Roblox culls back faces: anything open is
  see-through in game unless the piece is DoubleSided.

## Tools (write them the first time; keep them in `.claude/skills/mesh-qa/scripts/`)
These run in background Blender (Python + `bmesh`). Build `mesh_topology.py` and `mesh_views.py` first; they catch
most problems. Thresholds: `references/checks.md`.

| Script | What it must do |
|---|---|
| `mesh_topology.py <model> <out> [--open pieces] [--poses Clip@frame,…]` | On the **welded** mesh (weld ~1e-5 × size): open loops (holes, cracks, T-junction pinholes), floaters, inner shells, double walls, inverted faces, non-manifold edges, slivers, colour splotches and stray facets, symmetry error, UV smear, per-pose stretch, the triangle total and a per-piece breakdown. Writes JSON + a summary. ~5 s. |
| `mesh_views.py <model> <out> [--views 74\|26] [--pose Clip@frame]` | Renders flat unlit colours from ~74 fixed cameras around and below the model, with every visible **back face painted magenta** = exactly what is see-through in Roblox. |
| `mesh_report.py <views dir> [--original <views dir>]` | From the renders: see-through regions, 2D floaters, holes vs an original, jagged colour borders, specks, silhouette symmetry; cross-checks which 3D loops are actually visible; writes `_summary.md` and sheets with zoomed crops (left clean, right marked). |
| `mesh_symmetrize.py <raw.glb> <out> --preview \| --apply left\|right [--shift-box …]` | Finds the best mirror plane (yaw search ±50°), reports the mirror error and off-centre midline features, renders keep-left / keep-right previews; `--shift-box` slides a feature onto the plane; `--apply` cuts, mirrors and welds. |
| `mesh_qa` wrapper (`.ps1` or `.py`) | Runs the whole gate in one command for rest + each pose and writes one summary. |

Put QA output in the asset's `work/qa/<version>/`.

## Workflow

### 1. Before modeling: anatomy and motion references
Build an anatomy card from real sources: side / top / front photos, a skeleton diagram, slow-motion strips.
Proportions as ratios, limb counts, how each part moves. Feed the best 2-4 refs to the image generator, then check
its panels against the card. Template and sources: `references/anatomy_refs.md`.

### 2. Right after generation: symmetry
Run `mesh_symmetrize.py --preview`. A p95 mirror error above ~0.5 % = asymmetric (it nearly always is). Look at the
previews and the off-centre midline features (tail, dorsal fin, crest, horn): mirroring one side can double them or
cut them off. Slide them onto the plane, or keep them out of the mirror and rebuild them. Then `--apply` (keep the
cleaner side) and re-run topology: the seam must add no open loops.

### 3. While building: quick topology after every geometry or colour step
Seconds per run. Run it after decimation, cuts, recolouring, part attachment. A check that goes from PASS to FAIL
points at the step that broke it.

### 4. Before showing or uploading: the full gate
Run the gate with every clip's extreme poses and the original (the generated source or the last approved build):
1. Read the summary.
2. Open every sheet whose check isn't PASS and look at the zoomed crops. Magenta = see-through, yellow = sawtooth
   border, green = speck, orange = enclosed blob, cyan = floater, red = hole vs original.
3. Classify each finding: **real defect** → fix it (`references/fixes.md`); **by design** → say why and pass the
   flag (`--open` for single-sided overlays laid on the skin, a DoubleSided list, a float-OK list for parts that
   hover on purpose). Don't flag your way to a PASS: anything visible from a normal game camera is a defect.
4. Re-run until it passes. If something stays WARN / FAIL, say so plainly when showing the model, with the crop.

**Defects stay strict:** a see-through, inner-shell, inverted-face or floater FAIL blocks integration; only named
cosmetic classes (tiny-tip jag, splotches) may stay, listed in the NOTE.

### 4b. In-game angle shots
The QA views are close and orthographic; players see the asset small, from above, under game light. Render three
shots at its **in-game height** (Style Bible size ladder; a 5-stud avatar block for scale) under game light:
| Shot | Camera |
|---|---|
| **Usual view** | the player's usual camera on the asset, e.g. ~28 studs away, ~14 up (≈ 27° down) |
| **Carry / follow view** | over the shoulder, ~10 studs behind the avatar, ~6 up |
| **Reveal close-up** | the hatch / unlock framing, ~8 studs, slightly above eye level |
Check the silhouette, eyes and colour areas still read at that size. Before the user sees it, add real Studio shots
(a preview copy in the working place, not live).

### 4c. Three-round limit
If the same check still fails after **three** fix rounds, stop fixing and give the user options with the crops:
**switch route**, **accept named flaws** (each listed with its crop), or **park** the asset.

### 4d. Appeal check
After the gate passes, a **fresh critic** (a new subagent with no history; prompt in `references/checks.md` →
Appeal) looks at the turntable and the in-game shots, gives first impressions, then sees the reference images and
says what reads cheaper. Severe findings are fixed before the user sees it.

### 5. Record
Put the verdict table in the asset's `NOTE.md` (version, triangles, checks). New failure modes and fixes go in
`vault/Log/Lessons Learned.md`.

## Things that fooled earlier checks
- Back faces drawn in a viewer hid holes that Roblox shows (culled) → the magenta render exists for that.
- A 3D open loop isn't necessarily visible (buried tube ends) → only loops that show magenta fail.
- Glow strips and plates laid on the skin look like double walls and holes → pass them with `--open`.
- Parts that only touch by intersection read as floaters → count triangle overlap as touching; a real gap still
  fails.
- Whole-face colouring of any kind makes sawtooth borders; check borders on the flat render, never a shaded one.
- Thin necks and tail stocks vanish at some angles and make the next part look detached → 2D floaters catch it.
- A one-sided fold only shows when a pose turns it to the camera → always run the posed frames.
