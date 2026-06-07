# st - simple terminal, enhanced

A featured build of the [suckless terminal (st)](https://st.suckless.org/), rebuilt cleanly on upstream 0.9.3 as a transparent series of patches — one commit per feature. Includes Arabic RTL rendering, clickable URLs, smarter scrollback, undercurl, and more.

## Features

### Arabic / RTL

* **Proper bidi rendering** - lines containing Arabic or Hebrew are reordered to visual order at draw time via fribidi.
* **Arabic contextual shaping** - joined letterforms, lam-alif ligatures, and mirrored brackets in RTL runs.
* **Logical buffer** - selection still copies the original text; pure-RTL lines right-align like a native RTL terminal.

### Mouse

* **Ctrl+click a URL** - opens it in your default browser (`xdg-open`). Works on wrapped URLs and inside scrollback.
* **Ctrl+click a number/ID** - copies the token to the clipboard: ports, IPs, git hashes, container IDs, UUIDs.
* **Hand cursor on hover** - hold `ctrl` and the pointer turns into a hand over anything clickable.
* **Mouse wheel scrollback** - plain wheel scrolls history; inside TUI apps (vim, less, man) the wheel is passed to the app.

### Scrollback

* **Stays where you put it** - typing doesn't snap the view back to the bottom.
* **Return jumps to bottom** - press `Enter` while scrolled up to snap back to the prompt; the keypress is consumed.
* **Keyboard scrolling** - `alt-k/j`, `alt-↑/↓`, `alt-pageup/down`, or `shift-pageup/down`.

### Rendering

* **Undercurl** - SGR `4:1`-`4:5` underline styles (straight, double, curly, spiky, capped) with underline colors. Great with Neovim diagnostics.
* **Boxdraw** - pixel-perfect box-drawing and braille glyphs.
* **Ligatures** - via HarfBuzz.
* **Fallback fonts** - emoji and Arabic out of the box (`font2`).
* **Gruvbox theme**, Xresources overrides, opaque by default (compositor handles transparency; `*.alpha` is honored if set).

### Clipboard & keys

* **Copy/paste** - `alt-c` / `alt-v`, or `shift-insert`.
* **Zoom** - `alt+shift+k/j` or `alt+shift+↑/↓`.

## Patch series

Every feature is a standalone patch under [`patches/`](patches/) — numbered, documented, and verified to `git am` cleanly onto pristine upstream st 0.9.3 and build. Rebuild this terminal from scratch:

```
git clone https://git.suckless.org/st
cd st
git am /path/to/patches/*.diff
make
```

| # | Patch | Origin | Description |
|---|-------|--------|-------------|
| 01 | [arabic-rtl](patches/01-st-arabic-rtl.diff) | **original** | Bidi reordering + Arabic shaping (joined letterforms, lam-alif, mirrored brackets) via fribidi |
| 02 | [scrollback](patches/02-st-scrollback.diff) | suckless.org | Scrollback buffer with keyboard scrolling (prerequisite for 03-05) |
| 03 | [scrollback-usability](patches/03-st-scrollback-usability.diff) | **original** | View stays put while typing, plain-wheel scrolling, wheel passthrough to TUI apps |
| 04 | [scrollback-enterjump](patches/04-st-scrollback-enterjump.diff) | **original** | Enter snaps scrollback to the bottom without reaching the shell |
| 05 | [ctrlclick](patches/05-st-ctrlclick.diff) | **original** | Ctrl+click opens URLs, copies numeric/ID tokens; hand cursor on hover |
| 06 | [xresources](patches/06-st-xresources.diff) | suckless.org | Colors, font and more loadable from Xresources |
| 07 | [alpha](patches/07-st-alpha.diff) | suckless.org | Background opacity |
| 08 | [font2](patches/08-st-font2.diff) | suckless.org | Fallback font list (emoji, Arabic) |
| 09 | [boxdraw](patches/09-st-boxdraw.diff) | suckless.org | Pixel-perfect box-drawing and braille glyphs |
| 10 | [ligatures](patches/10-st-ligatures.diff) | suckless.org | HarfBuzz programming ligatures |
| 11 | [undercurl](patches/11-st-undercurl.diff) | suckless.org | SGR 4:1-4:5 underline styles + underline colors |
| 12 | [config](patches/12-st-config.diff) | optional | Gruvbox, monospace 10, opaque bg, alt-based bindings |

Community patches are adapted so the whole series applies cleanly in order; functionally unchanged. Details in [`patches/README.md`](patches/README.md).

## Installation

Dependencies: Xlib, Xft, fontconfig, freetype2, harfbuzz, fribidi headers.

```
git clone https://github.com/YoussefSakkaan/st
cd st
sudo make install
```

`$TERM` should be `st-256color` (the terminfo entry is installed by `make install`; it includes the `Su` capability for undercurl).

## Configuration

Compile-time via `config.def.h` (copied to `config.h` on first build — edit either and rebuild). At startup, Xresources can override colors, font, alpha and more: set e.g. `*.font`, `*.alpha`, `*.color0`... and run `xrdb` on the file.

To change the undercurl style edit `UNDERCURL_STYLE` in the config (`UNDERCURL_CURLY`, `UNDERCURL_SPIKY`, `UNDERCURL_CAPPED`).

## Using Undercurls in Neovim

```lua
vim.opt.termguicolors = true

vim.cmd([[
  highlight DiagnosticUnderlineError gui=undercurl guisp=#FF0000
  highlight DiagnosticUnderlineWarn gui=undercurl guisp=#FFAA00
  highlight DiagnosticUnderlineInfo gui=undercurl guisp=#00AAFF
  highlight DiagnosticUnderlineHint gui=undercurl guisp=#00FF00
]])
```

If undercurls don't show: check `infocmp -x st-256color | grep Su` and make sure `termguicolors` is set.

## Branches

* `master` - this build: upstream 0.9.3 + the patch series.
* `vanilla` - mirror of the patch series (same commits as master).
* `legacy` - the previous build this repo grew from (0.8.5-based, Luke Smith lineage).

## Credits

Original st by [suckless.org](https://st.suckless.org/). Community patches: scrollback, xresources, alpha, font2, boxdraw, ligatures, undercurl. Original patches in this repo: arabic-rtl, scrollback-usability, scrollback-enterjump, ctrlclick.

## License

See the LICENSE file for the terms of redistribution.
