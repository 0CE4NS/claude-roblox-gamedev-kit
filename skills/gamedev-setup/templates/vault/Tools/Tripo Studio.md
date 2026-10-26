---
type: reference
status: current
updated: <YYYY-MM-DD>
---

# Tripo Studio

**A paid web app (studio.tripo3d.ai) that turns reference views into clean low-poly meshes, then UVs, textures,
rigs and animates them. Claude drives it in the user's signed-in browser.** A route in the [[Asset Build Pipeline]].
Step-by-step: the `tripo-studio` skill (its `references/settings-map.md` has every screen, setting and cost as
mapped in Oct 2026; Tripo changes often, so re-check live costs).

## Plan and credits
- Plan: <Free / Pro / …>, <N> Studio credits a month, renews <date>. Balance <N> on <date>.
- **API credits are separate** from Studio credits (the CLI / API route); Studio credits only work in Studio.
- **Ask before spending:** before the first generate / process press in a task, give the job list with each cost,
  the total and the balance, and wait for a yes. A yes covers that list only. Settings, cost reads and uploads are
  free. Never buy credits or change the plan.

## How Claude drives it
- **The user's browser via Claude in Chrome** (signed in): `file_upload` puts local reference images straight into
  Tripo's upload inputs (multi-view: Front / Left / Right / Back). The built-in browser pane can't upload local files.
- A small helper script injected into the page (map the panel, read the cost and balance, set controls, click
  Generate only at the expected cost) saves a lot of clicking. Settings are saved **per browser**, so re-read the
  cost before quoting it.
- **Exports** download a file: ask before the first one in a task. Move each GLB into the asset's folder right away.
- **Privacy:** models stay Private / Sharing Only, never Public. Other games' images are reference only, never
  generation input unless the user says so for that image.

## Costs (as mapped in Oct 2026; check live)
| Tool | Cost | Notes |
|---|---|---|
| Smart Mesh **P2.0** | **100 for 4 variants** | clean low-poly; Triangle topology for Roblox; Use Same Polygon Count off = a polycount per variant (500-25,000); overshoots ~5 % |
| Smart Mesh P1.0 (fast) | 35 | |
| HD Model H3.1 | ~55 with the default switches | up to 2M polys; too heavy for Roblox creatures without retopo |
| Smart UV | **20** | removes the current texture |
| Texture | 10 (2K) / 20 (4K) | style ref image / multi-view / text |
| Rig | 20 | Humanoid (Mixamo etc.) or Other |
| Animate | presets free, text-to-motion 20 | presets are humanoid only |
| Segment / Retopo | 40 / 40 | |
| Edit Mesh | free retries | **regenerates** the boxed region, not a repair |
| Upload 3D Model | free | bring a Blender-fixed mesh back in |

## The route that works
Orthographic views from the image generator (front / left / back, right = left mirrored, stud-free, T-pose, wings
spread) → **Smart Mesh P2.0 multi-view**, 4 variants at per-model counts under the budget → export all → `mesh-qa`
each, pick the cleanest → **Smart UV** (20) → export → **your own flat-area paint** from the views (clean areas,
round / almond eyes, mirrored) → **your own rig and clips in Blender** → `mesh-qa` in poses → split into
mutation-ready areas. **What it gives:** clean, symmetric low-poly topology at the triangle budget straight from the
generator (no decimating), good UVs (~70 %+ used). **Not used:** Tripo's own texture for stylised looks, its rig
presets (they sink wings and flippers into the body), Edit Mesh.

Gotchas: [[Lessons Learned]] → Tripo Studio.

Related: [[Asset Build Pipeline]], [[Modal and Open 3D Models]], [[Codex]], [[Blender and Roblox Studio]]
