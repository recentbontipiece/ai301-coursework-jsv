# Replacing a Broken White Square with the GAIA Logo in the macOS Menu Bar

## Overview

GAIA is AMD's open-source local AI agent framework (`amd/gaia`). In issue `amd/gaia#5281`, the macOS menu-bar tray icon appeared as a solid white square because the template PNG assets contained flat opaque white pixels. To resolve this, I replaced the broken icons with the transparent GAIA head glyph, removed unused placeholder files, and added automated asset tests to prevent flat placeholders in the future. After testing the fix locally and sharing before-and-after evidence in [PR #6989](https://github.com/amd/gaia/pull/6989), the maintainer (`kovtcharov-amd`) merged the changes on November 9, 2026.

## What happened

I identified this bug using `issue-scout`, an automated tool I developed to triage open repositories, after refining its rubric to filter out issues with recent non-maintainer claims. Inspecting `main` (`03b04eea`) revealed why the tray icon failed: macOS template images (`tray-iconTemplate.png` and `tray-iconTemplate@2x.png` in `src/gaia/apps/webui/assets/`) rely solely on the alpha channel, and both files consisted entirely of opaque white (`ffffffff`) pixels.

```text
FAIL tray-iconTemplate.png: corner (0,0) is opaque — expected the round logo on a transparent background, not a filled square
tray-iconTemplate.png 16x16 distinct RGBA values: [ 'ffffffff' ]
tray-iconTemplate@2x.png 32x32 distinct RGBA values: [ 'ffffffff' ]
```

Before implementing the fix, I wrote a regression test (`assertTrayTemplatePng` in `tests/electron/_helpers/tray-icon-png.cjs`) to catch missing macOS template checks, verifying dimensions, transparent corners, black RGB values, and alpha coverage ratios. Running Jest confirmed the test failed on the old icons (`4d093a2`) and passed once the redrawn assets were committed (`a746401`).

```js
function assertTrayTemplatePng(buf, size, label) {
  // ...
  if (pixels[i] !== 0 || pixels[i + 1] !== 0 || pixels[i + 2] !== 0) {
    throw new Error(`${label}: visible pixel ${i / 4} is not black — ...`);
  }
  // ...
  const coverage = visible / (width * height);
  if (coverage < 0.2 || coverage > 0.9) { /* placeholder, not the logo */ }
}
```

```text
# test commit 4d093a2, with the old icons
Tests:       2 failed, 139 passed, 141 total
# after the icon commit a746401
Tests:       141 passed, 141 total
```

To fix the artwork, I extracted the GAIA robot head from `tray-icon@2x.png`, converted it to a pure black (`0,0,0`) transparent-background glyph at 16×16 px and 32×32 px, refined the visor and eye details, trimmed edge noise, and deleted unused placeholder files (`tray-icon-active.png` and `@2x` in `bc94fe5`).

```text
tray-iconTemplate.png 16x16 visible RGB: [ '0,0,0' ] | distinct alpha levels: 82 | coverage: 56%
tray-iconTemplate@2x.png 32x32 visible RGB: [ '0,0,0' ] | distinct alpha levels: 156 | coverage: 48%
```

My local pre-submission check (`ship-gate`) initially blocked the PR for missing real-world UI screenshots. After launching the Electron app locally on macOS 26.2 from my feature branch (`fix/5281-macos-tray-template-icon`), macOS properly tinted the GAIA head white against the dark menu bar. 

```text
ship-gate: STOP. your own pr-precheck rejected fix/5281-macos-tray-template-icon: verdict reject.
  - template-asks [fail]: Template Evidence asks for real-world proof matched to the changed surface; the description admits 'I haven't captured it in the running app' ...
```

| Before (`main`) | After (my branch) |
|---|---|
| ![menu bar before: a plain white square](https://raw.githubusercontent.com/recentbontipiece/gaia/fix/5281-macos-tray-template-icon-evidence/menubar-before-8x.png) | ![menu bar after: the GAIA head](https://raw.githubusercontent.com/recentbontipiece/gaia/fix/5281-macos-tray-template-icon-evidence/menubar-after-8x.png) |

After opening [PR #6989](https://github.com/amd/gaia/pull/6989), GAIA's automated reviewer verified the alpha channel and logo geometry, and maintainer `kovtcharov-amd` merged the PR into `main` (closing `#5281`) roughly five hours later.

> "Verdict: Approve … Nothing blocks the merge."
>
> "The 32 px icon is the logo, not a blob. I decoded its transparency and printed it as text: a head outline with the visor cut out and the eyes kept"
>
> (gaia's automated PR reviewer, 2026-11-09)

## Limitations and what's next

- **Single menu-bar appearance:** Verified against a dark menu-bar wallpaper, pending side-by-side Light Mode verification.
- **Local test hang and sizing:** The local `pytest` suite hung waiting on a localhost socket (relying on CI instead), and I retained standard `16×16` / `32×32` dimensions in `ICON_SPECS.darwin`.

Next, I plan to update `issue-scout` to flag environment/socket dependencies and enhance `ship-gate` to enforce dual light/dark mode screenshots for macOS template updates.
