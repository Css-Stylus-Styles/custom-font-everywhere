# Global Typography Override

A Stylus user style that forces three custom fonts on every website:

- **SF Pro Text** for body text
- **Test Tiempos Text** for articles, quotes, italics
- **Geist Mono** for code, editors, terminals

## Install

1. Install the [Stylus](https://github.com/openstyles/stylus) browser extension.
2. Open `custom-font-everywhere.user.css` in Stylus (or paste its contents
   into a new style).
3. Make sure the three fonts above are installed on your system. If they
   aren't, the browser falls back to the generic `sans-serif`, `serif`,
   and `monospace` and results will vary by OS.

## Fonts

| Variable | Font | Used for |
|----------|------|----------|
| `--font-sans` | SF Pro Text | Default body text |
| `--font-serif` | Test Tiempos Text | Articles, quotes, `<em>`, `<i>` |
| `--font-mono` | Geist Mono | Code, editors, terminals |

## How it's structured

| Block | What it does |
|-------|--------------|
| Global Sans | Sets the default font on almost everything, with a long `:not()` chain to skip buttons, cards, labels, icons, and other UI primitives |
| Serif Surfaces | Overrides to serif for `<article>`, `<blockquote>`, `<q>`, `<em>`, `<i>`, and any element with `serif` in its class |
| Code Detection | Monospace for `<code>`, `<pre>`, `<kbd>`, and any class containing `code`, `mono`, `editor`, `terminal`, `syntax`, `prism`, `token`, `hljs`, etc. |
| Editor-specific | Extra coverage for Monaco, CodeMirror, Shiki, Prism |
| Icon revert | Two layers of protection: a targeted font-only revert on all SVGs, plus a nuclear revert for Lucide / Font Awesome / Material / MDI |

## Gotchas

- **SVG icons are fragile.** Never use `all: revert` on SVGs — it strips
  `width`, `height`, and `fill`, collapsing the icons to nothing. Only
  revert font properties.
- **Cascade order matters.** The serif block must come *after* the sans
  block, otherwise the sans rule wins and serif is a no-op. If you
  rearrange, double-check with the browser inspector.
- **`[class*="icon"]` is a broad hammer.** It matches anything containing
  the substring `icon` (e.g. `unicorn`, `iconic`). It's intentionally
  over-broad to catch most icon classes; the targeted revert block
  covers what it misses.
- **`!important` is everywhere.** Sites that use higher-specificity
  selectors or inline styles may still win. This is by design — the
  alternative is a specificity arms race.

## TODO

- [ ] Document the per-site Qwen AI Markdown override
- [ ] Google Flash lite info page (some icons don't work there)
- [ ] Try replacing the universal `body *:not(...)` selector with a
  `:where()` version to lower specificity and speed up matching
