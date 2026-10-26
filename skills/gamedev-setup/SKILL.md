---
name: gamedev-setup
description: Set up a Roblox game project for working with Claude the "second brain" way - an Obsidian vault (status, design, Style Bible, how the user works, pipelines, tools, Decisions Log, Lessons Learned seeded with ~90 rules), a CLAUDE.md that points at it, and auto-loaded memory files - by interviewing the user about their game, tools, budgets and Studio rules and filling the templates with their answers. Use when the user asks to set up this kit, the vault or the workflow, says "set me up", or starts a new game project and wants the build-asset, mesh-qa, game-research, vfx-review and tripo-studio skills to have a vault to read.
---

# gamedev-setup

You are setting up a project for someone who wants to use this workflow: an Obsidian vault as your second brain,
a `CLAUDE.md` that points at it, auto-loaded memory files, and the kit's other skills (`build-asset`, `mesh-qa`,
`game-research`, `vfx-review`, `tripo-studio`). The workflow comes from a working Roblox project; everything
game-specific has been taken out. Your job is to fit it to **this** user's project.

The templates are in this skill's folder: `templates/vault/`, `templates/CLAUDE.md`, `templates/memory-starter.md`.

Work in this order. Don't skip the interview: the templates are only useful once they hold the user's real answers.

## 1. Read the templates
Skim every file in `templates/vault/`, `templates/CLAUDE.md`, `templates/memory-starter.md` and the other kit
skills' `SKILL.md` files. Note every `<placeholder>` you'll need to fill.

## 2. Look at the project
- What's in the folder already: a Rojo project (`default.project.json`, `src/`)? A `.rbxl` file? An existing
  `CLAUDE.md`, `vault/` or `.claude/` folder? Git?
- If there is an existing `CLAUDE.md` or `vault/`, **don't overwrite them**: show the user what you'd merge and ask.
- Which tools are installed and reachable: Blender (find the exe), Python, Roblox Studio + its MCP server (try
  listing studios), a browser tool, an image generator (Codex or other), Tripo, Modal. Write down what you find.

## 3. Interview the user (one message, short questions, they can skip any)
1. **The game:** name, genre, one-paragraph pitch, the core loop in one line.
2. **Reference games:** which 1-3 games should it look and feel like? Which parts of them (models, UI, world,
   effects)? Anything from them to never copy?
3. **Look:** art style in a few words (low-poly, blocky, smooth, studded, cel-shaded…), colour mood, any hard rules
   (triangle budget per asset, texture size, materials).
4. **Tools:** which of these they have and are OK with you using: Blender, an image generator, Tripo Studio, Modal /
   open 3D models, Claude in Chrome. **Spending limits** for anything paid (per task and per month).
5. **Studio rules:** the place file you'll work in, whether there's a published place you must never touch, when you
   may save (default: ask every time), whether you may upload assets to Roblox from their account.
6. **How they like to work:** how often to check in, how much you decide alone, how they give feedback, what annoys
   them. Whether to stop near usage limits.
7. **Their first task.**

## 4. Put the files in place
| From this skill | To the project |
|---|---|
| `templates/vault/` | `<project>/vault/` |
| `templates/CLAUDE.md` | `<project>/CLAUDE.md` (merge if one exists) |
| `templates/memory-starter.md` | not copied: follow it to make the memory files |

Copy with a plain file copy (on Windows never rewrite text through PowerShell `Get-Content` / `Set-Content`). Then
fill every `<placeholder>` from the interview:
- `CLAUDE.md`: the game name, the paths, the hard rules.
- `vault/Design.md`: the pitch and the loop. `vault/Style Bible.md`: the look rules, budgets, reference games.
- `vault/Preferences/Working Together.md`: their answers to 5 and 6, plus the general rules already there.
- `vault/Project Home.md` → Now: the current state and their first task. `vault/Dashboard.md`: the same for them.
- `vault/Tools/*`: delete the notes for tools they don't have (or mark them "not set up"); fill in the paths and
  limits of the ones they do have.
- The other kit skills use `<…>` for budgets and paths: tell the user which values they read from the Style Bible
  and Working Together, so those notes are the single place to change them.
- A first `vault/Log/Decisions Log.md` entry: "Project set up from the gamedev kit", with the date and their main
  answers.

Keep the user's own words where you can. Each rule gets a one-line *why*.

## 5. Memory
Follow `templates/memory-starter.md`: one memory file per hard rule (saving, publishing, spending, budgets, the vault,
one chat per task) and the `MEMORY.md` index.

## 6. Check and hand over
- Open `vault/Home.md` and click through: every `[[link]]` should go to a note that exists (or is meant to be
  written later).
- Tell the user, briefly: what you set up, what you filled in, what is still a placeholder, which tools are missing,
  and how to open the vault in Obsidian (**Open folder as vault** → `vault/`).
- Then start their first task in a **new chat** (one chat per task), from a brief (`vault/Templates/Task Brief.md`).

## Rules for you on this project from now on (they also live in the vault)
- Read `vault/Home.md` and `vault/Project Home.md` → Now first in every chat. Search
  `vault/Log/Lessons Learned.md` before any task.
- Ask before saving any place, spending money or credits, deleting things, or publishing. Never publish.
- Show your work with pictures and numbers. Nothing goes live without the user's yes.
- After a mistake or a fix, add a one-line rule with a one-line why to Lessons Learned.
- Keep the Dashboard (the user's view) and Project Home → Now (yours) current at every delivery.
