---
type: guide
status: current
updated: <YYYY-MM-DD>
---

# Asset NOTE (template for `assets/inbox/<Asset>/NOTE.md`)

Every delivery gets one. It's the record of **how the asset was made**, so the next one of its kind starts from it
and a fresh chat can pick it up.

```markdown
# <Asset> (<tier / kind>, <area>) - delivery note

**<Two or three sentences: what it is, the route, triangles, bones and clips, where it stands (waiting on the
user's look / in the game).>**

![](<Asset>_compare.jpg)   <!-- renders next to the references -->

## User decisions
- <date>: "<the user's words>" → <what it changed>

## Numbers
| | |
|---|---|
| Triangles | **<N>** (limit <N>), <N> meshes (one per colour area), every piece counted |
| Height | <authored> → <in game, studs> |
| Rig | <N> bones (+<N> FX): <list> |
| Clips | <Idle, Walk, Run, Special…> |
| Spend | image generator <N jobs>, Tripo <N credits>, GPU <$> |
| Roblox ids | <mesh / kit / texture ids> |

## References (`refs/`)
| File | Why |
|---|---|
| <file> | <what it's for: style / anatomy / motion> |

## Route and tools
| Step | Tool | Result |
|---|---|---|
| Design | <image generator, prompt file> | <hero + views> |
| 3D | <Tripo Smart Mesh, 4 variants / hand-built / TRELLIS> | <files, tris each; which one won and why> |
| UV / paint | <tool / script> | <result> |
| Rig + clips | <script> | <bones, clips> |
| Areas | <script> | <colour areas, eye plates> |

## Checks
| Check | Result |
|---|---|
| mesh-qa rest + <N> clip frames | PASS / WARN (<what>) |
| Reference match | <proportions within ~10 %? overlay sheet> |
| Motion per part | <min degrees per part per clip> |
| Colour vs refs under game light | <numbers> |
| Appeal critic | <severe / medium / minor findings, fixed or accepted> |

## In the game
<What was uploaded, where it's wired in, what was tested in Play, the save (with the user's yes).>

## Open
- <known issues, choices for the user>
```

Related: [[Asset Build Pipeline]], [[Decisions Log]]
