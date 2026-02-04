# dtslides — Future Enhancements Proposal

**Generated:** 2026-02-03
**Context:** Post-v1.1.0 UI improvements, all 12 proposal items complete
**Branch:** dev

## Overview

Now that the core typography, layout, and visual polish are solid, these are potential next directions for dtslides — organized by effort level.

---

## Quick Wins (single session each)

### 1. ✅ Custom Footer Text

Add a configurable footer line (e.g., course name, conference, date) that appears on every slide.

**Implementation:** Uses RevealJS built-in `footer` option with SCSS styling in `_shared.scss` and inline JS in both `_extension.yml` to hide footer on the title slide (CSS-only approach impossible since `.footer` is a sibling of `.slides`, not a descendant of `#title-slide`).

```yaml
format:
  dtslides-unm-revealjs:
    footer: "STAT 440 — Regression Analysis"
```

```scss
.reveal .footer {
  font-size: 0.5em;
  color: $heading-h3-color;
  opacity: 0.6;
  text-align: left;
  padding: 0 1em;
}
```

### 2. Print/PDF Optimization

Ensure `quarto render --to pdf` produces clean output:

```scss
@media print {
  .reveal .slide-logo,
  .reveal .slide-number,
  .reveal .slide-menu-button,
  .chalkboard-button { display: none; }

  .reveal .slides section {
    page-break-after: always;
    border-bottom: none;
  }
}
```

**Effort:** Small — test with `quarto render file.qmd --to pdf` and fix edge cases.

### 3. Speaker Notes Styling

Style the speaker notes view (`S` key in RevealJS) with readable fonts:

```scss
.reveal .speaker-notes {
  font-family: $font-family-sans-serif;
  font-size: 16px;
  line-height: 1.6;
}
```

**Effort:** Small.

---

## Medium Effort (1-2 sessions)

### 4. Dark Mode Variant

A `dtslides-dark` theme or toggle class for evening presentations / reduced eye strain.

**Approach A — Separate theme file:**
- New `academic-dark.scss` with inverted colors
- `$body-bg: #1a1a2e`, `$body-color: #e0e0e0`, darker code blocks

**Approach B — CSS class toggle:**
- Add `.dark-mode` class to `<html>` via RevealJS plugin
- Use CSS custom properties for all colors
- Toggle with keyboard shortcut (e.g., `D`)

**Trade-off:** Approach A is simpler but doubles maintenance. Approach B is elegant but requires refactoring all color references to CSS custom properties.

### 5. Animated Transitions Between Sections

Different transitions for section slides vs. content slides:

```yaml
format:
  dtslides-revealjs:
    transition: fade
    section-transition: slide  # custom option via plugin
```

Would require a small RevealJS plugin to detect `.section-slide` and override the transition.

### 6. Auto-fit Text

Prevent text overflow on content-heavy slides by auto-scaling font size:

```scss
.reveal .slides section.auto-fit {
  // RevealJS has built-in `data-auto-animate` but not auto-fit
}
```

This would need a JS plugin that measures content height and adjusts font-size. RevealJS's `fitty` integration could help.

---

## Larger Efforts (multi-session)

### 7. Handout Mode

Generate a condensed, non-animated version for student handouts:

- Strip fragments (show all at once)
- Remove speaker notes
- Compact layout (2-4 slides per page)
- Include slide numbers as references

**Approach:** Custom Quarto format `dtslides-handout` that uses `reveal.js` with all fragments revealed + PDF export.

### 8. Interactive Quiz Slides

Poll/quiz functionality for live lectures:

```markdown
## Pop Quiz {.quiz-slide}

What is the expected value of a sample mean?

- [ ] $\sigma^2$
- [x] $\mu$
- [ ] $\bar{x}$
- [ ] $s^2$
```

**Approach:** Custom callout type + JS to handle click state. Could integrate with classroom response systems.

### 9. Template Gallery

Pre-built slide layouts users can reference:

- Title + subtitle + logo
- Two-column comparison
- Image + caption
- Key finding highlight
- Timeline/roadmap
- Before/after comparison

**Approach:** A `_templates/` directory with QMD snippets users can copy.

---

## Recommended Next Steps

1. ~~**Custom footer text** (#1)~~ — ✅ Done
2. **Print/PDF optimization** (#2) — students frequently request PDF versions
3. **Speaker notes styling** (#3) — quick win for presenter experience
4. **Dark mode** (#4) — nice differentiator, but only if there's demand

---

## Technical Debt to Address

- ~~**`_shared.scss` duplication**~~ — ✅ Resolved via relative symlink (`_extensions/dtslides-unm/styles/_shared.scss -> ../../dtslides/styles/_shared.scss`). Edits to the canonical copy automatically apply to both themes.
- ~~**MathJax 2 → 3 migration**~~ — ✅ Resolved. The `html-math-method` block was at the top level of `_extension.yml` (dead config). Moved `html-math-method: mathjax` inside `contributes > formats > revealjs:`, which tells Quarto to use MathJax 3. The RevealJS `math.js` plugin throws a non-fatal console error (it expects MathJax 2 API), but rendering works correctly since Quarto handles math via Pandoc's HTML output which MathJax 3 picks up directly.
- **RevealJS plugin loading** — `unm-revealjs.js` is dead code: a boilerplate Quarto extension template that binds `T` to `deck.shuffle()`. It's not registered in `_extension.yml` (no `revealjs-plugins:` key) and never loads. Safe to delete or repurpose for actual functionality (e.g., the footer toggle could have gone here instead of `include-after-body`).
