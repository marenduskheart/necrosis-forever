# Prompt for ChatGPT: new bottom banner

Attach `strip.png` (the current banner, all five pieces side by side) with this prompt.

---

I'm redesigning the bottom banner art for a World of Warcraft addon (a warlock
UI called "Necrosis Forever"). It's a wide decorative bar along the bottom
edge of the screen, behind the action bars. I've attached the current version
as one stitched image so you can see the layout.

## What the current banner looks like

- A flat, full-width bar in grungy purple with a stone/concrete texture, dark
  speckles and a darker shade along the bottom.
- In the middle, the top edge rises into a rounded hump. Inside it is a dark,
  recessed circle (a socket where a button sits).
- Black tribal-style thorny flourishes run out from the circle along the top
  edge of the bar, plus two tribal hooks that curve down below the circle.
- The overall look is edgy and dated, from around 2010 (it's based on
  SpartanUI).

## What I want instead

Keep the same layout: a full-width bar with a raised center socket. Change the
style to:

- **More modern:** clean shapes, smooth gradients, a subtle glassy or
  satin-metal finish, soft inner glow, thin crisp edge lines. No grunge
  texture and no heavy tribal spikes.
- **A bit more feminine:** graceful curves, elegant filigree or vine-like
  flourishes instead of thorns, maybe small crescent-moon or star accents,
  soft highlights. Keep it a warlock theme: elegant and arcane, not cute.
- **Color:** a purple/violet base, with room for plum, amethyst, or a hint of
  rose/magenta in the highlights. Fine gold or silver accent lines are welcome.
- **Center socket:** keep a circular recess about 160 px across, centered
  horizontally and roughly at mid-height. Give it an elegant frame such as a
  thin ring, a crescent, or filigree.
- Keep detail low enough that the bar reads well behind buttons: the action
  buttons sit on top of the flat part, so that area should stay calm.

## Technical constraints (these matter)

The banner is made of **5 separate textures** placed edge to edge, left to
right, with no gaps:

| Piece | File | Size (px) |
|---|---|---|
| 1 far left | `base-left2.png` | 1024 x 256 |
| 2 left | `base-left1.png` | 512 x 256 |
| 3 center | `base-center.png` | 512 x 256 |
| 4 right | `base-right1.png` | 512 x 256 |
| 5 far right | `base-right2.png` | 1024 x 256 |

The full strip is **3584 x 256 px**.

- **Transparent background (PNG with alpha).** Everything above the bar must
  be fully transparent.
- **Bar height:** the flat bar fills the bottom of the canvas, from y = 102
  down to y = 256 (about 154 px tall). Its top edge is a straight line at
  y = 102 across all pieces except the center.
- **Center hump:** only the center piece (x = 1536 to 2048 in the full strip)
  rises above the bar, peaking around y = 29 at its middle and blending back
  into the y = 102 line by its left and right edges. The decorative flourishes
  may extend a little onto pieces 2 and 4, but must stay inside the bar
  (y >= 102) there.
- **Seamless joins:** the pieces must line up perfectly where they meet
  (x = 1024, 1536, 2048, 2560). Avoid hard features exactly on a seam.
- **Symmetry:** mirror left and right around the center (x = 1792).
- **The outer pieces stretch:** pieces 1 and 5 are stretched horizontally to
  reach the screen edges, so they should hold a plain, even texture or gradient
  with no circles or emblems that would look distorted when stretched.
- The game shows the image exactly as drawn (no tint), so design it in its
  final colors.

## What to deliver

One image, **3584 x 256 px, transparent PNG**, laid out exactly as described,
so I can cut it into the five pieces. If you can't produce that exact size,
use the same 14:1 aspect ratio with a transparent background and I'll resize it.
