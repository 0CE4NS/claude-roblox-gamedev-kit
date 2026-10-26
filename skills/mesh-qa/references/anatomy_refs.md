# Anatomy and motion references (before any design image or model)

Goal: know the creature's real structure and motion well enough that the design, the mesh, the rig and the clips are
right the first time. Thin references produced stiff "running" idles, wings folding the wrong way, bird feet under
heavy legs and lumpy inconsistent toes.

## Folder
`assets/inbox/<Asset>/refs/` with `photos/`, `anatomy/`, `motion/` (strips) and `SOURCES.md` (one line per file:
file, URL, author, licence, what it shows).

## Where to look
| Need | Source | How |
|---|---|---|
| Clean photos (side, front, top, under) | Wikimedia Commons | API: `https://commons.wikimedia.org/w/api.php?action=query&generator=search&gsrnamespace=6&gsrsearch=<animal> side view&prop=imageinfo&iiprop=url\|extmetadata&iiurlwidth=1280&format=json`; pause between downloads (rate limits) |
| Skeleton / muscle / proportion diagrams | Google Images, Wikimedia, museum open access (Smithsonian Open Access, Biodiversity Heritage Library) | open the source page and download the original; reference use only |
| Motion | YouTube slow-motion clips (`<animal> slow motion walk / takeoff / swim`), Muybridge sequences (public domain) | `game-research` skill: `yt_grab.py` + `strip.py` for 8-12 frame strips with exact times |
| Fantasy parts (wings on a quadruped, crowns, crests) | real analogues: bat / bird wing anatomy, real crowns, real horns | same |
| Art-style fit | the reference game's own models (`game-research` style study) | |

Google Images is for finding; download from the source page (real resolution, licence visible) and note it in
SOURCES.md.

## Anatomy card (write it before the design brief)
```
Creature: <name>   Real analogue(s): <animals>
Counts: legs N (front/hind), wings N, fins N, toes per foot N, tail segments, horns / crest
Proportions (measured on the side photo, as ratios): head length : body length = ..., leg length : body depth = ...,
  wing span : body length = ..., tail : body = ..., eye position (fraction along the head) = ...
Joints and what bends: spine (which part bends), shoulder / hip axes, wrist fold, tail base
Rest pose for the design images: straight, symmetric, limbs mirrored, mouth closed unless designed
Motion notes from the strips: idle (breath period, what sways), main move (keys with timing), special move
Silhouette must-haves: the 3-5 features that make it read at game distance
Must NOT: (extra limbs, wrong fold direction, ...)
```

## Using the card
- Design brief: attach 2-4 of the clearest refs, state counts and proportions from the card, ask for a straight
  symmetric pose. Check every panel against the card (limbs per side, extra appendages) before any 3D step.
- Keyframes: from the motion strips (real timing), not from imagination; loop periods divide the loop length.
- Rig: pivots at the measured joints; fold directions from the photos (feathers point back and down when folded).
- After the 3D step: compare the model's side view with the side photo at the same scale; proportions within ~10 %.
