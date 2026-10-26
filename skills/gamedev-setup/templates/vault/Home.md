---
type: index
updated: <YYYY-MM-DD>
---

# <Game name> — project vault

The single place for **how we build this game**: what the game is, what the user likes and dislikes, how assets
are made, and what we learned. Written for both people and AI agents: short, current, linked. Read this note first,
then only the notes your task needs.

**The user's view:** [[Dashboard]] (what's being worked on, what waits on them, latest pictures).

## Map

**One owner per topic.** If two notes seem to disagree, the owner wins.

| Topic | Owner | Also |
|---|---|---|
| **What's happening now** | [[Project Home]] → Now | [[Dashboard]] (the user's view) |
| **What the game is** | [[Design]] | feature notes as they're written (one per feature: islands, pets, eggs, UI, shop…) |
| **How it looks** | [[Style Bible]] | galleries and look books as they're made |
| **How we work with the user** | [[Working Together]] | memory files (auto-loaded) |
| **How assets are made** | [[Asset Build Pipeline]] | [[Rigging and Animation]] · [[VFX and Particles]] · [[Asset Library]] |
| **Tools** | [[Codex]] · [[Tripo Studio]] · [[Modal and Open 3D Models]] · [[Blender and Roblox Studio]] | |
| **Other games** | [[Reference Games]] | one study note per game (`game-research` skill) |
| **History** | [[Decisions Log]] | |
| **Mistakes to avoid** | [[Lessons Learned]] | |
| **Templates** | `Templates/` | [[Task Brief]] · [[Asset NOTE]] · [[Research Page]] · [[Decision Entry]] |

`_archive/` holds replaced notes. Read them only for history; never follow a rule from them that the owner note
doesn't repeat.

**Start here:** [[Project Home#Now|Project Home → Now]] says what's happening now and links to everything it
touches. Before a task, also search [[Lessons Learned]] for earlier mistakes on the same topic.

## Rules for this vault

- **One topic per note**, titled plainly. Put a note in the folder of its area; add it to the map above.
- **Frontmatter on every note:** `type` (index / guide / reference / log), `updated` (YYYY-MM-DD), and `status`
  where it helps (current / proposal / superseded).
- **State facts and rules, not chat history.** Each rule gets a one-line *why* (usually user feedback). Dated
  history goes in [[Decisions Log]], not in topic notes.
- **Link, don't duplicate.** Code lives in the repo (`tools/`, `src/`), assets in `assets/library/`, step-by-step
  procedures in the skills. Notes summarize and point there. Add a rule to its owner note, not to a second note.
- **Keep notes short.** A topic note holds current rules and gotchas. When a build diary grows past a screen, move
  the dated part to `_archive/` and keep the lesson.
- **No asset dumps in the vault.** Images that explain a rule may sit in `_media/`. Production assets belong in
  `assets/library/` ([[Asset Library]]).
- **Link everything related** with `[[Note Name]]`. Every note ends with a `Related:` line so the graph stays
  connected. Link to headings without dates in them, so a heading edit doesn't break links.
- **Keep it current.** Every task chat updates [[Project Home#Now|Project Home → Now]], the [[Dashboard]] and the
  [[Decisions Log]] when it delivers something. When something changes, edit the owner note and bump `updated`.
  Mark replaced approaches `status: superseded` with a link to the new one; move whole replaced notes to
  `_archive/`.
