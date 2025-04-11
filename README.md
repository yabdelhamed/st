# Enhanced st - a simple terminal with Undercurl Support

This is an enhanced fork of [Luke Smith's build](https://github.com/LukeSmithxyz/st) of the [suckless terminal (st)](https://st.suckless.org/) with additional features that make it even better, particularly for Neovim users.

## My Enhancements

* **Undercurl support** - Proper undercurl rendering with customizable styles and colors
* **Color-coded diagnostics** - Excellent visual feedback for code errors and spell checking
* **Full integration with Neovim** - Works perfectly with Neovim's diagnostic and spellchecking features

## Original Features (from Luke's build)

* **URL functionality** - Follow URLs by pressing `alt-l` and copy them with `alt-y`
* **Clipboard integration** - Copy the output of commands with `alt-o`
* **Scrollback** with `alt-↑/↓` or `alt-pageup/down` or `shift` while scrolling with the mouse
* **Vim-bindings** - Scroll up/down in history with `alt-k` and `alt-j`
* **Font-size adjustment** - Zoom in/out with `alt+shift+j/k` or `alt+shift+up/down`
* **Clipboard handling** - Copy text with `alt-c`, paste with `alt-v` or `shift-insert`
* **Xresources/pywal integration** - Dynamic colors
* **Default gruvbox theme** - Pleasing aesthetics out of the box
* **Transparency support** - Adjustable via Xresources

## Patches Included

* **Undercurl patch** - Adds support for wavy underline rendering in 3 styles (spiky, curly, capped)
* Boxdraw
* Ligatures
* font2
* Latest version updates

## Undercurl Configuration

This build includes three different undercurl styles that can be configured in `config.h`:

1. **Spiky** (default) - Zigzag pattern that looks great for error highlighting
2. **Curly** - A more rounded wavy pattern
3. **Capped** - A hybrid style with straight lines and capped ends

To change styles, edit the `UNDERCURL_STYLE` definition in `config.h`:

```c
// Available styles
#define UNDERCURL_CURLY 0
#define UNDERCURL_SPIKY 1
#define UNDERCURL_CAPPED 2
// Active style
#define UNDERCURL_STYLE UNDERCURL_SPIKY
```

## Using Undercurls in Neovim

To take full advantage of the undercurl support in Neovim, add the following to your Neovim config:

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

You need to have xlib header files and libharfbuzz build files installed.

```
git clone https://github.com/yourusername/st
cd st
sudo make install
```

On OpenBSD, be sure to edit `config.mk` first and remove `-lrt` from the
`$LIBS` before compiling.

Be sure to have a composite manager (`xcompmgr`, `picom`, etc.) running if you want transparency.

## Configuration with Xresources

For many key variables, this build of `st` will look for X settings set in either `~/.Xdefaults` or `~/.Xresources`. You must run `xrdb` on one of these files to load the settings.

For example, you can define your desired fonts, transparency or colors:

```
*.font:	Liberation Mono:pixelsize=12:antialias=true:autohint=true;
*.alpha: 0.9
*.color0: #111
...
```

### Color Settings Priority

1. Default: Gruvbox colors
2. If Xresources colors are defined, those will be used
3. If `wal` has run in your session, its colors will take highest priority

## Troubleshooting Undercurls

If undercurls don't appear correctly:

1. Make sure your `$TERM` is set to `st-256color`
2. Check that your terminfo database is updated: `infocmp -x st-256color | grep Su`
3. Make sure your Neovim configuration includes `vim.opt.termguicolors = true`
4. Try different undercurl styles by changing `UNDERCURL_STYLE` in `config.h`

## Credits

- Original st by [suckless.org](https://st.suckless.org/)
- Luke Smith's [fork](https://github.com/LukeSmithxyz/st)
- Undercurl implementation inspired by various terminal emulators

## License

See the LICENSE file for the terms of redistribution.