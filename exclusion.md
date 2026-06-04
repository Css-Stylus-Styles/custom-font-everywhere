# Site Exclusions

This style is scoped to a single `@-moz-document` block with a regex
that determines which URLs the rest of the CSS applies to.

The current block:

```css
@-moz-document regexp("^(?!.*github\\.com).*$") {
    /* all the rules */
}
```

## How the regex works

The engine inside `@-moz-document` is JavaScript regex (the same one
V8 / SpiderMonkey use). It supports modern features including negative
lookaheads.

| Token | Meaning |
|-------|---------|
| `^` | Anchor: start of the URL string |
| `(?!...)` | Negative lookahead — fails the match if the inner pattern matches from the current position |
| `.*` | "Any character, zero or more times" (i.e. "anything") |
| `github` | Literal characters `github` |
| `\\.` | CSS-escaped backslash followed by `.` in the regex source; the engine sees `\.` which means a literal dot |
| `.*$` | The rest of the URL, anchored to the end |

Reading the whole thing in English: *"Match the entire URL only if it
does not contain `github.com`."*

The `\\.` looks like two backslashes in the file, but CSS string
escaping reduces it to one before the regex engine sees it. The engine
receives `\.` (escaped dot) and matches a literal dot. Without the
escape, `.` would mean "any character" and `github.com` would also
match `githubxcom`.

## Adding more exclusions

The negative lookahead is a plain regex alternation. To exclude more
sites, group them and join with `|`:

```css
@-moz-document regexp("^(?!.*(github\\.com|gitlab\\.com|bitbucket\\.org)).*$") {
```

| URL | Applies? |
|-----|----------|
| `https://github.com/...` | no |
| `https://gitlab.com/...` | no |
| `https://bitbucket.org/...` | no |
| `https://google.com` | yes |
| `https://gist.github.com` | no (subdomain also blocked) |

### Excluding only specific subdomains

If you want to keep `api.github.com` covered by your custom style but
let `github.com` itself be handled by the global override, you need a
positive match against the host instead of a substring check:

```css
@-moz-document regexp("^(?!https?://(www\\.)?github\\.com/).*") {
```

That only blocks URLs whose host is exactly `github.com` or
`www.github.com`, leaving `gist.github.com`, `api.github.com`, etc.
free to receive the override.

### Excluding a path prefix

To exclude only a specific section of a site (e.g. the editor but not
the rest of the site):

```css
@-moz-document regexp("^(?!.*github\\.com/.*/edit/).*$") {
```

That blocks anything under a `*/edit/*` path on `github.com` while
leaving the rest of the site untouched.

## Testing your regex

Before pushing, validate the pattern in isolation. Two good options:

1. **[regex101.com](https://regex101.com/)** — pick the "ECMAScript
   (JavaScript)" flavor, paste the pattern (with `\\.` reduced to
   `\.`), and feed it sample URLs. The "matches / not matches" panel
   shows exactly what the engine sees.
2. **Stylus "Applies to" field** — paste the regex there temporarily
   and check it against a few sites. Stylus shows whether a URL
   matches live in the editor.

## Gotchas

- **Forgetting the `\\` in CSS.** The CSS source has `\\.` (two
  backslashes); the regex engine sees `\.` (one backslash + literal
  dot). If you write `\.` directly in the CSS file, the engine
  receives just `.` and matches any character.
- **Forgetting the anchors `^` and `$`.** Without them, the regex can
  match a substring of the URL. The behavior is usually the same here,
  but anchoring makes intent explicit and protects against future
  changes.
- **Blocking more than you meant.** The substring `github.com` also
  appears in `gist.github.com`, `api.github.com`, and any URL with
  `?ref=github.com`. Use the host-only variant above if that matters.
- **Per-site overrides don't compose.** If you have two Stylus styles
  both targeting the same site, the later one (by import order in
  Stylus's "Manage" page) wins. Order them deliberately.

## Reference

- [@-moz-document on MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/@document)
- [JavaScript RegExp on MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)
