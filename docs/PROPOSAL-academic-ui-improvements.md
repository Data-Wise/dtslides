# dtslides Academic Theme — UI/Design Improvements

**Generated:** 2026-02-03
**Context:** `_extensions/dtslides/styles/academic.scss` + `_shared.scss`
**Last Updated:** 2026-02-03

## Overview

Audit of the dtslides academic theme against best practices from the [Clean theme](https://github.com/grantmcdermott/quarto-revealjs-clean) (Grant McDermott), [Slidecraft 101](https://emilhvitfeldt.com/project/slidecraft-101/) (Emil Hvitfeldt), and current [presentation design trends](https://www.slidesai.io/blog/presentation-design-trends).

---

## Quick Wins — ✅ ALL COMPLETED

### 1. ✅ Increase Root Font Size (26px → 34px)

```scss
// academic.scss
$font-size-root: 34px;  // was 26px
```

**Why:** 26px is well below the RevealJS default (40px) and Clean theme (32px). Everything scales from this value — headings, body, code. Bumping to 34px makes content readable in lecture halls and over screen-share.

### 2. ✅ Increase Line Height (1.3 → 1.5)

```scss
$line-height-body: 1.5;  // was 1.3
```

**Why:** 1.3 makes multi-line bullets and paragraphs feel cramped. Standard recommendation is 1.5–1.6 for presentation text.

### 3. ✅ Fix Heading Font (Segoe UI → Lato)

```scss
// Remove the mismatched variable. Use the one that matches the Google Font import.
$heading-font-family: "Lato", sans-serif;  // was "Segoe UI", Tahoma, ...
```

**Why:** Lato is already loaded via Google Fonts but isn't actually used by h1 due to variable name mismatch. Segoe UI doesn't exist on macOS, causing unpredictable fallbacks. Roboto (body) + Lato (headings) is a proven pairing.

### 4. ✅ Increase Code Block Font Size (0.55em → 0.65em)

```scss
$code-block-font-size: 0.65em;  // was 0.55em
```

**Why:** At 0.55em of 26px root, code is ~14px — barely readable on projectors. Combined with the root size increase, this gives comfortable code display.

---

## Medium Effort — ✅ ALL COMPLETED

### 5. ✅ Soften Heading Weights

```scss
// _shared.scss — Headings section (selectors use .reveal prefix for specificity)
.reveal h1 {
  font-weight: 600;  // was bolder
}

.reveal h2 {
  font-weight: 500;  // was bolder
  margin: 0 0 $spacing-xl 0;  // increased from $spacing-xxs → $spacing-xl (2rem)
}
```

**Why:** `bolder` on large text looks heavy and competes with body content. The Clean theme uses `lighter` — 500–600 is a good middle ground for academic formality without visual weight.

**Additional fix applied:** All heading selectors changed from bare `h1`/`h2`/`h3`/`h4` to `.reveal h1`/`.reveal h2` etc. to beat RevealJS's built-in specificity.

### 6. ✅ Increase Code Block Inner Padding

```scss
// _shared.scss — Code Blocks section
.reveal pre code {
  padding: $spacing-md;  // was $spacing-xs (0.5rem → 1rem)
}
```

**Why:** Slidecraft recommends 20–25px padding inside code containers. Current 0.5rem (8px) makes code feel cramped against edges.

### 7. ✅ Increase Table Font Size

```scss
// academic.scss
$font-size-small: 0.75em;  // was 0.6em (used by tables and tiny-slide)
```

**Why:** Table text at 0.6em is too small for projection. 0.75em keeps tables compact but readable.

### 8. ✅ Improve List Spacing

```scss
// _shared.scss — Lists section
.reveal .slide ul li,
.reveal .slide ol li {
  margin-top: 0.3em;   // was 0.2em
  margin-bottom: 0.3em;
}
```

**Why:** Slightly more breathing room between bullet points improves scannability.

---

## Additional Fixes Applied (Not in Original Proposal)

### A. ✅ CSS Specificity Fix for Callouts

Quarto's built-in callout CSS uses `div.callout` element selectors. Changed all callout overrides in `_shared.scss` from `.reveal .callout...` to `.reveal div.callout...` for proper specificity.

### B. ✅ rem → em Conversion

Changed root font-size from 26→34px inflated all `rem` values by 31%. Converted all font-size `rem` values to `em`:
- Font-size utility `pre` blocks (`.largest pre` through `.smallest pre`)
- References section font-size (`0.7rem` → `0.6em`)
- Callout title/content/icon sizes (`1.6rem`/`1.4rem` → `1.1em`/`0.95em`)

### C. ✅ UNM Theme Sync

Applied matching improvements to UNM theme's `custom.scss`:
- `$font-size-small: 0.6em` → `0.75em`
- `$code-block-font-size: 0.55em` → `0.65em`
- `_shared.scss` already synced (all layout/specificity fixes shared)

---

## Bigger Changes (Future Session)

### 9. Code Source + Output Visual Connection

Remove the gap between source code and its output to make them appear as one unit:

```scss
// _shared.scss
.reveal div.sourceCode + div.cell-output {
  margin-top: 0;  // was $spacing-xs
}

.reveal div.cell-output-stdout pre,
.reveal div.cell-output pre {
  border-radius: 0 0 6px 6px;  // round only bottom corners
  border-left: 3px solid $accent;
  border-top: 1px solid rgba($accent, 0.2);  // subtle separator
}

.reveal div.sourceCode {
  margin-bottom: 0;  // remove bottom margin when followed by output

  & + div.cell-output pre {
    margin-top: 0;
  }
}
```

### 10. Slide Margin

In `_extension.yml`:
```yaml
margin: 0.10  # was 0.08
```

More edge breathing room. Prevents content from feeling crowded at slide edges.

### 11. Section Slide Enhancement

Add a subtle bottom border or accent line to section divider slides:

```scss
.section-slide h1 {
  border-bottom: 3px solid $accent;
  padding-bottom: $spacing-md;
  display: inline-block;
}
```

### 12. Link Underlines

Add subtle underlines to links for accessibility:

```scss
.reveal a {
  text-decoration: underline;
  text-underline-offset: 3px;
  text-decoration-thickness: 1px;
  text-decoration-color: rgba($accent, 0.4);
}
```

---

## Reference Themes

| Theme | Root | Line Height | Heading Weight | Code Size |
|-------|------|-------------|----------------|-----------|
| RevealJS default | 40px | 1.6 | normal | 0.55em |
| Clean (McDermott) | 32px | 1.6 | lighter | inherited |
| Slidecraft (Hvitfeldt) | 40px | 1.6 | varies | 0.7–0.8em |
| ~~dtslides original~~ | ~~26px~~ | ~~1.3~~ | ~~bolder~~ | ~~0.55em~~ |
| **dtslides current** | **34px** | **1.5** | **500–600** | **0.65em** |

## Sources

- [Quarto RevealJS Clean Theme](https://github.com/grantmcdermott/quarto-revealjs-clean) — Grant McDermott
- [Slidecraft 101: Colors & Fonts](https://emilhvitfeldt.com/post/slidecraft-colors-fonts/) — Emil Hvitfeldt
- [Slidecraft 101: Code & Output](https://emilhvitfeldt.com/post/slidecraft-code-output/) — Emil Hvitfeldt
- [Quarto RevealJS Themes](https://quarto.org/docs/presentations/revealjs/themes.html) — Quarto docs
- [RevealJS Presentation Size](https://revealjs.com/presentation-size/) — RevealJS docs
- [2026 Presentation Design Trends](https://www.slidesai.io/blog/presentation-design-trends) — SlidesAI
