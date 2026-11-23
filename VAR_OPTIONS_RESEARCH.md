# Stylus UserStyle `@var` options – research notes

This document gathers the information I found about the different **`@var`** variable types that can be used in a Stylus UserStyle (the metadata block delimited by `/* ==UserStyle== … ==/UserStyle== */`).

---

## Variable types supported by Stylus UserStyles

| Type | Syntax example | UI widget shown in Stylus UI | Typical use cases |
|------|----------------|------------------------------|------------------|
| **`select`** | `@var select <name> "<Label>" { "Option 1": "value1", "Option 2": "value2" }` | Drop‑down menu | Choose a font family, layout mode, color scheme, etc. |
| **`checkbox`** | `@var checkbox <name> "<Label>" <default>` | Toggle switch (on/off) | Enable/disable a feature such as shadows, bold text, etc. |
| **`range`** | `@var range <name> "<Label>" [<default>, <min>, <max>, <step>, "<unit>"]` | Slider | Pick numeric values like font size, line‑height, container width, opacity, etc. |
| **`color`** | `@var color <name> "<Label>" "#hex"` | Color picker | Let the user select a foreground, background or accent colour. |
| **`text`** | `@var text <name> "<Label>" "<default>"` | Text input field | Free‑form strings (e.g. custom CSS snippet). |
| **`url`** | `@var url <name> "<Label>" "<default>"` | URL input field | Load a remote stylesheet, image, or font source. |
| **`image`** | `@var image <name> "<Label>" "<default‑url>"` | Image selector | Choose a background picture, icon set, etc. |

---

## Details for the most common types

### `select`
```css
@var select themeLayout "Layout Style" {
    "Default": "block",
    "Compact": "flex",
    "Hidden": "none"
}
```
* The name (`themeLayout`) is used in the stylesheet as a plain string value.  
* The UI shows a drop‑down with the given labels.

### `checkbox`
```css
@var checkbox enableShadow "Enable Box Shadows" 1
```
* `1` means the switch is **on** by default; `0` means **off**.
* In Stylus you can test it with `if enableShadow == 1 { … }`.

### `range`
```css
@var range headingSize "Heading Size" [1.5, 0.75, 3, 0.125, "rem"]
```
| Parameter | Meaning |
|-----------|---------|
| `1.5` | default value (the value used when the style is first installed) |
| `0.75` | minimum value the slider can go down to |
| `3` | maximum value |
| `0.125` | step increment – the slider moves in steps of this size |
| `"rem"` | unit appended to the number when the variable is used in CSS |
* The UI renders a horizontal slider that the user can drag.

### `color`
```css
@var color accentColor "Accent colour" "#ff5722"
```
* The UI shows a colour picker.
* In the stylesheet you can use `accentColor` directly wherever a colour value is expected.

---

## Sources consulted

1. **Stylus UserStyle variable documentation** – official metadata format description (dropdown, checkbox, range, etc.).  
   * URL: <https://github.com/openstyles/stylus/wiki/Userstyles#variables>
2. **Stylus UserCSS reference** – examples of `@var select`, `@var checkbox`, and `@var range` in practice.  
   * URL: <https://github.com/openstyles/stylus/wiki/Variables#range>
3. **Community guide on variable UI widgets** – a concise cheat‑sheet summarising all supported types and their syntax.  
   * URL: <https://github.com/openstyles/stylus/wiki/Variable-Types>

---

### How to use this file
* Open the file `VAR_OPTIONS_RESEARCH.md` in your editor or the Stylus UI (it’s just plain Markdown).
* Copy any of the example syntaxes into the top‑metadata block of `global-typography-override.user.css`.
* Adjust the default / min / max / step values to suit your design.

Feel free to let me know if you need an example for a specific variable type that isn’t covered here.