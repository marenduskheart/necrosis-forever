# Necrosis Forever

Personal fork of the Necrosis warlock addon for the WoW `_forever_` client.
Only WoW Forever is supported. There is one toc, `NecrosisForever.toc`; every
Lua/XML file outside `libs/` is loaded by it (directly or through an `.xml`).
Other flavours' tocs and code, and upstream's AI session notes, were removed
in "chore: remove files not loaded by WoW Forever"; flavour suffixes
(`_retail`, `-Forever`) were dropped from file names in the commit after it.
Images and sounds were kept as is: code builds their paths by name.
License: GPL v2 or later (`_Copyright.txt`, `_GPL_V2.txt`). Keep both files; a
shared build must stay GPL with source included.

## Origin and upstream

- Forked from upstream `CKNOEP/Necrosis` `main` at `0443bd5` (tag `v160001-fix5`),
  which was identical to the installed CurseForge copy.
- Remote `upstream` = https://github.com/CKNOEP/Necrosis.git (push disabled).
- Remote `origin` = github.com/marenduskheart/necrosis-forever via the
  `github-maren` SSH alias (`git@github-maren:marenduskheart/necrosis-forever.git`).
  Commits are signed with the Maren Duskheart GPG key (repo-local config).
- Pick up upstream work: `git fetch upstream`, review with
  `git log --oneline main..upstream/main`, then `git merge upstream/main`
  (or `git cherry-pick <sha>` for single fixes).
- Expect conflicts on lines with texture paths: resolve by keeping upstream's
  change but with `NecrosisForever` in the path.
- Upstream's Forever code lives in their `*_retail.lua` / `*-Forever.lua`
  files. Map them to ours: Initialize_retail -> Initialize, Dialog_retail ->
  Dialog, Dialog_Forever -> DemonList, Spells-Forever -> Spells,
  Necrosis-Forever -> Necrosis, Timers/Functions_retail -> Timers/Functions,
  XML/{Attributes,Options-Sphere,Options-Buttons,Options-Misc,Panel}-Forever
  and XML/{Options-Menus,Options-Timers,XML}_retail -> same name without the
  suffix, UI/BottomBanner/Framework_retail -> UI/BottomBanner/EditModeLayout,
  NecrosisForever_Camelot.toc -> NecrosisForever.toc. Changes to their other
  flavours' files can be ignored.

## What the fork changed (commit "feat: fork Necrosis as Necrosis Forever")

- Folder and toc renamed `Necrosis*` -> `NecrosisForever*`; title
  "Necrosis Forever" (Interface 16001, X-Min-Interface-Forever 160001).
- All `Interface\AddOns\Necrosis\` paths (about 850) point at `NecrosisForever`.
- Folder-name lookups use `NecrosisForever`: `NECROSIS_ID` (Constants.lua, also
  the AceLocale app name), `AppName` and `GetMetadata` in Initialize.lua,
  `ADDON_NAME` in VersionCheck.lua.
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
- The banner is built by the code string inside `Initialize.lua`
  (CreateArtwork, run via loadstring 0.5 s after load), full width on
  UIParent, sized by the Options "BottomBanner Size" slider
  (`BottomBannerWidthScale`, `NUI:UpdateBottomBannerSize`).
- `UI/BottomBanner/Framework.lua` loads after `EditModeLayout.lua` and
  overrides its NUI:Show/Hide/ImportLayout..., and both create a frame named
  `NecrosisUI` (800x200); the later one wins.
- The old `UI/BottomBanner/Style.lua` (deleted) drew a second banner whose
  outer pieces were anchored to that 800-wide frame, so they came out squashed.
- The outer pieces have a fixed `SetSize(1024, 256)` and are anchored only to
  the middle pieces, so they run off the screen edge. (They used to be
  anchored to both screen edge and middle piece, which squashed 1024 px into
  the ~150 px left over; a SetTexCoord crop attempt didn't take effect.) If
  more than 1024 px is ever left beside the middle, a gap appears.
- Tint: `SetVertexColor(1, 1, 1)` (no tint) on all five pieces; was
  `0.7, 0.3, 1.0` (purple). The art carries its own colors.
- Editing rules: keep exact power-of-two sizes and the alpha channel; piece
  edges must line up. WoW needs a full client restart for new texture files.

## Notes

- Commits: Conventional Commits, GPG-signed.
- In-file code for other flavours (e.g. `C_EditMode` or Retail-only API
  branches) is still present; only whole unused files were removed.
