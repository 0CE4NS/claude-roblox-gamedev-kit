# Analysis checklists

Answer each with evidence (file + time). Skip what the scope excludes. Date every observation.

## Creature / pet models
- Style families (box / voxel, hybrid low-poly, smooth) and which creatures belong to which.
- Surface: which masses are textured or patterned, smooth parts (claws, horns, eyes, trims), facet size, outlines,
  gradients, neon / glow.
- Proportions: head : body, chunkiness, size range in game (creatures vs player height), "big" variants.
- Rarity cues: colours, auras, particles, size, labels. Variants / mutations (golden, rainbow…) and how they change
  the look.
- Eyes and faces: textured or geometry, expressions.
- Effects on creatures: which layers, where they attach, how they move with the body.

## Animation
- Idle (period, what moves, sleeping variants), walk / roam gait, fliers' hover, special / attack, hatch reveal.
- Timing from strips: frames per step, bob amplitude relative to body height, secondary motion (tails, ears,
  wings).
- What players mock (sliding, stiff marches): comments in transcripts are evidence.

## UI layout
- HUD map: corners, hotbar, currency, buttons (icon + colour + label), server feed, timers.
- Panel anatomy: header bar colour per panel, body texture, card style, price pills, close button, tabs.
- Fonts (weight, outline), corner radius, border thickness, icon style.
- Text tone: tutorial copy, banners, toasts.

## UI motion (strips at 0.1 s or finer)
- Open / close: scale / slide / fade, overshoot, duration (frames × spacing).
- Feedback: button press, number count-ups, income pops, toasts (enter, hold, exit), confetti, screen shake, colour
  flashes, progress bars.
- Attention: pulsing "NEW", bouncing arrows, glints on offers.

## Gameplay loop
- Step list from first join: spawn → goal → actions → rewards → upgrades → repeat; timers and numbers at each step.
- Risk / pressure (chasers, stealing, defence), social hooks (feeds, trading, PvP), offline earnings.
- Progression: zones, gates, rebirths, indexes, quests.
- What keeps people playing, per the transcripts (creators' excitement and complaints).

## Monetization
- Passes and products with prices, grouped into families (currency packs, boosts, skips, luck, eggs / gacha,
  cosmetics, event items); price points, the cheapest entry offer, repeated price sets (the same price ladder on
  every limited item = a template).
- **Date the store from the API:** creation dates show each drop; a bulk update date on many products = a price
  change day. Compare with the previous study.
- **API vs seen in game:** mark every product as seen in a video (file + time) or "API only - unverified".
- **Prices in videos may be member prices** (Premium discounts): take list prices from the API.
- **Free vs paid path:** badge award counts show what free players reach; items with both a cash and a Robux price
  mean Robux is a skip, not a gate.
- Where offers appear and when (after a loss, at a timer, pop-ups), limited / weekly stock, bundles, odds
  disclosure.
- Creator behaviour: do video makers buy on camera? A "spending run" video shows the whole store in order.
- Free monetization: rewarded ads, friend boosts, group rewards.

## Events (keep apart from the permanent game)
- Timeline: update log lines, badge creation dates, video upload dates, wiki edit dates.
- Per event: name, start / end, event currency, event-only loop or area, event eggs / creatures, shop, quests,
  staged unlocks.
- Status for every finding: permanent / event:<name> / unknown.

## World
- Plot / base layout, zone themes (colour, light, hero props), terrain treatment, signage, NPCs.
- Scale: player vs props vs creatures; traversal (jumps, mounts, portals).
