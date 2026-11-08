# Necrosis Forever

Personal fork of the Necrosis warlock addon for the WoW `_forever_` client.
License: GPL v2 or later (`_Copyright.txt`, `_GPL_V2.txt`). Keep both files; a
shared build must stay GPL with source included.

## Origin and upstream

- Forked from upstream `CKNOEP/Necrosis` `main` at `0443bd5` (tag `v160001-fix5`),
  which was identical to the installed CurseForge copy.
- Remote `upstream` = https://github.com/CKNOEP/Necrosis.git (push disabled).
  No `origin` yet; the repo is local only.
- Pick up upstream work: `git fetch upstream`, review with
  `git log --oneline main..upstream/main`, then `git merge upstream/main`
  (or `git cherry-pick <sha>` for single fixes).
- Expect conflicts on lines with texture paths: resolve by keeping upstream's
  change but with `NecrosisForever` in the path.

## What the fork changed (commit 472b2e9)

- Folder and all `.toc` files renamed `Necrosis*` -> `NecrosisForever*`; title
  "Necrosis Forever". The `_forever_` client loads `NecrosisForever_Camelot.toc`
  (Interface 16001, X-Min-Interface-Forever 160001).
- All `Interface\AddOns\Necrosis\` paths (about 850) point at `NecrosisForever`.
- Folder-name lookups use `NecrosisForever`: `NECROSIS_ID` (Constants.lua, also
  the AceLocale app name), `AppName` and `GetMetadata` in Initialize.lua /
  Initialize_retail.lua, `ADDON_NAME` in VersionCheck.lua.
- Kept on purpose: the global `Necrosis` table, frame names (`NUI_*`,
  `NecrosisUI`...) and SavedVariables `NecrosisConfig`. So the original Necrosis
  addon must be disabled in game, and settings carry over from it.
- VersionCheck.lua still checks the upstream GitHub/CurseForge release, so it may
  announce upstream updates.

## Bottom banner (the purple bar)

- Art: `UI/BottomBanner/Images/base-{left2,left1,center,right1,right2}.png`
  (1024x256, 512x256, 512x256, 512x256, 1024x256, RGBA). Current art is a
  ChatGPT-generated redesign (gold filigree, lotus, crescents) made from
  `UI/BottomBanner/CHATGPT_PROMPT.md`. Built from two 2172x724 sources:
  - `Images/base-source-center.png`: the center design, scaled by 0.883 so it
    fits 256 px high (bar top at y=114, socket centered at x=1792).
  - `Images/base-source-repeat.png`: a plain bar that tiles seamlessly. It
    was stretched in three bands so its gold lines match the center art,
    then tiled outward from x=1792 (mirrored on the left).
  The center art is blended onto the tiled bar with a 160 px fade at both
  ends, faint alpha is cleaned up, and the strip is sliced into the pieces.
- The `.blp` files and `base-left1.psd` are the old SpartanUI-derived art,
  unused; the old PNGs are in git history (before the redesign commit).
- Loaded in two places, both with explicit `.png`: `UI/BottomBanner/Style.lua`
  and `Initialize_retail.lua` (CreateArtwork). Initialize.lua has an old copy
  pointing at a non-existent `NecrosisUI/Themes/Classic` path.
- Tint: `SetVertexColor(1, 1, 1)` (no tint) on all five pieces in both files; was
  `0.7, 0.3, 1.0` (purple). The art carries its own colors.
- Editing rules: keep exact power-of-two sizes and the alpha channel; piece
  edges must line up. WoW needs a full client restart for new texture files.

## Notes

- The ~40 French `*.md` files in the root are leftover AI session notes from
  upstream, not docs to trust.
- Not yet tested in game after the fork.
