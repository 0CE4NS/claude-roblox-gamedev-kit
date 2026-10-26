---
type: reference
status: current
updated: <YYYY-MM-DD>
---

# Blender and Roblox Studio

## Blender
- **Background builds** so the user's open scene is never touched:
  `"<path to blender.exe>" -b --factory-startup -P <script.py> -- <args>`. The user sees renders and clips at
  reviews. One live Blender (MCP) at a time if you use the Blender MCP.
- Pass **absolute paths** to Blender scripts (relative ones resolve against the drive root).
- `Image.pixels` returns sRGB; convert to linear for materials. Preview renders: the Standard view transform (AgX
  washes colours out).
- Set the scene fps to 30 before baking clips; reset the pose before saving.
- Keep each asset's build script and its arguments, so a fix rebuilds the same look.

## Roblox Studio (MCP)
- Connect Studio's MCP server so Claude can run Luau in Edit and Play, read the console, take screenshots and
  start / stop Play. Several Studio windows can be connected: pick by name.
- **The working copy:** `<build/Game.rbxl>`. **Ask before every save.** Never publish. The live game
  <place id> is the user's. Reference places are read-only.
- If the world is built at runtime, Edit shows an empty workspace: capture in Play. In Play, hold the client
  camera with `BindToRenderStep` for screenshots of particles; wait 2-4 s, then capture.
- Screenshots can leave the Edit camera Scriptable → reset it as the **last** Studio action (new Fixed camera,
  destroy the old one); delete temporary preview models.
- Other chats may share Studio: check before Play or screenshots; never stop someone else's Play test.
- **Sounds and private assets:** check in Play (`ContentProvider:PreloadAsync` on the client), never Edit.
- **Uploads:** an Open Cloud API key with asset upload permission lets Claude upload meshes, images and audio
  (keep the key encrypted locally, never in the repo or the vault). The same key type can publish places: never do
  that.
- **Pushing code** (no Rojo running): a small script that copies `src/` files into Studio module sources, or Rojo
  serve + the Rojo plugin.

## Importing skinned models
- 1 glTF unit = 1 stud; the importer turns the skeleton 180° about Y (head faces −Z).
- The Model holds `RootPart` (with the bones) and one MeshPart per glTF mesh (only the first primitive of each mesh
  is kept: one mesh per colour area). Delete the extra `AnimationController` and `InitialPoses` if the game runs its
  own rig code.
- Anchor the RootPart only; skinning only deforms pieces in its assembly.

Related: [[Asset Build Pipeline]], [[Rigging and Animation]], [[Working Together]]
