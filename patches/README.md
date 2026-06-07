# Patches

Standalone patches against [vanilla st](https://git.suckless.org/st) (0.9.3, commit `688f70a`). Each one is a single commit on the [`vanilla`](../../tree/vanilla) branch — apply them in the order below to rebuild the featured terminal from upstream:

```
git clone https://git.suckless.org/st
cd st
git am st-arabic-rtl-0.9.3.diff
patch -p1 < st-scrollback-0.9.2.diff
git am st-scrollback-usability-0.9.3.diff
git am st-scrollback-enterjump-0.9.3.diff
git am st-ctrlclick-0.9.3.diff
make
```

## st-arabic-rtl-0.9.3.diff

Bidi reordering and Arabic contextual shaping (joined letterforms, lam-alif ligatures, mirrored brackets) via fribidi, applied at draw time. The terminal buffer keeps logical order so selection copies the original text. Applies to vanilla st; needs fribidi.

## st-scrollback-0.9.2.diff

The official scrollback patch from [st.suckless.org](https://st.suckless.org/patches/scrollback/), unchanged. Included here as the prerequisite for the three patches below.

## st-scrollback-usability-0.9.3.diff

Requires st-scrollback. Three fixes:

- typing no longer snaps the view back to the bottom
- plain mouse wheel scrolls history (no shift needed)
- on the alt screen the wheel sends arrow keys, so TUI apps (vim, less, man) scroll themselves instead of revealing stale history

## st-scrollback-enterjump-0.9.3.diff

Requires st-scrollback. Pressing Enter while scrolled up snaps the view back to the bottom; the keypress is consumed so no empty command reaches the shell.

## st-ctrlclick-0.9.3.diff

Requires st-scrollback (uses its TLINE macro). Ctrl+click on:

- a URL (`://` or `www.`) opens it with xdg-open
- any token containing a digit (ports, IPs, git hashes, container IDs, UUIDs) copies it to the clipboard

Holding Ctrl shows a hand cursor over clickable tokens. Tokens follow wrapped lines and work inside scrollback.
