---
type: guide
status: current
updated: <YYYY-MM-DD>
---

# Rigging and animation

**How creatures get skeletons and clips that read as alive.** Steps: the `build-asset` skill §4. Gotchas: [[Lessons
Learned]] → Rigs and animation.

## Study reference rigs first
Before rigging a new body type, open one or two good rigs of the same type (open-source models, store assets you
own) in background Blender and measure them: bone count per chain, where the chains bend, how the motion is layered.
Copy the chain density and the motion layering, not the mesh.
| Body type | What to look for |
|---|---|
| Serpent / long body | a dense spine chain (10-15 bones) with a travelling wave passing down it |
| Bird / winged | wing chains (shoulder → elbow → wrist → tip), feather and crest chains of 4-6 bones, a tail chain |
| Quadruped | hip and shoulder pivots, a ring of geometry at every joint, IK-planted feet |
| Blocky rigid-part | many small parts each with its own key, phase-shifted per limb |

## Skinned rigs in Roblox
- One skeleton deforms one MeshPart per colour area; the pieces share bones by name.
- No Humanoid / AnimationController needed; pieces must stay **jointed to the importer's RootPart** (anchoring each
  piece disables skinning). Anchor the RootPart only.
- The importer turns the skeleton 180° about Y: the head faces −Z. 1 glTF unit = 1 stud.
- Bone motion is client-side (`Bone.Transform` doesn't replicate): run the rig in a client script.
- Name bones in one scheme for every asset (Root, Hips, Chest, Neck, Head, Jaw, Ear.L/R, Tail1-n, Arm / Forearm /
  Paw .L/R, Thigh / Shin / Foot .L/R, Wing chains), plus FX bones for effect anchors.

## Motion rules
- An **idle** and a real **gait** from the real animal (slow-motion footage, frame strips), plus a special move.
- **The whole model moves in every clip:** body, neck, head, limbs, tail, fins, ears, wings. Group the bones by part
  and measure the max rotation per part per clip; a part under ~5° fails.
- Starting minimums: **idle** head turn or tilt ≥ 10°, body bob or breathing ≥ 1.5 % of height, a weight shift;
  **walk / hop** body rise and fall ≥ 3 % of height, head counter-motion, side-to-side weight shift; **fliers** wing
  arc ≥ 60° in a flap, a held glide, take-off and landing.
- Feet planted, mirrored limbs, light parts and tails lag (follow-through), smooth transitions between clips.
- **Wing flaps that read as flight:** slow downstroke (~58 %) down and forward, fast upstroke with elbow and wrist
  folded; the tip lags (flexes up while the arm drives down); body nose-down ~10° and rising on each downstroke;
  legs curl in flight.
- **Closed wings tuck down** onto the flanks like a perched bird, tips toward the tail.
- Loop lengths divide evenly (e.g. periods that divide 12 s) so layered motions stay in sync.

## Rig checks before showing anything
1. **Region / weight map:** every vertex coloured by the bone that moves it, front / side / top / back. Features
   that should move but don't, limbs sharing a colour.
2. **Motion map:** vertices coloured by how far they move between two poses. Grey on a moving part = not animated;
   a colour jump between neighbours = will separate.
3. **Stretch map** at each clip's extremes: > ~30 % on a visible facet reads as a glitch.
4. **`mesh-qa` in posed frames** of every clip (holes open at joints in poses only).
5. **Scale test:** clones at 0.5× / 2× / 3× match the 1× pose × scale.

## Auto-rigging and generated motion (optional)
Auto-riggers (Tripo Rig, TokenRig) and text-to-motion models (UniMate, Tripo Animate) are starting points. Humanoid
presets press wings and flippers into the body. A hand-keyed rig matched the keyframe sheet far better than generated
clips on a dragon, except for the idle.

Related: [[Asset Build Pipeline]], [[Blender and Roblox Studio]], [[VFX and Particles]], [[Lessons Learned]]
