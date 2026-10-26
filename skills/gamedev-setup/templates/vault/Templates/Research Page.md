---
type: guide
status: current
updated: <YYYY-MM-DD>
---

# Research page (template: one question per page)

When the user has to pick a direction ("how do other games do trampoline tiers?"), answer one question per page
with pictures, not a wall of text. Images go in `vault/_media/research/<slug>/` (JPG ≤ 1280 px, clips MP4 ≤ 6 s or
GIF ≤ 3 MB).

```markdown
---
type: reference
status: current
updated: <YYYY-MM-DD>
---

# <Question as a title>

**The question:** <one line>. **It decides:** <what we'll build differently depending on the answer>.

## What they do
<8-12 frames or clips from at least 3 games or sources, mixed in source, angle (the game camera first) and state.>

![[<img>.jpg|400]]
*<Game> — <file + time or URL> — <what it shows>.*

- <3-6 observed bullets; guesses marked as guesses; every number dated.>

## Options for us
| Option | Looks like | Effort | Trade-off |
|---|---|---|---|
| **A** (recommended) | ![[<img>.jpg\|200]] | <S / M / L> | <…> |
| B | | | |
| C | | | |
```

Related: [[Reference Games]], [[Dashboard]]
