# Global Typography Override

A Stylus user style that replaces fonts across all websites.

## Font Variables

| Variable | Font | Purpose |
|----------|------|---------|
| `--font-sans` | SF Pro Text | Default text |
| `--font-serif` | Test Tiempos Text | Articles, quotes, emphasized text |
| `--font-mono` | Geist Mono | Code blocks, editors, terminals |

## Sections

**Global Sans Typography** (lines 27-56)
Applies `--font-sans` to everything except icons (Material Icons, Font Awesome, Phosphor, Lucide, Codicons, Octicons, Devicon).

**Serif Surfaces** (lines 62-70)
Applies `--font-serif` to `<article>`, `<blockquote>`, `<em>`, `<i>`, and elements with `serif` class.

**Aggressive Code Detection** (lines 76-138)
Targets every conceivable code element and class pattern: `<code>`, `<pre>`, `<kbd>`, `[class*="code"]`, `[class*="mono"]`, `[class*="editor"]`, `[class*="terminal"]`, etc.

**Monaco / VSCode Web** (lines 144-155)
Specific overrides for Monaco editor web instances.

**CodeMirror** (lines 161-168)
Overrides for CodeMirror-based editors.

**GitHub / Shiki / Prism** (lines 174-183)
Syntax highlighting containers.

**Rendering Improvements** (lines 194-198)
Enables `optimizeLegibility` and font smoothing.

## Notes

- Uses `regexp(".*")` to match all domains
- `!important` is used throughout to override site styles
- Icons are explicitly excluded to prevent breakage

## TODO

- [ ] Add edge cases / observations here