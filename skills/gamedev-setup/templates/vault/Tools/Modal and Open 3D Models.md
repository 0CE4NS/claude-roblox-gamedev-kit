---
type: reference
status: current
updated: <YYYY-MM-DD>
---

# Modal and open 3D models (optional, advanced)

**Image → 3D mesh, auto-rigging and text-to-motion on rented GPUs.** Useful for bake-offs against Tripo, or when you
don't have a Tripo plan. Needs a [Modal](https://modal.com) account and some Python; Claude can write the Modal apps
and an MCP server that wraps them. Skip this note if you don't use it.

## Budget rules
- **A monthly cap** (<$…>, account total). Check the spend **and** the Modal bill before every job (the bill lags).
  The MCP server should refuse jobs past the cap and have a `stop` tool that cancels jobs and undeploys apps.
- Apps scale to zero ~60 s after the last call; each extra container adds a ~90 s cold start. A high-quality
  TRELLIS job is ~3.5 min of an L40S (~$0.15).
- Collect jobs as soon as they finish; estimates that count until collection overstate the cost.

## Models that were worth it
| Model | What | Notes |
|---|---|---|
| **TRELLIS.2** (Microsoft) | one image → textured mesh | No text prompt, one image only: the image *is* the instruction. Exports are double-walled (an outer and a hidden inner skin): ask ~2x the faces and drop inner shells. Good settings for creatures: resolution 1536, full guidance interval `[0,1]` on the coarse and shape stages (20 steps, guidance 7.5), remesh off. |
| **Hunyuan3D 2.1** (Tencent) | one image → shape + PBR paint | Often closer to the reference's shape than TRELLIS. Licence: the Tencent Hunyuan 3D Community License **excludes the EU, the UK and South Korea** and has a large-company clause; read it and decide. |
| **TokenRig** | auto skeleton + skin weights | starting point only; review every bone |
| **UniMate** | text → motion clips | rigs ≤ ~70 bones; prompts like "An object …"; good idles, weak gaits on creatures |

## Input images for image-to-3D
- One subject, symmetric straight pose, even soft light, no cast shadows or specular hotspots (they bake into the
  texture), long-lens / near-orthographic camera (perspective distorts the mesh), plain background.
- An RGBA PNG with a real alpha skips background removal: use it when the subject is close to the background colour.
- Change one setting at a time; keep a seed that works.

## After generation (every time)
Weld → drop inner shells → decimate the whole closed mesh to the budget (start at a ratio of ~0.15) → symmetrize
(cut, mirror, weld) → flat colour areas → `mesh-qa` → fix only what it flags. Never rebuild the whole body.

Related: [[Asset Build Pipeline]], [[Tripo Studio]], [[Rigging and Animation]], [[Lessons Learned]]
