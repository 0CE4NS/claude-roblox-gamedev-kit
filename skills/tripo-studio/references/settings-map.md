# Tripo Studio settings map (mapped Oct 2026)

Mapped on a Pro account with an in-page helper (`T.map()` / live `T.cost()`). Costs are what the action button
showed with the other settings at their defaults; "trial" means a struck-through cost and 0 (a free member trial).
**Tripo ships changes often: re-map when the UI looks different and update this file with the date.**

## Account
- Plans have a monthly Studio credit allowance and a renewal date; read the live balance from the header
  (`T.balance()`) and the plan and usage history from the account page.
- Studio credits only work in Studio. The CLI / API use separate API credits.

## Rail tools (route = `/workspace/<route>[/<model-slug>]`)
| Rail | Route | Needs a model | Cost |
|---|---|---|---|
| Image | `generate-image` | no | image models (below); paid plans include some free generations a month |
| Model | `generate` | no | 30-70 (HD Model), 35 / 100 (Smart Mesh) |
| Segment | `segmentation` | yes | 40 |
| Segment ▸ Fill Parts | `fill-parts` | segmented model | per part |
| Retopo | `retopology` | yes | 40 |
| Smart UV | `smart-uv` | yes | 20 |
| Texture | `texture` | yes | 20 |
| Texture ▸ Edit (Magic Brush) | `texture-edit` | yes | 5 a preview |
| Texture ▸ Upscale | `texture-upscale` | yes | 10 |
| Texture ▸ PBR | `texture-pbr` | yes | 5 |
| Rig | `rigging` | yes | 20 |
| Rig ▸ Animate | `animate` | rigged model | 20 |

Open an existing model by clicking its card in the right-hand Assets list; the rail links then carry that slug.

## Model ▸ HD Model
- **Input mode:** single image · multi-view (Front / Left / Right / Back slots) · batch (up to 10 images, one model
  each) · text (prompt ≤ 1,000 chars + a T-Pose chip). JPG / PNG / WEBP ≤ 20 MB. After an upload the panel offers
  **Generate Multi-Views** (makes the other views from one image).
- **AI Model:** H3.1 (best quality, default) · H3.0 (fast and balanced) · H2.5 (legacy). Ultra Mesh Quality only
  applies to H3.1.
- **Geometry & Texture** popover:

| Setting | Values (default) | Cost effect |
|---|---|---|
| Ultra Mesh Quality | switch (on) | +15 |
| AI Complete | switch (off) | 0 (improves the input image, minor changes) |
| Texture | switch (on) | off saves ~25 |
| Texture Quality | 2K · 4K (on) · 8K | 2K −10, 8K +10 |
| Remove Lighting | switch (on) | 0 |
| PBR | switch (on) | +5 |
| Topology | Quad · Triangle (on) | Quad +5 |
| Polycount | 500 – 2,000,000 | 0 |

- Members: Generate in Parts (+5), 8K texture, **Privacy** Public / Private / Sharing Only.
- Default total ~55.

## Model ▸ Smart Mesh (clean low-poly, designed topology)
- Same input modes and Privacy.
- **Topology** popover: Quad (default) · Triangle; **Polycount 500 – 25,000** (default 5,000); **Use Same Polygon
  Count** (on). Off = one slider per variant (Model 01-04).
- **Number of generations:** 1 / 2 / 4 (default). The cost doesn't change with the number.
- **AI Model:** **P2.0** (100) · P1.0 Fast (35).
- For game creatures: 4 variants at different counts under your budget for one price, so the budget is met at
  generation instead of by decimating.

## Image (`generate-image`)
- Prompt (≤ 10,000 chars) + optional reference image; templates **Character Extraction**, **T Pose**, **3D
  Enhance**.
- Models offered in Oct 2026: GPT Image 2 / 2.5, Nano Banana / 2 / Pro, Midjourney v8.2. Aspect 1:1 / 16:9 / 9:16 /
  4:3 / 3:4; 1-4 images; 4K switch.

## Segment, Retopo, Smart UV, Texture
- **Segment:** Detail Level Simple 3-6 parts · Balanced 6-15 · Detailed 15+ (40). Then **Fill Parts** completes
  each part's hidden surfaces.
- **Retopo:** Quad / Triangle; Smart Mesh P1.0 switch; Polygon Count 500 – 10,000 (40).
- **Smart UV:** shows the current UV utilisation; Unwrap UV (20). **Removes the current texture.**
- **Texture:** style reference (image / multi-view / text / none), Remove Lighting, 2K / 4K / 8K (20). Edit = Magic
  Brush (Creativity Strength 0-1, preview 5). Upscale 4K / 8K (10). PBR (5).

## Rig and Animate
- Rigging Type **Humanoid** (T- or A-pose; presets ActorCore · Mixamo · UE5 · VRM · Unity) or **Other** (any shape).
  Auto Rig 20. Humanoid gives a ~65-bone skeleton with finger bones (strip them for creatures).
- **Animate:** text-to-motion (20), multi-stage motion, and a library of ~95 humanoid presets (free; clicking a tile
  applies it at once).

## Export (top-right Export on a model)
- File name; **Format** USD / FBX / OBJ / STL / GLB / 3MF; FBX target Blender / Mixamo / 3ds Max; **Texture
  Resolution** 512 / 1k / 2k / 4k / 8k; **Send to** (DCC Bridge add-ons for Blender, Unity, Unreal, Roblox and more;
  untested here).
- Rigged models: Export Skeleton switch, Number of Animations (Select All), Animation stay in Place.

## Page quirks
- Plain `el.click()` doesn't pick select options: use real pointer events (the helper's `T.pick`).
- Closed popovers stay in the DOM (`data-state="closed"`); help dialogs ignore Escape: close them by their button.
- Screenshots can lag one action behind: read state from the DOM, not pixels.
- Settings persist per browser in localStorage. Put back what you changed for a test.
