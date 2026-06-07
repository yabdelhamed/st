# Patches

The complete patch series against [vanilla st](https://git.suckless.org/st) (0.9.3, commit `688f70a`). Applying all eleven in order rebuilds the full featured terminal from upstream. Each patch is one commit on the [`vanilla`](../../tree/vanilla) branch.

```
git clone https://git.suckless.org/st
cd st
git am /path/to/patches/*.diff   # applies 01..11 in order
make
```

The series is verified: every patch applies with `git am` on a pristine upstream checkout and the result builds cleanly.

## Original patches (not on st.suckless.org)

### 01-st-arabic-rtl.diff

Bidi reordering and Arabic contextual shaping (joined letterforms, lam-alif ligatures, mirrored brackets) via fribidi, applied at draw time. The terminal buffer keeps logical order so selection copies the original text. Applies to bare vanilla; needs fribidi.

### 03-st-scrollback-usability.diff

Requires 02. Three fixes: typing no longer snaps the view to the bottom, plain mouse wheel scrolls history (no shift needed), and on the alt screen the wheel sends arrow keys so TUI apps (vim, less, man) scroll themselves.

### 04-st-scrollback-enterjump.diff

Requires 02. Pressing Enter while scrolled up snaps the view back to the bottom; the keypress is consumed so no empty command reaches the shell.

### 05-st-ctrlclick.diff

Requires 02 (uses its TLINE macro). Ctrl+click opens URLs (`://`, `www.`) with xdg-open and copies digit-bearing tokens (ports, IPs, git hashes, container IDs, UUIDs) to the clipboard. Hand cursor while Ctrl-hovering clickable tokens. Tokens follow wrapped lines and work inside scrollback.

## Community patches (adapted to apply on this series)

Sourced from [st.suckless.org/patches](https://st.suckless.org/patches/); conflicting hunks were re-anchored so the whole series applies cleanly in order. Functionally unchanged.

### 02-st-scrollback.diff

st-scrollback-0.9.2, verbatim. Scrollback buffer with keyboard scrolling.

### 06-st-xresources.diff

st-xresources-20230320. Colors, font, and other settings loadable from Xresources at startup.

### 07-st-alpha.diff

st-alpha-20240814. Background opacity via a compositor.

### 08-st-font2.diff

st-font2-0.8.5. Fallback font list (emoji, Arabic, etc.) in config.

### 09-st-boxdraw.diff

st-boxdraw_v2-0.8.5. Pixel-perfect box-drawing and braille glyphs.

### 10-st-ligatures.diff

st-ligatures (hbtransform-on-specs variant). HarfBuzz shaping for programming ligatures; skips boxdraw cells.

### 11-st-undercurl.diff

st-undercurl-0.9-20240103. SGR `4:1`-`4:5` underline styles (straight, double, curly, spiky, capped) and 58/59 underline colors, plus the `Su` terminfo capability.
