# Roblox VFX facts that cost earlier rounds

The same facts, with a place for your own rejected rounds, are in `vault/Pipelines/VFX and Particles.md`. Keep this
list in sync with it.

## Attachment and motion
- `LockedToPart` on an emitter parented to a **Bone** locks to the bone's anchored RootPart, not the animated bone.
  Fix: invisible anchored proxy parts moved onto `Bone.TransformedWorldCFrame` every frame (`workspace:BulkMoveTo`),
  emitters on the proxies.
- Locked particles keep the offset they spawn with: on a fast host they hover off the edge.
- Attached ≠ static: fire needs birth, rise, fade and a reaction to motion; permanent cards read as stickers.
- `Orientation = VelocityParallel` turns sprites into cards edge-on to the camera ("looks flat"). Camera-facing
  sprites can only lean in SCREEN space: compute the lean from the camera each frame.
- Same-rate emitters fill in lockstep and fire together: jitter the rates (±30-45 %).

## Seeing it
- Roblox doesn't simulate emitters outside the player's real camera; a screenshot tool's camera parameters only
  move the camera for its shot → particles look absent. Hold the client camera on the effect, wait 2-4 s.
- Captures land 1-2 s after the call: short effects (0.2-0.5 s) never show unless slowed or frozen.
- `AlwaysOnTop` BillboardGuis may not appear in screenshots.
- Judge by day: additive white washes out against the sky. Starting values: Brightness 1.1-2.5, LightEmission
  0.3-0.55, colour ramps starting warm, not white.

## Textures and sizes
- A particle draws about 2 × Size wide; a flipbook cell whose shape fills 1/4 of the cell needs a particle ~3-4× the
  visible size.
- 4x4 flipbooks at 12-16 fps look low frame rate in fast motion; 8x8 (64 frames) at ~30 fps reads smooth.
- Greyscale flipbooks + emitter colour ramps give control; colour baked into the texture multiplies with the tint.
- A Beam lays its texture's VERTICAL axis along the beam: draw strips soft across the columns, constant down the
  rows.
- A FaceCamera Beam with Segments = 1 may not render (use 10). TextureSpeed = texture cycles per second.
- Uploaded images are stored at ≤ 1024 px on the long side; fresh uploads can take minutes to show.
- Continuous emission tops out around 400 particles/s per emitter; `Emit(n)` bursts are separate.
- A velocity-aligned particle's image top sits 90° clockwise of its motion.

## Scale and cost
- `Model:ScaleTo` scales emitter Size / Speed / Acceleration but not `PointLight.Range` or `ZOffset`: record the
  build values and rescale those yourself.
- Budget per asset: emitters, particles alive (≈ Rate × mean Lifetime) and lights. Many assets on a server is the
  real budget.

## Glow without particles
- MaterialVariant / SurfaceAppearance emissive lights hot tips; warm emissive colours drift to yellow-white (keep warm
  glow as real Neon pieces). Bloom carries halos.
- Moving light along a body = Beams between FX bones with a soft dash texture and travelling brightness pulses.
- Glow that grows inside a texture: an EditableImage repainted at runtime with an emissive mask (needs the
  EditableImage permission in Game Settings; test in a published test place).
