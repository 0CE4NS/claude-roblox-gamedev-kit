# <Game name> (Roblox)

<One or two lines: what the game is and what's happening right now, e.g. "a pet-collecting tycoon; building the
first island and six pets as a vertical slice".>

## Read first
1. `vault/Home.md`: the map of the Obsidian vault (Claude's second brain). One owner note per topic; the owner wins.
2. `vault/Project Home.md` → **Now**: what's happening, what's next, what waits on the user. The user's own view is
   `vault/Dashboard.md` (every task chat keeps it current).
3. The owner note for your task, then search `vault/Log/Lessons Learned.md` for the topic.

| Topic | Owner note |
|---|---|
| Status / next steps | `vault/Project Home.md` → Now |
| What the game is | `vault/Design.md` |
| How anything looks | `vault/Style Bible.md` |
| How to work with the user | `vault/Preferences/Working Together.md` + the auto-loaded memory files |
| How assets are made | `vault/Pipelines/Asset Build Pipeline.md` |
| History / decisions | `vault/Log/Decisions Log.md` |
| Mistakes to avoid | `vault/Log/Lessons Learned.md` |

`vault/_archive/` holds replaced notes: history only, never rules.

## Skills (`.claude/skills/`)
| Skill | Use for |
|---|---|
| `build-asset` | any creature, pet, character or prop model: references → route (hand-built / image-to-3D + clean-up / Tripo / parts; a bake-off for a new kind) → rig → motion, colour and mesh checks → delivery → the user |
| `mesh-qa` | the measured gate before any mesh is shown, uploaded or delivered (+ in-game angle shots, appeal critic, 3-round limit) |
| `game-research` | studying another Roblox game (videos, wiki, Roblox page, style study) or answering one question as a picture page |
| `vfx-review` | particle effects and glows with a blind review against a goal board |
| `tripo-studio` | driving Tripo Studio in a browser: settings, credit costs, uploads, generations, export |

## Where things live
- Game code: `<src/>` (`<src/shared/Config.luau>` holds the tunables).
- Studio working copy: `<build/Game.rbxl>`. Save only after the user says yes.
- Tools and scripts Claude writes: `tools/`.
- Assets: `assets/library/` (with provenance notes), asset deliveries in `assets/inbox/<Asset>/`.
- Research: `references/<game>-study-<date>/` (local, large, not in git).

## Hard rules (details in memory and Working Together)
- One chat per task. Each chat commits only its own changes and checks that no other chat is using Studio before
  Play.
- Ask before saving any place; never publish; the live game <place id, if any> and its data are the user's.
- Ask before spending money or credits (Tripo, Modal, anything paid). Monthly caps: <Modal $…>, <Tripo credits…>.
- ≤ <N> triangles per <creature / pet / prop>, every piece counted.
- Stop and save state (Project Home → Now updated, nothing running) by ~97 % of the usage limit.
- Windows: never rewrite source files with PowerShell `Get-Content`/`Set-Content`; use the Edit tool or Python.
