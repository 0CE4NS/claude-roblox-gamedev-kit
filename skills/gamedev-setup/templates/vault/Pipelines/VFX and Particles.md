---
type: guide
status: current
updated: <YYYY-MM-DD>
---

# VFX and particles

**What works for effects in Roblox, what was rejected and why.** The review loop is the `vfx-review` skill; the look
rules are [[Style Bible]] §5. Add every rejected round here in the user's words, so it's never tried again.

## Attachment and motion
- `LockedToPart` on an emitter parented to a **Bone** locks to the bone's anchored RootPart, not the animated bone.
  For particles that must ride a moving body part: invisible anchored proxy parts moved onto
  `Bone.TransformedWorldCFrame` every frame (`workspace:BulkMoveTo`), emitters on the proxies.
- Locked particles keep the offset they spawn with: on a fast host they hover off the edge.
- Attached ≠ static: fire needs birth, rise, fade and a reaction to motion. Permanent cards read as stickers.
- `Orientation = VelocityParallel` turns sprites into cards that go edge-on to the camera ("looks flat").
- Same-rate emitters fire in lockstep: jitter the rates (±30-45 %).

## Seeing it
- Roblox doesn't simulate emitters outside the player's real camera; a screenshot tool's own camera parameters don't
  count. Hold the client camera on the effect every frame, wait 2-4 s, then capture.
- Captures land 1-2 s after the call: slow short effects with `TimeScale`, or freeze them.
- `AlwaysOnTop` BillboardGuis may not appear in screenshots.
- **Judge by day.** Starting values that read: Brightness 1.1-2.5, LightEmission 0.3-0.55, colour ramps starting
  warm, not white. Full additive (LightEmission 1, Brightness 2) blows out.
- Lights and LightEmission usually need cutting (×0.25-0.4) under bloom.

## Textures and sizes
- A particle is drawn about 2 × Size wide; a flipbook cell whose shape fills 1/4 of the cell needs a particle 3-4×
  the visible size.
- 4x4 flipbooks at 12-16 fps look low frame rate in fast motion; 8x8 (64 frames) at ~30 fps reads smooth.
- Greyscale flipbooks + emitter colour ramps give the most control.
- A **Beam** lays the image's vertical axis along the beam: draw strips soft across the columns, constant down the
  rows; a Beam's image top sits at the first attachment. FaceCamera Beams need Segments > 1.
- Uploaded images are stored at ≤ 1024 px on the long side; fresh uploads can take minutes to show.
- Continuous emission tops out around 400 particles/s per emitter.

## Scale and cost
- `Model:ScaleTo` scales emitter Size / Speed / Acceleration but not `PointLight.Range` or `ZOffset`: rescale those
  yourself.
- Budget per asset: emitters, particles alive (≈ Rate × mean Lifetime) and lights. Many assets on a server is the
  real budget, not one.

## Glow without particles
- Moving glow on a skinned body: **Neon strips** on the mesh (DoubleSided) + **Beams** between FX bones with
  brightness pulses travelling down them. Decals can't follow a skinned mesh.
- Glow that grows inside a texture: an EditableImage painted at runtime + an emissive SurfaceAppearance mask (needs
  the EditableImage permission in Game Settings; test in a published test place).

## Tried and rejected
| Effect | What was tried | User's words | Do instead |
|---|---|---|---|
| <effect> | <approach> | "<quote>" | <what worked> |

Related: [[Style Bible]], [[Rigging and Animation]], [[Asset Library]], [[Lessons Learned]]
