# Fix recipes per finding

Fix the cause, not the symptom: if a fix changes how a liked part LOOKS, it isn't a fix. Re-run the gate after every
fix; a fix for one finding often creates another (a filler that hides a hole from above floats from below).

## Holes, cracks, see-through
- **Pinholes (3-4 edges) along colour borders** = T-junctions: a cut put a vertex on one side's edge but not the
  other's. Weld the border verts (`bmesh.ops.remove_doubles` at ~1e-4·size), or split the neighbour edge at the same
  point so both sides share it.
- **Openings on a closed body** (cut caps, wing roots, mirror seams): cap with `bmesh.ops.holes_fill` /
  `triangle_fill` + `beautify_fill`, or zip a designed part's ring onto the boundary by angle. Never `holes_fill` a
  1M-face generated mesh (it hangs); work on the low mesh.
- **Open primitives** (gems, cones, tubes) seen from below: close their bottoms or sink them into the host;
  DoubleSided only for true sheets (glow strips, membranes), and list them for the Roblox import.
- **Real holes vs design gaps:** only fill where the posed original covers and the build doesn't.
- **Holes only in poses** (skin opening at a joint): weights, not geometry; see stretch below.

## Floaters, inner shells, double walls, inverted faces
- **Floating part** that should touch: sink it 0.2-0.5 % of size into the host along the host normal. If it hovers
  by design (an eye plate above the skin), pass it as float-OK and say why.
- **Inner shells / double walls:** drop inside shells by ray parity FIRST, then fill small holes, then fix normals
  (parity lies while nested shells exist). Refuse mass normal flips.
- **Best cure for generated double walls:** build a closed surface before decimating (an offset or uniform
  resampling), then decimate half + mirror.
- **Inverted faces:** `normals_make_consistent` on closed islands only; open sheets need the side decided by which
  side the camera sees.

## Colour: splotches, sawtooth borders, gradients, smeared textures
- **Never colour whole faces** from a generated texture, normal thresholds or dominant bones on a low mesh: sawtooth
  borders and splotches every time. Define each area as a smooth field (height on the body, angle around the spine,
  distance along a limb) and CUT every straddling triangle along the field's border, so borders are smooth curves.
  Check on the flat render.
- **A splotch already there:** recolour it to its host colour (it's ≥ 85 % surrounded by it), unless it's a designed
  marking, which then deserves a designed shape cut in along its outline.
- **Gradients:** a deliberate ramp along a field (e.g. claw tip → base in four flat steps), as cut bands or vertex
  colours. Not noise, not baked shading. Mirrored on both sides.
- **Textured pieces:** small important areas (face, eyes) get their own piece or map. Eyes on a flat plate set just
  above the skin.
- **UV smear:** re-unwrap (xatlas or Tripo Smart UV, not Blender Smart UV on a crumpled decimate) or give the piece
  its own plate.

## Asymmetry
- Symmetry preview first; slide off-centre features onto the plane, then apply.
- A pose baked in (curled tail, one leg forward): straighten it on the source or in the rig's rest pose before
  mirroring weights.
- Off-axis (generators keep the hero's 3/4 turn): the plane search's yaw straightens it.
- Ask the image generator for a perfectly symmetric pose: it saves most of this.

## Stretch in poses
Weights as cut planes with an even band plus sideways falloff; loose shells (eyes, claws, shards) take ONE weight
set where they touch the host; keep head yaw small on wide necks and get look-around from body yaw. Weights must
depend on position only. > 30 % stretch on a visible facet reads as a glitch.

## Over budget
1. Find the pieces that cost most (the per-piece triangle lines).
2. Overlays first: fewer glow-strip segments, merged decals, gems as low cones.
3. Wasted areas next: generators often spend half the budget on toes and feet; decimate only those (lock the rest
   with a vertex group).
4. Body: decimate half and mirror, weight silhouette rims so the outline survives, give the head a fixed budget.
   Check silhouette loss against the original by number, not by eye.
5. Never cut the silhouette to fit; cut surface detail and rig.
