# Memory starter (for Claude)

Claude Code loads a memory index (`MEMORY.md`) into every chat for this project. It sits in Claude Code's own
project memory folder (`~/.claude/projects/<project-folder-name>/memory/`). Use it for **short, durable rules and
pointers**. The vault holds the details. A memory should point at the vault note that owns the full rule.

## Format
One fact per file, with frontmatter:
```markdown
---
name: studio-rules
description: ask before saving any place; never publish; check other chats before Play
metadata:
  type: feedback
---

Ask before saving any place (the user answers each time). Never publish. Before Play or screenshots, check that no
other chat is using Studio; never stop someone else's Play test. Full rules: vault/Preferences/Working Together.md.
**Why:** the published game and its data are the user's.
**How to apply:** before any save, ask; before Play, check the other sessions.
```
Types: `user` (who they are), `feedback` (how they want work done, corrections included, with the why), `project`
(ongoing goals and constraints; absolute dates), `reference` (links to outside things).

`MEMORY.md` is the index, one line per memory:
```markdown
- [Studio rules](studio-rules.md) — ask before saving any place; never publish; check other chats before Play
```

## Starter set (fill from the interview)
| File | What it says |
|---|---|
| `studio-rules.md` | when to save, never publish, the live place is the user's, shared-Studio etiquette |
| `project-vault.md` | the vault is the second brain: Home.md first, one owner note per topic, status in Project Home → Now, history in Decisions Log, lessons in Lessons Learned |
| `one-chat-per-task.md` | one chat per task from a brief with the user's own refs; subagents for bounded side jobs |
| `spending.md` | every paid tool, its cap, "ask before every spend" |
| `design-rules.md` | pointer: look and budget rules live in the Style Bible, which wins |
| `qa-before-showing.md` | run the mesh-qa gate in every pose, then play every animation by hand before showing |
| `feedback-rules.md` | on feedback: reproduce the user's exact view, change only what's asked, list every change, handle the whole message in one round |
| `usage-limit.md` | quality first, never ration; stop and save state by ~97 % of the usage limit |
| `windows-utf8.md` (Windows only) | never rewrite source with PowerShell Get-Content/Set-Content (BOM + garbled text) |

## Rules
- Check for an existing memory before writing a new one; update it instead of duplicating.
- Delete memories that turn out wrong.
- Don't save what the repo or the vault already records. Save the non-obvious part.
