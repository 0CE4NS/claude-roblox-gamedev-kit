---
name: game-research
description: Dated research on another Roblox game for this project - recent YouTube gameplay with transcripts and frame strips, the Roblox page (passes, products, prices, badges, update log), a wiki crawl with original images, and analysis of pets/creatures, animation, UI, UI motion, loop, monetization, events and world; also style studies that rank a game's real images by similarity, and one-question picture pages for decisions. Use when the user asks to study, research or "see how X does it" for another game, wants style references from one, or has to pick a direction between options.
---

# game-research

Turn "look at how game X does it" into a dated, sourced study the whole project can reuse: a references folder of
clean stills, strips, transcripts and wiki originals, plus a vault note whose every claim points at a file and a
time. If a study of the game exists already (`vault/References/`), read the latest one and do a delta, not a redo.

## Ground rules
- **Reference only by default.** Other games' assets, IDs and code are for analysis; we make our own. Using
  something directly is the user's call per asset.
- **Games change weekly.** Every finding carries a date and a status: `permanent`, `event: <name> (dates)` or
  `unknown`. A loop that exists only in a holiday event is not the core loop.
- **Observed vs inferred.** Write what a frame or a page shows; mark guesses as guesses. Prices and player counts
  are snapshots: date them.

## Tools (write them the first time in `.claude/skills/game-research/scripts/`; run with `python -I`)
Needs Python, `yt-dlp` and `ffmpeg`. No logins or cookies anywhere.

| Script | What it must do |
|---|---|
| `roblox_game.py "<name>"\|<place/universe id>\|<url> <dir>` | From Roblox's public web APIs (games, game passes, developer products, badges, thumbnails): page stats, description (often the update log), every pass and product with its price and creation / update dates, badges with dates and award counts, icon and screenshots. Writes `game.md` (products grouped into families, a content timeline from creation dates) and `products.csv`. |
| `wiki_crawl.py <https://x.fandom.com> <dir> [--category Pets …]` | Every article (or categories) as text + infobox fields, categories, last-edit date, event / limited hint tags, original images (full resolution); `index.md` + `SOURCES.csv`. Skip joke stubs. |
| `yt_search.py <out.json> "q1" "q2" [--days 30 --n 15]` | Recent videos with upload date, length, views, channel (yt-dlp, no API key). |
| `yt_grab.py <id> <dir> --tag <n-channel-topic>` | Metadata, a clean `[mm:ss]` transcript, contact sheets stamped with each tile's REAL time; delete the video afterwards unless `--keep-video`. |
| `strip.py <id\|video> <out> --at 12:41` / `--from 12:40 --to 12:43 --n 12` | Full-resolution stills / motion strips at exact times; from a YouTube id, download only those seconds. |
| `img_study.py <out> --images <dirs> --seed <confirmed dir> [--query ours.png]` | Dedupe (perceptual hash), measure style traits (palette, saturation, edge density, outline, facet size), cluster, rank candidates by similarity to confirmed in-game images, write a style card and an ours-vs-theirs table. |

## Workflow
### 0. Scope and set up
Pick the areas that matter (default: all). Folder: `references/<game-slug>-study-<YYYY-MM-DD>/` with `roblox/ wiki/
videos/ creatures/ animations/ ui/ ui-motion/ world/ mechanics/ monetization/ events/ style/` and `SOURCES.md`. Name
files `<game>-<what>-<mmss|source>.jpg`.

### 1. The Roblox page
`roblox_game.py` into `roblox/`. The description is usually the update log: copy the dated lines into the events
timeline. Products and passes are the monetization backbone. Mark which products you later SEE in a video and which
are API-only.

### 2. The wiki
Find the wiki(s) (pick the one with most articles and recent edits). Crawl the content categories (pets, eggs,
mutations, areas, events, mechanics, updates, codes). Wikis are fan-edited: cross-check numbers against videos.

### 3. Videos
`yt_search.py` with several queries (`"<game> update"`, `"<game> new"`, `"<game> guide"`, `"<game> all pets"`,
`"<game> event"`, `"<game> shop robux"`), last 30 days. Pick 6-10 covering different areas and channels, newest
first; write in `SOURCES.md` why each was picked. For each: `yt_grab.py` → read the transcript first → skim the
sheets → pull stills and strips of the key moments. Motion (gaits, hatch reveals, UI open / close, counters) needs
strips at 0.1-0.25 s spacing; measure durations from the stamped times.

**"Sign in to confirm you're not a bot":** put `--extractor-args youtube:player_client=web_embedded,mweb` in a
`yt-dlp.conf` in the current folder, run one download at a time. Check `--list-subs` before picking a small channel.
Long downloads: run them in the background.

### 4. Analyse each area
Checklists: `references/analysis_checklists.md`. Every bullet gets a file + time (`ui/<game>-shop-0416.jpg`,
video 3 at 12:41).

### 5. Events vs permanent
A dated timeline (update log, badge dates, video dates, wiki edits). Signs of event content: "Limited Time",
countdowns, event currency, an event area, seasonal theme, items missing from newer videos. Keep event-only loops
apart from the core loop.

### 6. Style study (when models or art matter)
Confirm a seed set (wiki originals + your own cropped in-game stills), search Google Images for more (`"<game>"
<creature>`, large size), download originals from their source pages, run `img_study.py --seed`: high similarity =
same game and style; low = other games, fan art, thumbnails. Read the style card and compare your renders with
`--query`. Feed the top-ranked images to the asset's reference set.

### Question-led research: picture pages
When the user has to pick a direction, answer **one question per page** from the existing studies plus targeted
new downloads: `vault/Templates/Research Page.md`. 8-12 frames or clips from at least 3 games or sources, captioned;
3-6 observed bullets; 2-3 options for us with effort and trade-off, one marked (recommended). Link it from the
Dashboard; the user picks.

### 7. Write up
Vault note `vault/References/<Game> Study <YYYY-MM>.md`: what was studied and when, findings per area with images
and file + time, the events timeline, monetization tables, a style card, and "What this means for us (ideas, not
decisions)". Add it to `Reference Games.md` and the Home map; mark an older study superseded. Tell the user the 3-5
most useful findings with their images; show sheets, not walls of text.
