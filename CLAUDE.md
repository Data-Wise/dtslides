# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**dtslides** is a Quarto extension providing two reveal.js presentation themes with shared layout architecture.

- **Author:** Davood Tofighi
- **Version:** 1.1.0
- **Quarto Requirement:** >= 1.6.0
- **GitHub:** Data-Wise/dtslides

### Themes

| Format string | Theme | Palette |
|---------------|-------|---------|
| `dtslides-revealjs` | Academic (generic) | Navy/slate |
| `dtslides-unm-revealjs` | UNM-branded | Cherry/turquoise |

## Common Commands

```bash
# Render presentations
quarto render template.qmd           # UNM theme
quarto render example.qmd            # UNM theme
quarto render example-long.qmd       # UNM theme
quarto render example-academic.qmd   # Academic theme

# Preview with live reload
quarto preview template.qmd

# Install extension in another project
quarto add Data-Wise/dtslides/_extensions/dtslides       # Academic
quarto add Data-Wise/dtslides/_extensions/dtslides-unm   # UNM
```

There is no build step, test suite, or linter. Quarto handles SCSS compilation automatically during render.

## Architecture

Two extension directories share a common `_shared.scss` layout partial:

```
_extensions/
├── dtslides/                    # Academic theme (format: dtslides-revealjs)
│   ├── _extension.yml
│   └── styles/
│       ├── academic.scss        # Color variables (navy/slate palette)
│       └── _shared.scss         # Shared layout partial (712 lines)
│
└── dtslides-unm/                # UNM theme (format: dtslides-unm-revealjs)
    ├── _extension.yml
    ├── unm-revealjs.js          # Reveal.js plugin (shuffle keybinding on 'T')
    ├── images/                  # UNM logos, title slide background
    └── styles/
        ├── custom.scss          # Color variables (cherry/turquoise palette)
        └── _shared.scss         # SAME file — must be manually synced
```

### Theme Chain

Both `_extension.yml` files define: `theme: [simple, "styles/<theme>.scss"]`

Quarto's built-in `simple` theme loads first, then the theme SCSS overrides it. Each theme SCSS sets color variables in `/*-- scss:defaults --*/`, then `@import '_shared'` in `/*-- scss:rules --*/`.

### _shared.scss Structure (~712 lines)

The shared partial contains all color-agnostic layout rules:

1. **Imports** — Google Fonts (Roboto, Fira Code, Lato), Font Awesome 6.7.2
2. **Base Styles** — Body, `.reveal` container, image defaults, overflow fix
3. **Code Blocks** — `pre`, `pre code`, R output styling (`.cell-output-stdout`)
4. **Tables** — Striped rows, no hover effect
5. **Slide Types** — `.small-slide`, `.tiny-slide`, `.quote-slide`, `.image-slide`, `.section-slide`, `.slide-code`
6. **Emphasis Classes** — `.alert`, `.fg`, `.bg`, `.highlight`, `.note`, `.warning`
7. **Callouts** — `.callout-example`, `.callout-question`, `.callout-insight`
8. **Title Slide** — `#title-slide` layout
9. **Logo & Footer** — `.slide-logo`, `.slide-number`, `.slide-menu-button`
10. **Utilities** — Font sizing (`.largest` to `.smallest`), alignment, badges, datatables, panel-tabset

### IMPORTANT: Syncing _shared.scss

Both extension directories contain their own copy of `_shared.scss`. After editing one, copy it to the other:

```bash
cp _extensions/dtslides/styles/_shared.scss _extensions/dtslides-unm/styles/_shared.scss
```

This is required because Quarto SCSS `@import` resolves relative to the importing file's directory, and two separate extension directories cannot share a single partial.

## Key Files for Common Tasks

| Task | File(s) |
|------|---------|
| Change academic colors | `_extensions/dtslides/styles/academic.scss` |
| Change UNM colors | `_extensions/dtslides-unm/styles/custom.scss` |
| Change shared layout | `_extensions/dtslides/styles/_shared.scss` (then sync) |
| Modify format defaults | `_extensions/dtslides/_extension.yml` or `_extensions/dtslides-unm/_extension.yml` |
| Update UNM logos | `_extensions/dtslides-unm/images/` |
| Test academic theme | `example-academic.qmd` |
| Test UNM theme | `template.qmd`, `example.qmd`, `example-long.qmd` |

## Academic Theme Colors

| Variable | Hex | Usage |
|----------|-----|-------|
| `$primary` | #1B365D | Headings (h1, h2), title |
| `$accent` | #4A6FA5 | Links, buttons, h4 |
| `$secondary` | #6C757D | h3, borders |
| `$highlight-color` | #E8C547 | Highlights |
| `$success` | #2E7D32 | Example callouts |
| `$warning-color` | #E65100 | Warning/caution |
| `$info` | #0277BD | Insight callouts |

## UNM Theme Colors

| Variable | Hex | Usage |
|----------|-----|-------|
| `$unm-cherry` | #BA0C2F | Primary headings, alerts |
| `$unm-turquoise` | #007A86 | Links, accents, `.fg` |
| `$unm-silver` | #A7A8AA | Table headers |
| `$unm-lobo-gray` | #63666A | h3, borders |
| `$unm-high-noon` | #FFC600 | Highlights |
| `$unm-sandia-sunset` | #ED8B00 | Table stripes, `.bg` |

## Extension Usage

```yaml
# Academic theme
format:
  dtslides-revealjs:
    slide-number: c/t

# UNM theme
format:
  dtslides-unm-revealjs:
    slide-number: c/t
```
