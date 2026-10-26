---
type: log
status: current
updated: <YYYY-MM-DD>
---

# Lessons learned

**Short rules by topic, each with a one-line why.** Search here before a task. After a mistake or a fix, add one
rule under its topic (the why in a few words, the story in [[Decisions Log]] if it matters).

The rules below came from a real project's mistakes (Roblox, low-poly creatures, October 2026). They're a head
start. Keep the ones that fit, delete the ones that don't, and add your own as you go.

## Showing work
- **Run the `mesh-qa` gate on every build, in every pose; a see-through, inner-shell, inverted or floater FAIL is
  never integrated.** *Why:* a creature shipped with hundreds of flagged cracks; the user found them in minutes.
- **Reproduce the user's exact view (angle, crop, zoom) before calling a fix done.** *Why:* "fixes" checked from
  other angles still showed the problem from the user's angle.
- **Re-shoot from the game's own camera distance before calling a visual bug.** *Why:* a "black slab" was the
  player's own prop in a too-close capture.
- **Look at a frame of every GIF / render before sending it.** *Why:* GIFs went out with black blobs for eyes.
- **Check every creature from below too** (pitch −60…−80). *Why:* fin undersides kept generator stripes no sheet
  showed.
- **Before showing a concept, count limbs per side and look for extra appendages.** *Why:* a duplicate mini wing
  went to the user unnoticed.
- **Before saying something is gone, search the whole place (ServerStorage too).** *Why:* the user was told models
  were removed that had only been swapped.

## Feedback and taste
- **Change only what the user named; list every change.** *Why:* an eye got huge while only its socket was meant to
  change.
- **When the user approves a look, freeze it and change only what they name.** *Why:* every tweak after a liked
  preview made it worse.
- **A budget redesign starts from the current hero image** (keep pose, proportions, fullness). *Why:* a "budget"
  rebuild read as a different, cheaper creature.
- **Low detail ≠ soft shapes:** ask for faceted, angular planes and sharp shards. *Why:* "no fine detail" got a
  vinyl toy.
- **A fix for motion must not change how an effect looks.** *Why:* swapping clouds for beams to fix lag made a liked
  effect look broken.
- **Rule-consistency ≠ looking good:** UI is built from measured real stills and judged side by side.
- **Sculpted form comes from image-to-3D, Tripo or hand-building; code is for rig, motion and effects.** *Why:*
  fully procedural / code-built whole bodies were rejected every time; code-built parts and effects on a real body
  were liked.
- **Replace small repeated parts (feet, claws) with one designed part, mirrored.** *Why:* generated toes of
  different sizes read "inconsistent".
- **Concept heroes follow the user's own references feature by feature.** *Why:* added features (three tail bars
  instead of the reference's one ribbon) made it "not epic".

## Image-to-3D and meshes
- **TRELLIS exports are double-walled:** drop inner shells before anything else; ask for ~2x the target faces.
  *Why:* "ugly texture" patches were the inner wall showing through holes.
- **Weld (`remove_doubles`) before any component logic or decimation.** *Why:* glTF splits vertices at UV seams; a
  filter deleted most of a body, and a decimate tore a textured import open along every seam.
- **Decimate the whole closed mesh first, then cut and cap.** *Why:* cutting wings before remeshing shredded the
  body.
- **Decimate a generated mesh, don't rebuild it:** the raw mesh decimated to budget usually keeps the look; rebuilds
  changed it completely. Start the collapse ratio around 0.15 and compare raw vs low from the same cameras.
- **No voxel remesh on thin open sheets** (feathers, fins). *Why:* a remesh dropped the texture and shredded the
  fins.
- **Decimate half and mirror** for symmetric creatures. *Why:* QEM decimation is lopsided even on a symmetric
  source.
- **Judge decimation by numbers** (silhouette difference vs the original), not by eye. *Why:* a "fine" render missed
  a quarter of the silhouette.
- **Split edges shared by 3+ faces before collapse decimation, weld after.** *Why:* decimation stalled.
- **Blender Decimate vertex group marks what collapses,** not what's kept. *Why:* inverted, the body collapsed and
  the toes stayed.
