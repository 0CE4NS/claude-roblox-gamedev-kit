---
type: guide
status: current
updated: <YYYY-MM-DD>
---

# Asset library

**Where outside assets come from, where they're filed, and how their source is recorded.** Every asset the game
uses can say where it came from.

## The sourcing ladder (try in this order)
1. **What we already have:** `assets/library/` (search `INDEX.md`), earlier deliveries in `assets/inbox/`.
2. **Make it:** the image generator for textures, sprites and concept art ([[Codex]]); Blender for meshes.
3. **Free and open:** ambientCG, Poly Haven (CC0 textures), Kenney.nl (CC0 sprites, UI, icons), OpenGameArt,
   itch.io free packs, freesound.org (read each licence).
4. **Roblox Creator Store / Toolbox:** search with 3-5 phrasings; insert into `ServerStorage.__Sourcing` in a **test
   place**, inspect, delete readme / credit scripts, **check every script** for hidden loaders and fake popups.
5. **Another game's files** (decompiled copies, captures): reference only, unless the user says otherwise per asset.
   Ask: "From <game> (<file / id>): <what it is>. I'd like to use it as <direct / modified / reference only> for
   <our asset>. OK?" Numbers (rates, lifetimes, curves) are always fine to learn from.

<What the user pre-approved: …>

## Where things go
| What | Folder |
|---|---|
| Research boards, downloaded refs, video strips | `references/<topic>-<YYYY-MM-DD>/` + `SOURCES.md` (not in git) |
| Image generator outputs | its own output folder, then the used ones filed in the library |
| Raw downloaded packs (before sorting) | `assets/library/_incoming/<name>-<date>/` |
| Production assets | `assets/library/<category>/<name>-<source>-<date>/` with a `NOTE.md` and a row in `INDEX.md` |
| Third-party tools / repos | `tools/third_party/<name>-<date>/` |

Downloads and extracted archives are untrusted: each in its own new folder; run any script that reads them with
`python -I`, from a different folder.

## NOTE.md per library asset
```
# <name> (<YYYY-MM-DD>)
- Source: Toolbox <id> by <creator> <url> | image generator (prompt: <file>) | <site> <url> | user's file <path>
- Licence / permission: (CC0 / site licence / Creator Store terms / user-approved on <date>)
- Roblox id(s): ...        Original files here: ...
- What it is: (8x8 flipbook, 64 frames, greyscale, 1024², ...)
- Used for: <asset / effect>, place <name>
- Changes we made: ...
```

Related: [[VFX and Particles]], [[Codex]], [[Working Together]]
