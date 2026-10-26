# Claude Roblox Gamedev Kit

Skills for building a Roblox game with **Claude Code** the "second brain" way. Claude keeps an **Obsidian vault**
with what the game is, how you like work done, how assets get made and every mistake it has made before. On top of
that it gets procedures for making 3D creatures and props, checking them with measurements before you see them,
studying other games and making effects.

It comes from a real Roblox project (October 2026). Everything game-specific is stripped out: you get the workflow,
the rules and the lessons.

## Skills

| Skill | What it does |
|---|---|
| [`gamedev-setup`](skills/gamedev-setup/SKILL.md) | **Start here.** Interviews you about your game, tools, budgets and Studio rules, then sets up the vault, `CLAUDE.md` and memory files with your answers |
| [`build-asset`](skills/build-asset/SKILL.md) | Creature / pet / prop models: references → route (hand-built, Tripo, image-to-3D + clean-up, parts; a bake-off for a new kind) → rig → reference-match, motion, colour and mesh checks → delivery → your look |
| [`mesh-qa`](skills/mesh-qa/SKILL.md) | A measured gate before any mesh is shown: see-through holes, floaters, inner shells, messy colour borders, pose stretch, asymmetry, the triangle budget; in-game angle shots, a fresh appeal critic, a three-round limit |
| [`game-research`](skills/game-research/SKILL.md) | Dated studies of other Roblox games (Roblox page, passes and products, wiki, YouTube transcripts and frame strips, style ranking) and one-question picture pages for decisions |
| [`vfx-review`](skills/vfx-review/SKILL.md) | Particle effects and glows built to a written spec and judged by a blind fresh reviewer against a goal board |
| [`tripo-studio`](skills/tripo-studio/SKILL.md) | Driving Tripo Studio in your browser: settings and credit costs, uploads, Smart Mesh variants at your triangle budget, export. Always asks before spending |

The vault templates (about 25 notes, including a **Lessons Learned** list with ~90 rules from real mistakes) are in
[`skills/gamedev-setup/templates/vault/`](skills/gamedev-setup/templates/vault/).

## Install

```bash
npx skills add 0CE4NS/claude-roblox-gamedev-kit
```

That installs into the current project; add `-g` to install for every project. Or install it as a Claude Code
plugin:

```text
/plugin marketplace add 0CE4NS/claude-roblox-gamedev-kit
/plugin install gamedev-kit@claude-roblox-gamedev-kit
```

Or clone the repo and copy `skills/*` into your project's `.claude/skills/`.

## Get started

1. Install [Claude Code](https://claude.com/claude-code) and [Obsidian](https://obsidian.md) (free).
2. Install the skills (above) and open Claude Code in your game's project folder.
3. Say **"set me up with the gamedev kit"** (or run `/gamedev-setup`). Claude asks about your game, then makes
   `vault/`, `CLAUDE.md` and the memory files.
4. In Obsidian: **Open folder as vault** → your project's `vault/`. Start at `Home.md`; your own view is
   `Dashboard.md`.
5. One chat per task: start each chat from the brief in `vault/Templates/Task Brief.md` with your own reference
   images.

## What you need

| Tool | Needed? | Used for |
|---|---|---|
| Claude Code | **yes** | everything |
| Obsidian (or any Markdown editor) | **yes** | reading the vault |
| Roblox Studio + its MCP server | **yes** for game work | Claude edits the place, runs Play tests, takes screenshots |
| Blender + Python | **yes** for 3D | building, cleaning, rigging, rendering and checking models (run in the background) |
| An image generator (e.g. Codex) | strongly recommended | concept art, orthographic views, blueprints, textures, effect sprites |
| Tripo Studio | optional (paid) | reference views → clean low-poly meshes at your triangle budget |
| Modal + TRELLIS / Hunyuan3D | optional, advanced | open image-to-3D models and auto-rigging for bake-offs |

**About scripts:** the kit is all Markdown. The skills call Python and Blender scripts. Each skill says exactly what
each script must do (inputs, outputs, what it measures, the thresholds), so your Claude writes its own the first time
and keeps it in your project. The first model takes longer than the ones after it.

## How to get good results

- **Big, varied references:** 8-12 images of the target style plus 6-10 real-world photos for anatomy and motion.
- **Measure, don't eyeball:** the `mesh-qa` gate and fresh critic subagents catch what a quick look misses.
- **Small reviewed steps:** one change, a picture or a number, then the next.
- **Feedback rules:** Claude looks at exactly what you looked at, changes only what you named and lists every change.
- **Log every mistake** as a one-line rule in Lessons Learned. This is what makes it get better over time.
- **Nothing goes live without your yes;** Claude asks before saving your place and never publishes.

## Licence

MIT. Use it, fork it, change it.
