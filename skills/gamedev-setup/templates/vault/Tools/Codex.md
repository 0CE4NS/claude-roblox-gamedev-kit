---
type: reference
status: current
updated: <YYYY-MM-DD>
---

# Codex (image maker)

**Role: images only** (concept art, blueprints, orthographic views, textures, effect sprites, flipbooks), run
headless by Claude. Claude does the design. Any other image generator works the same way (GPT Image, Nano Banana,
Midjourney, Tripo's Image tool); keep the brief and the plan → generate → self-review → report steps. Briefs:
[[Style Bible]] §4.

## Running it headless (Codex CLI / desktop app)
```
codex exec -C "<image work folder>" --skip-git-repo-check -s workspace-write -m <model> \
  -c model_reasoning_effort=high -o result.txt -i ref1.png -i ref2.png -   # prompt on stdin
```
- Windows desktop app: the exe is under the `OpenAI.Codex` app package (`Get-AppxPackage OpenAI.Codex` →
  `InstallLocation\app\resources\codex.exe`).
- Use `high` effort for textures, the highest for sheets and hero images. A job takes 2-15 min: run it in the
  background.
- **At most 5 `-i` images.** Copy references into the work folder first. Label each one's role in the prompt.
- Drafts may land in `~/.codex/generated_images/` first; other sessions' drafts land there too, so check the folder
  you asked for.
- Keep the image work folder separate from the game repo, so the tool can't touch game code.

## Prompts that worked
- **Hero + model sheet** for image-to-3D: one creature, 3/4 front, straight symmetric pose, limbs clear of the
  body, even soft light, plain #D9D9D9, no floor / shadow / particles; "count before saving: exactly <1 head, 2 wings,
  4 legs>".
- **Keyframe sheet** (attach the hero + real motion photos): row 1 the active loop in 4 keys from one camera with a
  dashed rest-height guide line; row 2 idle, idle look, special wind-up, special. The keyframe sheet is the target
  the clips are tuned against.
- **Particle texture:** "one game particle texture for a Roblox ParticleEmitter: <a soft puff>. 512x512 PNG,
  GREYSCALE ONLY (the game tints it), on pure #000000 black (drawn additively), fading to black well before the
  edges. No text, no frame, no watermark."
- Always: "The design is decided; follow it exactly, do not redesign."

Related: [[Style Bible]], [[Asset Build Pipeline]], [[VFX and Particles]], [[Asset Library]]
