---
type: guide
status: current
updated: <YYYY-MM-DD>
---

# Asset build pipeline

**How a creature, pet, character or prop goes from idea to Roblox: a large varied reference set, a route (a
bake-off for a new kind), build to the [[Style Bible]], rig with Claude reviewing every bone, pass the motion,
colour and `mesh-qa` checks, show the user, then integrate.** Step-by-step: the `build-asset` skill. This note keeps
the overview and the *why*.

## Routes
| Route | When | Main tools |
|---|---|---|
| **Hand-built** | simple, compact silhouettes (small animals, chunky props) | Blender, traced over an orthographic blueprint |
| **Tripo Studio** | anything from clean multi-view references, wings / horns / many parts included; clean low-poly topology at your budget straight from the generator, symmetric | [[Tripo Studio]] (`tripo-studio` skill) → own texture paint → own rig in Blender |
| **Image-to-3D + clean-up** | wings, long necks or tails, many parts, when you have GPU access | [[Modal and Open 3D Models]] (TRELLIS, Hunyuan3D) → **decimate first** to the budget → fix only what the checks flag |
| **Parts** | a body plus repeated parts: one wing / leg / fin made once and mirrored, reusable feet | any body + a parts kit saved in `tools/parts/` |
| **Body + designed parts and effects** | crowns, fins, glow strips, procedural motion on a real body | Blender or a canvas; **never whole bodies from code** |

**The Tripo route in one line** (worked on several creatures): stud-free orthographic views (front / left / back,
right = left mirrored) in a T-pose with wings spread → Smart Mesh multi-view, 4 variants with a polycount each under
the budget → `mesh-qa` all four, pick → Smart UV → export → flat-area paint from the views (own script) → own rig and
clips in Blender → `mesh-qa` in poses → mutation-ready area split. Tripo's own texture, rig presets and Edit Mesh
weren't good enough for stylised creatures.

**Bake-off** for a new kind of asset: two routes side by side to the same check level, shown with the same renders
and clips; the user picks, the winner becomes that kind's route.

## Checks before the user sees an asset
`mesh-qa` (defects strict, every pose) + **in-game angle shots** (the player's usual camera, a carry / close view, a
reveal close-up) + a **reference match** (overlay and proportions within ~10 %) + a **motion check** (every part
moves, light parts lag) + a **colour check** (brightness against the reference under game light) + an **appeal
check** (a fresh critic against the refs). At most **three fix rounds** on the same failure, then the user gets
options: switch route, accept named flaws, or park.

## Gates
- **Every asset comes to the user before it goes live:** renders and clips next to the refs, the checks above,
  critics first; the user's OK, then integration.
- **Every delivery** (to `assets/inbox/<Asset>/`): a `NOTE.md` ([[Asset NOTE]]) with the references, the route and
  tools step by step, the `mesh-qa` report, the triangle count and renders next to the refs. This is the record of
  how it was made: the next asset of the same kind starts from it.

## Decisions and why (general, from the original project)
- **Simple creatures hand-built:** they came out cleaner and closer to the reference than generated ones.
- **Code never builds whole bodies:** procedural / canvas-built bodies were rejected; designed parts, motion and
  glow on a real body were liked.
- **Decimate generated meshes, don't rebuild them:** the raw mesh decimated to budget looked the same; rebuilds
  changed it completely.
- **Bones are Claude's job:** auto-rig output is a starting point. Review every skeleton and edit bones and weights.
- **Real motion, measured:** minimum movement per part per clip.
- **Bigger, varied reference sets:** 8-12 style refs + 6-10 real photos per asset; 3-5 was too few.
- **Blender runs in the background;** the user sees renders and clips at reviews.
- **Symmetric by construction:** mirror the body, aim the rest pose, mirror weights and effect seeds.

Related: [[Style Bible]], [[Rigging and Animation]], [[VFX and Particles]], [[Codex]], [[Tripo Studio]],
[[Modal and Open 3D Models]], [[Blender and Roblox Studio]], [[Lessons Learned]]