- **Re-export the GLB after every post-rig .blend edit.** *Why:* an uploaded model never had its shoulder fix.
- **Separate colour areas by cutting the body along the outline,** not by laying a plate on top. *Why:* six plate
  methods failed (poke-through, crumpling, slivers).
- **One glTF primitive per MeshPart:** set every face to material slot 0 before export. *Why:* Roblox kept only the
  first of 13 primitives.
- **Roblox names MeshParts after the mesh data:** rename `o.data.name` too.
- **Image-to-3D from a 3/4 hero misreads foreshortened parts** (a tail came out twice as long): feed it the side
  view or fix the length before rigging.

## Colour and textures
- **Colour by smooth per-vertex (or surface-point) fields, cut triangles along the border;** never colour whole faces
  by rule. *Why:* every face-level rule left sawtooth borders ("rough jagged colouring").
- **Per-texel projection of reference views gives splotchy colours and stepped eyes.** Vote areas, smooth them as
  surface fields, mirror, remove islands, paint eyes analytically (round / almond), anti-alias. *Why:* "unclean
  texturing, eyes aren't round, colours get splotchy".
- **Vertex-level smoothing is too coarse on low-poly meshes:** small markings on big faces vanish; smooth on a fine
  grid of surface points instead, or cut a small marking in along its outline.
- **Thin painted features (smile, nostrils) and small eyes vanish when you cut a texture into areas** (thinner than
  the triangles): lay designed plates instead.
- **Judge colour on a flat unlit render;** judge white / pale models in a lit render.
- **Eyes are flat textured plates in a shallow socket,** uploaded opaque (Roblox draws transparent TextureID pixels
  black). *Why:* domed eyes "pop out".
- **One unusable map blanks the whole SurfaceAppearance;** fresh uploads can be "not approved" for minutes; Studio
  caches a failed fetch per id. Diagnose with a Decal board, re-upload under new ids.
- **`MeshPart.TextureID` switches off a MaterialVariant** (e.g. a stud material). For a textured asset that also
  needs a material pattern, split it into one untextured part per colour area and colour them in Roblox.
- **Emissive is not Neon:** warm emissive glows drift to yellow-white; keep warm glows as real Neon pieces.
- **Keep metalness low (≈0.15) for bright cartoon gold.** *Why:* it reflected the sky as olive.
- **A small important area (the face) gets its own piece / map.** *Why:* sharing a 1024 map made it blurry.
- **Blender's glTF exporter embeds a re-imported image's original bytes,** not your edited pixels: load the saved PNG
  as a new image, set it on the node, then export.

## Rigs and animation
- **Set Blender's scene fps to 30 before baking;** Blender re-imports glTF at 24 fps, so sample clips in seconds.
- **Reset the pose before saving** after unassigning an action. *Why:* QA's "rest" was a clip frame.
- **Review every auto-rig skeleton** (pivots at real joints, names, missing / extra bones); edit bones and weights
  yourself. *Why:* an auto-rigger gave a winged creature 78 bones with collapsed wing chains.
- **Run a stretch map at each clip's extremes; > ~30 % on a visible facet reads as a glitch.** *Why:* a "head
  glitch" was neck facets stretching 65 % with a clean gate.
- **Skinning weights must depend on position only:** different weights per colour area at shared border verts tore
  the legs open when posed.
- **Weight regions with cut planes and even bands, not boxes;** loose shells (eyes, claws) take one weight set.
- **Keep head yaw small (±4°) on wide necks;** get the look-around from body yaw.
- **Lower a T-pose limb about the root's lower edge:** turned about its middle the underside swings into the body
  (inverted armpit faces).
- **Single-sheet membranes and frills go in a part the game makes DoubleSided,** by bone weight share. *Why:*
  thousands of see-through regions in flap poses.
- **A skinned kit's origin is the creature's pivot:** export it under the body (the root bone), not at the bounding
  box middle. *Why:* a long tail put the pivot behind the body and it swung round while turning.
- **Human animation presets press wings and flippers into the body:** rig your own clips for creatures.
- **Real motion, measured:** every part moves in every clip (≥ ~5°), light parts lag (follow-through). *Why:* clips
  capped at 0.5°-1.5° read stiff.
