---
name: tripo-studio
description: Drive Tripo Studio (studio.tripo3d.ai, on the user's own signed-in account) from the user's browser via Claude in Chrome (uploads local refs) or Claude's built-in browser for this Roblox game - read and tune every setting (HD Model, Smart Mesh low-poly with per-variant polycounts, texture, PBR, segment, retopo, Smart UV, rig, animate, image models, export), check live credit costs, ask before spending, upload reference images, run generations and export GLBs. Use whenever the user mentions Tripo, Tripo Studio or Smart Mesh, or wants a creature, egg or prop made with Tripo or compared against other image-to-3D models. The build-asset skill uses it as a route; mesh-qa gates its output.
---

# tripo-studio

Tripo Studio is a web app. Claude runs it on the user's signed-in account. The map of screens, settings, defaults
and costs (as of Oct 2026) is `references/settings-map.md`; read it first, and re-check live costs: Tripo changes
often.

**Which browser:**
- **Default: the user's own browser via Claude in Chrome** (load the browser tools in one ToolSearch, including
  `file_upload` and `javascript_tool`). Signed in there, and **`file_upload` puts local reference images straight
  into Tripo's upload input**. Work in a new tab; close it when done.
- **Built-in browser pane** when Chrome isn't connected: works for settings and generation, but **can't upload local
  files** (then the user drags the file onto the upload box, or use a public https copy of your own image).
- Settings are saved **per browser**, so two browsers can show different defaults and costs: always re-read the cost
  before quoting it.

## Standing rules
- **Credits are the user's.** Changing settings, reading costs, uploading a reference and mapping panels are free.
  **Pressing a generate / process button spends credits:** before the first one in a task, give the user the job
  list with the cost of each, the total and the balance, and wait for a yes. A yes covers that list only. Never buy
  credits or change the plan.
- **Never sign in or out** for the user; if the page shows "Sign up / Log in", ask them to sign in.
- **Exports download files:** ask before the first download in a task (file name, format).
- **Privacy:** keep models **Private** or **Sharing Only**, never Public (unreleased assets). Other games' images
  are reference only: never the generation input unless the user says so for that image.
- **Leave settings as you found them** after an exploration.
- Output is a candidate: it goes through `mesh-qa` and the `build-asset` steps before the user sees it as finished.

## Start
1. Open a tab at `https://studio.tripo3d.ai/workspace/generate`. Cookie banner: reject non-essential. Check the
   header shows a credit balance, not "Sign up / Log in".
2. **Inject a helper** (write `tools/tripo/studio_helpers.js` the first time; see below) with `javascript_tool`.
   Re-inject after any full page load.
3. Close any Guide / "View Your Model" dialogs.

## The helper (`window.T`, write it once)
Tripo's controls are Radix-style popovers and selects where a plain `el.click()` often does nothing, and screenshots
can lag one action behind. A small in-page helper makes this reliable:
| Call | Does |
|---|---|
| `T.map()` | outline of the current tool panel: each control's label and state, the action button's cost, tooltips |
| `T.cost()` / `T.balance()` | the action button text with its cost / the credits in the header |
| `T.go('<route>')` | switch rail tool (routes in the settings map) keeping the open model |
| `T.open('<text>')` | click a tab, popover trigger or button by its text (`'Smart Mesh'`, `'Topology'`, `'Export'`) |
| `T.mode(i)` | input mode: 0 image, 1 multi-view, 2 batch, 3 text |
| `T.pick(regex, label)` | choose a select option with real pointer events |
| `T.setSwitch(label, bool)` / `T.setSeg(label)` / `T.setRadio(label)` / `T.setNumber(label, n)` | set controls; each returns the new cost |
| `T.generate(expectedCost)` | **spends credits**: clicks the action button only if its cost matches |
Read state with `T.map()` / page text, not pixels.

## Getting reference images in
1. **Claude in Chrome:** `find` "image file input" (multi-view mode has four: Front / Left / Right / Back) →
   `file_upload` with the ref and the absolute local path(s). The box shows "Uploading…", then the image. Remove a
   test image with the bin icon.
2. **The user drags the file** onto the upload box; Claude does everything else.
3. **No upload:** text mode, or Tripo's Image tool (T Pose / Character Extraction templates).

## Workflows
- **Tune for a job:** go to Model → Smart Mesh or HD Model → set each control → `T.map()` to confirm → state the
  cost → (after the user's yes) `T.generate(cost)` → watch the Assets list until the new card finishes → check the
  balance dropped by the cost.
- **Creature candidate (≤ the triangle budget):** Smart Mesh **P2.0**, multi-view, **Triangle** topology (Roblox
  is triangles), 4 generations with Use Same Polygon Count **off** and Model 01-04 at e.g. 60 % / 75 % / 95 % / 95 %
  of the budget (two at the top = two seeds; Tripo overshoots ~5 %). One price for four candidates.
- **Bake-off:** the same refs through Tripo and through another route; `mesh-qa` on each, one board.
- **UVs:** Smart UV on the winner (it removes the texture); check the exported GLB has `TEXCOORD_0` (the first Save
  may not register).
- **Rig:** non-humanoid creatures use Rigging Type **Other**; the Animate presets are humanoid only and press wings
  and flippers into the body, so creature motion stays with your own Blender rig.
- **Getting the mesh out:** Export (GLB; texture 1k-2k for Roblox) with real clicks after the user OKs the
  download. Files land in Downloads; move them into the asset's folder right away. Some browsers write the whole file
  to a temp file before their Save dialog: a small script that waits for a complete GLB (check the glTF header length)
  and copies it out saves clicking the dialog. A browser may allow only one automatic download per site until the
  user allows multiple downloads; that's the user's setting to change.
- Judge variants from your own renders (`mesh-qa` + a fixed-camera Blender render), not Tripo's viewer (each variant
  opens at its own camera).
- **Re-map** after a Tripo update: `T.map()` on each rail tool and popover, diff against the settings map, update it
  with the date.

## Learned the hard way
- **Edit Mesh regenerates** the boxed region; it isn't a repair tool (it made see-through holes worse). Repair in
  Blender (weld, fill holes, decimate) and bring it back with **Upload 3D Model** (free) if you need Tripo again.
- **Tripo's own texture** gives a soft painted look; for flat stylised areas, paint your own from the views.
- **Generate in Parts** exports untextured parts: run Texture on the part model afterwards.
- **Export of a rigged model:** Export Skeleton on, all animations, "Animation stay in Place" on (the game moves
  the creature).
