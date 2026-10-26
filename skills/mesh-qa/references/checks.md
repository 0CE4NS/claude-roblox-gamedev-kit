# mesh-qa checks: what each one measures and when it fails

All distances are fractions of the model's largest bounding-box side (`size`). Render checks use 320 px images
(74 views, or 26 for a quick run); "px" means pixels in those images. These thresholds worked on a real project;
tune them with the user's reviews.

## Topology (welded mesh, rest pose)
| Check | Measures | FAIL / WARN |
|---|---|---|
| `open_loops` | boundary-edge groups after welding all pieces (weld 1e-5·size). Shape: pinhole (< 2 % perimeter; usually a T-junction crack from cutting along a colour border), crack (long and thin), opening | FAIL on any not in an `--open` piece; the report then checks which are VISIBLE |
| `floaters` | islands not touching the body or any attached part (gap > 0.5 % size and no triangle overlap) | FAIL |
| `inner_shells` | islands whose vertices are ≥ 75 % inside a bigger closed island (generated inner skin, sunk parts) | FAIL |
| `double_walls_pct_area` | faces with a near-parallel face within 0.4 % size: `stacked` (same direction) and `facing` (inside-out inner skin). Overlays and thin back-to-back parts don't count | FAIL ≥ 1 % of area |
| `inverted_faces_pct_area` | faces of closed islands whose outward ray hits the island an odd number of times | FAIL ≥ 0.1 % |
| `nonmanifold_edges` | edges with 3+ faces (fused sheets; blocks decimation) | WARN |
| `sliver_area_pct` | triangles with a corner < 3° | WARN ≥ 2 % |
| `triangles` | total triangles of everything rendered | FAIL > the budget |
| `colour_splotches` | extra patches of a colour on the body, < 25 % of that colour's main area and < 1 % of total area, ≥ 85 % surrounded by one other colour | FAIL on any |
| `stray_facets` | the same with a single face | WARN > 2 |
| `symmetry_p95_pct` | mirror distance of vertices across the best plane | WARN ≥ 0.5 % |
| `uv_smear_pct_area` | textured faces with < 1/3 of the median texel density (streaks) | WARN ≥ 2 % |
| `stretch_<pose>` | faces whose edges stretch > 30 % in that pose (read as glitches) | WARN |

Per-piece lines (triangles, loops, floaters, shells per piece) are the first place to look. Body pieces should be
clean; overlay pieces (strips, eye plates) are expected to have loops.

## Renders
| Check | Measures | FAIL / WARN |
|---|---|---|
| `see_through` | regions ≥ 6 px of magenta (visible back faces) in any view of any pose; DoubleSided pieces excluded only when named | FAIL |
| `floating_pieces` | silhouette components detached from the main body (after closing 1-px gaps); with an original, bits the original also shows detached don't count | FAIL |
| `holes_vs_original` | the original covers, ours doesn't, inside our outline | FAIL |
| `jagged_colour_borders` | share of colour-border pixels a radius-2 majority filter changes (sawtooth teeth, notches). Colours that only appear as thin lines are skipped | WARN ≥ 0.05, FAIL ≥ 0.12 |
| `specks_per_view` | colour islands < 8 px | WARN ≥ 1 |
| `silhouette_asymmetry_pct` | rest pose front / back silhouette vs its mirror | WARN ≥ 2 % |
| `visible_open_loops` | 3D loops whose projected centre shows magenta in some view | FAIL |
| textured models | when < 85 % of pixels match a small palette, compare fine-scale colour noise between versions instead | compare |

## Appeal (fresh critic, after the gate passes)
Spawn a new subagent with no history. Give it the turntable renders, the clips (or strips) and the in-game angle
shots, **not** the references, design notes or build history. Prompt:

> You are a regular player of <reference game> who collects <pets / creatures>. Here is a new one for a Roblox game
> like it. First impressions: what is it, how rare does it look, would you want it, what looks off? Then (second
> message) here are 8-12 <pets> of the same kind from <reference game>: compared with them, what reads cheaper or
> wrong (shape, proportions, colours, eyes, surface, motion)? Give severity-ranked findings (severe / medium /
> minor) and a "keep these" list.

Severe findings are fixed before the user sees it; the rest go in the NOTE with a verdict each.