- **`ScaleTo` doesn't scale `PointLight.Range` or `ParticleEmitter.ZOffset`;** don't animate from cached model-space
  numbers, test rigs at 0.5× / 2× / 3×.
- **Particles locked to a part don't follow a skinned bone:** move invisible anchored parts onto the bone's
  `TransformedWorldCFrame` each frame.
- **`Bone.WorldCFrame` already includes the animation,** while skinned MeshParts sit at rest: place attachments from
  the rest frame (parent * `Bone.CFrame`).

## References and checks
- **Draw references for 3D in a straight, neutral pose** (body straight, fins / wings spread); the curve comes from
  the rig.
- **Gather references broadly:** Google Images, Pinterest, Reddit, YouTube, wikis, the image generator; log sources.
- **Check moving effects by their motion:** 6-10 consecutive frames ~0.1 s apart, measure direction and speed.

## Roblox Studio
- **Particles only simulate inside the player's real camera view:** hold the client camera on the subject every
  frame, wait 2-4 s, then capture. Screenshot camera parameters alone show "missing" particles.
- **Captures land 1-2 s after the call:** slow short effects (`TimeScale`) or freeze them.
- **Screenshots can leave the Edit camera Scriptable:** reset it as the last Studio action.
- **Skinning only deforms pieces in the RootPart's assembly:** anchor the RootPart only.
- **`ModuleScript.Source` set from code fails at ≥ 200,000 characters:** split big data into several modules.
- **Never paste code from a popup into Studio;** check Toolbox models for hidden scripts (fake "Roblox" popups,
  obfuscated loaders) before using them.

## Effects
- **Self-reviews pass bad effects:** use a blind fresh reviewer against a goal board (`vfx-review` skill). *Why:* an
  effect went through eight rejected rounds while self-reviews kept approving.
- **A Beam lays an image's width across the ribbon and its height along it.** Test a new beam texture on a flat
  strip first.
- **Judge effects by day:** additive white washes out against the sky.

## Research
- **YouTube bot checks** ("Sign in to confirm you're not a bot"): a `yt-dlp.conf` with
  `--extractor-args youtube:player_client=web_embedded,mweb`, one download at a time, no cookies.
- **Every number needs a video frame or the API;** fan wikis go stale within a week.
- **Creator overlays look like game UI:** confirm on a second video.
- **Prices in videos may be member prices** (Roblox Premium discounts): take list prices from the API.

## Image generator
- **At most ~5 attached images per job;** copy refs into the working folder first.
- **Repeat a pose instruction plainly** when the tool keeps turning a head.
- **Fix concepts with an edit pass** (attach the draft, list only the fixes, "keep everything else").

## Tripo Studio
- **Edit Mesh regenerates the boxed region,** it doesn't repair: it made see-through holes worse. Repair in Blender
  (weld, fill holes, decimate) and re-upload.
- **Smart UV: the first Save may not register;** check the GLB for `TEXCOORD_0` before painting.
- **Generate in Parts exports untextured parts:** run Texture on the part model afterwards.
- **A one-sided fold shows up only when a pose turns it to the camera:** snap the midline and mirror the clean half.
- **Multi-view wings come out as front-facing sheets** when the front view shows the feather fan: roll the wing for
  flight and spread the chest → arm weight blend over ~1 stud.

## Tooling traps
- **Windows / PowerShell 5.1:** never rewrite source with `Get-Content` / `Set-Content` (BOM, mojibake, lost CRLF);
  use the Edit tool or Python. `Out-File` adds a BOM. Empty `""` arguments to native exes get dropped.
- **Blender resolves relative paths against the drive root:** pass absolute paths to Blender scripts.
- **Keep the build command and arguments in every build's JSON.** *Why:* a lost command had to be reconstructed.
- **Hoist expensive expressions out of big comprehensions.** *Why:* `max(xs)` inside one froze a build for 10 min.
- **Committing only your changes in a shared tree:** compile what you stage (`git show :<file>`), not what's on disk.
- **Collect GPU jobs as soon as they finish;** cost estimates that count until collection overstate the bill.

Related: [[Working Together]], [[Style Bible]], [[Asset Build Pipeline]], [[Rigging and Animation]], [[Tripo Studio]],
[[Decisions Log]]
