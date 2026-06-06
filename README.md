# st - simple terminal, enhanced

A customized build of the [suckless terminal (st)](https://st.suckless.org/) with undercurl rendering, clickable URLs, smarter scrollback, and quality-of-life fixes — particularly nice for Neovim users.

## Features

### Mouse

* **Ctrl+click a URL** - opens it in your default browser (`xdg-open`). Works on wrapped URLs and inside scrollback.
* **Ctrl+click a number/ID** - copies the token to the clipboard: ports, IPs, git hashes, container IDs, UUIDs. Paste with `ctrl-v` elsewhere or `alt-v` in st.
* **Hand cursor on hover** - hold `ctrl` and the pointer turns into a hand over anything clickable.
* **Mouse wheel scrollback** - scroll up through history; inside TUI apps (vim, less, man) the wheel is passed to the app instead.

### Scrollback

* **Stays where you put it** - typing no longer snaps the view back to the bottom.
* **Return jumps to bottom** - press `Enter` while scrolled up to snap back to the prompt; the keypress is consumed, so no stray empty command.
* **Keyboard scrolling** - `alt-↑/↓`, `alt-pageup/down`, `alt-k/j` (vim style), or `shift` + mouse wheel.
* **Selection works in history** - select and copy text anywhere in scrollback; it copies what you see.

### Rendering

* **Undercurl support** - wavy underline rendering in 3 styles (spiky, curly, capped) with per-color support; great with Neovim diagnostics and spell checking.
* **Boxdraw** - pixel-perfect box-drawing and powerline glyphs.
* **Ligatures** - via HarfBuzz.
* **font2** - fallback fonts, including emoji.
* **Default gruvbox theme** with Xresources/pywal overrides.

### Clipboard & misc

* **URL helpers** - follow URLs on screen with `alt-l`, copy them with `alt-y`.
* **Copy command output** with `alt-o`.
* **Copy/paste** - `alt-c` / `alt-v`, or `shift-insert`.
* **Font-size adjustment** - zoom with `alt+shift+j/k` or `alt+shift+up/down`.
* **Opaque by default** - `alpha = 1.0`; let your compositor (picom) handle window transparency, or set `*.alpha` in Xresources for built-in translucency.

## Undercurl Configuration

Three undercurl styles, configured in `config.h`:

1. **Spiky** - zigzag pattern, great for error highlighting
2. **Curly** - a more rounded wavy pattern
3. **Capped** - a hybrid style with straight lines and capped ends

```c
// Available styles
#define UNDERCURL_CURLY 0
#define UNDERCURL_SPIKY 1
#define UNDERCURL_CAPPED 2
// Active style
#define UNDERCURL_STYLE UNDERCURL_CAPPED
```

## Using Undercurls in Neovim

```lua
vim.opt.termguicolors = true

-- For spell checking
vim.opt.spell = true
vim.opt.spelllang = 'en_us'
vim.cmd([[
  highlight SpellBad gui=undercurl guisp=#FF0000
  highlight SpellCap gui=undercurl guisp=#0000FF
  highlight SpellRare gui=undercurl guisp=#FF00FF
  highlight SpellLocal gui=undercurl guisp=#00FF00
]])

-- For LSP diagnostics
vim.diagnostic.config({
  underline = true,
  virtual_text = true,
  signs = true,
  update_in_insert = false,
  severity_sort = true,
})

vim.cmd([[
  highlight DiagnosticUnderlineError gui=undercurl guisp=#FF0000
  highlight DiagnosticUnderlineWarn gui=undercurl guisp=#FFAA00
  highlight DiagnosticUnderlineInfo gui=undercurl guisp=#00AAFF
  highlight DiagnosticUnderlineHint gui=undercurl guisp=#00FF00
]])
```

## Installation

You need xlib header files, harfbuzz, and fribidi build files installed.

```
git clone https://github.com/YoussefSakkaan/st
cd st
sudo make install
```

On OpenBSD, edit `config.mk` first and remove `-lrt` from `$LIBS` before compiling.

If you want window transparency, run a composite manager (`picom`, `xcompmgr`, etc.).

## Configuration with Xresources

For many key variables, this build looks for X settings in either `~/.Xdefaults` or `~/.Xresources` (or wherever your setup loads them from). Run `xrdb` on the file to load the settings.

```
*.font: monospace:pixelsize=10:antialias=true:autohint=true
*.alpha: 1.0
*.color0: #111
...
```

### Color Settings Priority

1. Default: gruvbox colors
2. If Xresources colors are defined, those are used
3. If `wal` has run in your session, its colors take highest priority

## Troubleshooting Undercurls

If undercurls don't appear correctly:

1. Make sure your `$TERM` is set to `st-256color`
2. Check that your terminfo database is updated: `infocmp -x st-256color | grep Su`
3. Make sure your Neovim configuration includes `vim.opt.termguicolors = true`
4. Try different undercurl styles by changing `UNDERCURL_STYLE` in `config.h`

## Credits

Based on [st from suckless.org](https://st.suckless.org/) and community patches (scrollback, boxdraw, ligatures, alpha, undercurl).

## License

See the LICENSE file for the terms of redistribution.
