# Agent Instructions — アナログ時計 (Analog Clock)

## Project Overview

This is a single-file analog clock (`index.html`) with Japanese kanji numerals, designed to be kept on screen while the user works. It runs by double-clicking the file in a browser — no build tools, servers, or dependencies required.

## Design Decisions

- **Single HTML file** — Everything (CSS, SVG, JS) is self-contained in `index.html`. Do not split into separate files unless the user explicitly asks.
- **No second hand** — Removed by user request for a calmer display.
- **Transparent clock face** — The clock has no background fill; it shares the page background color. Only the outer edge ring defines the clock boundary.
- **Japanese kanji numerals** — Hour labels use 一〜十二 instead of Arabic 1–12. Cardinal positions (十二, 三, 六, 九) are brighter.
- **Japanese tab title** — Browser tab shows アナログ時計.
- **Gray palette** — The user moved away from warm rust/copper tones to a neutral cool gray scheme. All colors are CSS custom properties in `:root`.
- **Hour ticks only** — Minute tick marks were removed. Only 12 hour-position ticks remain.
- **No date display** — Removed by user request.
- **No glow/aura effects** — Removed by user request for a cleaner look.

## Architecture

```
index.html
├── <style>        CSS custom properties, hand/tick/number styles
├── <svg>          Clock edge circle, tick group, number group, hand lines, center dot
└── <script>       Tick/number generation, hand rotation via requestAnimationFrame
```

### Key elements

| SVG Element       | ID / Class         | Purpose                      |
|-------------------|--------------------|------------------------------|
| `<circle>`        | (inline styled)    | Outer edge ring              |
| `<g id="ticks">`  | JS-generated lines | 12 hour tick marks           |
| `<g id="numbers">`| JS-generated text  | Hour numerals (一〜十二)     |
| `<line>`          | `#hand-hour`       | Hour hand (shorter, thicker) |
| `<line>`          | `#hand-minute`     | Minute hand (longer, thinner)|
| `<circle>`        | `.center-dot-outer`| Center cap                   |
| `<circle>`        | `.center-dot-inner`| Center cap inner dot         |

### Hand lengths

- **Hour hand**: 45 SVG units from center
- **Minute hand**: 78 SVG units from center
- SVG viewBox is `0 0 200 200`, center at `(100, 100)`

## Constraints

- Must remain a single HTML file (no external JS/CSS files)
- Must work offline (except Google Fonts, which falls back gracefully)
- No frameworks, no build steps
- Keep it minimal — the user wants a quiet, unobtrusive clock

## When modifying

1. Colors → edit the `:root` CSS custom properties
2. Hand lengths → edit the two numbers in the `update()` function (`45` and `78`)
3. Clock size → edit `.clock-wrapper` width/height (default `320px`)
4. Adding features → check with the user first; they prefer simplicity
