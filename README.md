# dtslides — Quarto Reveal.js Themes

A Quarto extension providing two reveal.js presentation themes with a shared layout architecture.

| Theme        | Format string            | Palette                                                  |
|--------------|--------------------------|----------------------------------------------------------|
| **Academic** | `dtslides-revealjs`      | Navy / slate — clean, institution-neutral                |
| **UNM**      | `dtslides-unm-revealjs`  | Cherry / turquoise — University of New Mexico branding   |

Requires Quarto >= 1.6.0.

## Installing

```bash
quarto add Data-Wise/dtslides
```

This installs both themes under `_extensions/`. Check the directory into version control.

## Updating

To update to the latest version:

```bash
quarto update Data-Wise/dtslides
```

This replaces the extension files under `_extensions/` while preserving your `.qmd` files and any custom SCSS overrides.

## Upgrading from unm-revealjs

If you previously used the `unm-revealjs` extension (v1.0.x), follow these steps:

1. Remove the old extension directory:

   ```bash
   rm -rf _extensions/unm
   ```

2. Install the new extension:

   ```bash
   quarto add Data-Wise/dtslides
   ```

3. Update your YAML front matter:

   ```yaml
   # Before
   format:
     unm-revealjs:
       slide-number: c/t

   # After
   format:
     dtslides-unm-revealjs:
       slide-number: c/t
   ```

All CSS classes, callouts, and slide options work the same way — only the format string changed.

## Using

### Academic theme

```yaml
format:
  dtslides-revealjs:
    slide-number: c/t
```

### UNM theme

```yaml
format:
  dtslides-unm-revealjs:
    slide-number: c/t
```

## Features

Both themes share a 760+ line layout partial (`_shared.scss`) providing consistent slide components regardless of which color palette you choose. See [REFERENCE.md](REFERENCE.md) for the full class list with usage examples.

### Slide types

Apply as a class on any slide (`## Title {.small-slide}`):

| Class                | Effect                                       |
|----------------------|----------------------------------------------|
| `.small-slide`       | Reduced font size for dense content          |
| `.tiny-slide`        | Even smaller — useful for reference tables   |
| `.quote-slide`       | Centered text with styled blockquote         |
| `.image-slide`       | Minimal chrome, image-focused                |
| `.section-slide`     | Large centered heading for section dividers  |
| `.slide-code`        | Optimized for code-heavy slides              |
| `.large-image-slide` | Full-height image display                    |
| `.scrollable`        | Enable vertical scrolling on a slide         |

### Emphasis classes

Inline spans for highlighting content (`[text]{.alert}`):

| Class        | Effect                                                      |
|--------------|-------------------------------------------------------------|
| `.alert`     | Bold text in the theme's alert color with tinted background |
| `.fg`        | Bold text in the theme's accent color                       |
| `.bg`        | White text on the theme's accent background                 |
| `.highlight` | Yellow/gold highlighted text                                |
| `.note`      | Italic text with tinted background                          |
| `.warning`   | Bold warning-colored text with tinted background            |

### Custom callouts

Div-based callouts with auto-generated titles (`:::{.callout-example} ... :::`):

| Class               | Icon     | Color (Academic / UNM) |
|---------------------|----------|------------------------|
| `.callout-example`  | Example  | Green / Green          |
| `.callout-question` | Question | Primary / Cherry       |
| `.callout-insight`  | Insight  | Blue / Turquoise       |

Standard Quarto callouts (`.callout-note`, `.callout-warning`, etc.) are also styled.

### Font sizing utilities

Apply to a slide or a div to scale text:

| Class       | Scale  |
|-------------|--------|
| `.larger`   | 1.50em |
| `.largest`  | 1.20em |
| `.large`    | 1.25em |
| `.small`    | 0.80em |
| `.smaller`  | 0.64em |
| `.smallest` | 0.51em |

Code blocks inside these containers scale proportionally.

### Other utilities

| Class                                          | Purpose                            |
|------------------------------------------------|------------------------------------|
| `.left`, `.center`, `.right`                   | Text alignment                     |
| `.v-center`, `.v-top`, `.v-bottom`             | Vertical alignment (grid-based)    |
| `.button`                                      | Styled link button with play icon  |
| `.badge`, `.badge-primary`, `.badge-secondary` | Inline label badges                |
| `.citation`                                    | Smaller, muted citation text (0.8em) |
| `.references`                                  | Compact reference list with auto-shrink for 6+/11+ entries |

### Code blocks

- Syntax-highlighted source code with Fira Code font
- R console output gets a distinct lighter background with an accent-colored left border
- `code-copy: false` by default (can override in YAML)

### Tables

- Striped rows, uppercase headers, no hover effect
- DataTable component sizing included for interactive tables

## Examples

- [example-academic.qmd](example-academic.qmd) — Academic theme: callouts, emphasis, code, scrollable slides
- [example.qmd](example.qmd) — UNM theme: core features showcase
- [example-long.qmd](example-long.qmd) — UNM theme: extended demo with more slide types
- [template.qmd](template.qmd) — UNM theme: minimal starting template

## Architecture

Both themes import a shared `_shared.scss` partial that contains all color-agnostic layout rules. Each theme defines only its color variables (~100 lines), then `@import '_shared'` pulls in the full layout.

```text
_extensions/
├── dtslides/           # Academic theme
│   └── styles/
│       ├── academic.scss    ← color variables (navy/slate)
│       └── _shared.scss     ← shared layout (765 lines)
└── dtslides-unm/       # UNM theme
    └── styles/
        ├── custom.scss      ← color variables (cherry/turquoise)
        └── _shared.scss     ← symlink → dtslides/styles/_shared.scss
```

> **Note for contributors:** The UNM theme's `_shared.scss` is a symlink to the academic theme's canonical copy. Edits to `_extensions/dtslides/styles/_shared.scss` automatically apply to both themes.

## Acknowledgments

Inspired by [quarto-revealjs-clean](https://github.com/grantmcdermott/quarto-revealjs-clean) by Grant McDermott.

## License

[MIT](LICENSE)

## Contributing

Issues and pull requests welcome at [Data-Wise/dtslides](https://github.com/Data-Wise/dtslides).
