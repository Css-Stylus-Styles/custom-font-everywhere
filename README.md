# Global Typography Override

A Stylus user style that replaces fonts across all websites.

## Font Variables

| Variable | Font | Purpose |
|----------|------|---------|
| `--font-sans` | SF Pro Text | Default body text |
| `--font-serif` | Test Tiempos Text | Articles, quotes, emphasized text |
| `--font-mono` | Geist Mono | Code blocks, editors, terminals |

> The named fonts must be installed on your system. If they aren't, the
> browser falls back to the generic family (`sans-serif`, `serif`,
> `monospace`) and results will vary by OS.

## Sections

| Section | Purpose |
|---------|---------|
| Root Variables | Defines the three font custom properties |
| Global Sans Typography | Applies `--font-sans` to everything, with broad exclusions for buttons/cards/icons/UI primitives |
| Placeholders | Serif for textareas and `[role="textbox"]` elements |
| Serif Surfaces | Serif for `<article>`, `<blockquote>`, `<q>`, `<em>`, `<i>` |
| Aggressive Code Detection | Monospace for `<code>`, `<pre>`, `[class*="code"]`, `[class*="mono"]`, `[class*="editor"]`, etc. |
| Monaco / VSCode Web | Monospace inside Monaco editor web instances |
| CodeMirror | Monospace inside CodeMirror-based editors |
| GitHub / Shiki / Prism | Monospace inside syntax highlight containers |
| Prevent Icon Breakage | Targeted font revert on SVGs (does not collapse icons) |
| Restore icons | Nuclear revert for common icon systems (Lucide, Font Awesome, Material, MDI) |
| Rendering Improvements | `optimizeLegibility`, antialiased font smoothing |

## Notes

- Uses `regexp(".*")` to match all domains
- `!important` is used throughout to override site styles
- Icon classes are excluded from the sans rule; a separate revert block
  catches icon systems whose classes don't contain `"icon"` (Lucide,
  Font Awesome `fa-*`, Material, MDI, etc.)
- **Do not** use `all: revert` on SVGs — it strips `width`/`height`/`fill`
  and collapses the icons. Only revert font properties.
- Serif surfaces are intentionally narrow: paragraphs and spans inherit
  from their parent, so text inside `<article>` still renders in serif.

## Changelog

### 3.1.4
- **Fixed:** `svg, svg * { all: revert }` was collapsing SVG icons on
  Qwen AI, AI Studio, and similar sites, leaving large empty bands at
  the top of the page. Replaced with targeted `font-family`/`font-weight`/
  `font-size` revert.
- **Fixed:** `strong` and `strong *` were being forced to monospace,
  which broke bold text everywhere. Removed from the code block.
- **Fixed:** Serif rule was overriding the sans rule for `<p>`, `<li>`,
  `<ol>`, `<ul>`, `<span>`, `<div>` (cascade order), so most text ended
  up in serif on most sites. Serif now only applies to `article`,
  `blockquote`, `q`, `em`, `i`, `.serif`, `[class*="serif"]`.
- **Fixed:** `<i>` elements with icon classes (Font Awesome) were
  matching the serif rule directly. Added icon exclusions to the `i`
  selector in the serif block.
- Fixed `[class* ="mat-mdc-button"]` whitespace and removed the
  `:not([role=""])` debug artifact.

## TODO

- [ ] Add Qwen AI Markdown to the documented per-site overrides
- [ ] Google Flash lite info page (some icons don't work there)
- [ ] Consider replacing the universal `body *:not(...)` selector with
  a `:where()`-based version to lower specificity and reduce
  per-element matching cost
