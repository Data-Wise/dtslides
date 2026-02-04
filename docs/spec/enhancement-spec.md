# dtslides Enhancement Specification

**Project:** unm-revealjs → dtslides
**Version:** 1.0.2 → 1.1.0
**Date:** 2026-02-03
**Author:** Davood Tofighi

---

## 1. Code Review Findings

### 1.1 Critical Issues

#### 1.1.1 Code block font size is too small (8px)

`$code-block-font-size: 0.5rem` in `custom.scss:69` resolves to 8px at 16px root. R code output is nearly unreadable when projected. The CSS file `unm-revealjs.css:42` attempts to fix this with `.reveal pre { font-size: 1.75rem !important }`, creating a conflicting override.

#### 1.1.2 Conflicting styles between custom.scss and unm-revealjs.css

Both files style the same selectors (`pre`, `pre code`, `.reveal table`, callouts, title slide, code blocks). The CSS file uses `!important` on 20+ rules, creating specificity wars. Title slide sizing differs between them: SCSS sets `h1: 2.5em`, CSS sets `1.6em`.

#### 1.1.3 Emphasis mixin has broken parameter reference

In `custom.scss:390`, the mixin declares `$padding-x: $spacing-x` but `$spacing-x` is undefined (should be `$spacing-xs`). The mixin body then ignores its own `$padding-y`/`$padding-x` parameters entirely, hardcoding `$spacing-xxs` on line 396.

#### 1.1.4 Missing Quarto SCSS section markers

`custom.scss` lacks `/*-- scss:defaults --*/` and `/*-- scss:rules --*/` markers that Quarto uses to separate variable declarations from CSS rules. This prevents users from overriding SCSS variables via their document YAML front matter.

### 1.2 Moderate Issues

| #   | Issue                   | Detail                                                                                          |
|-----|-------------------------|-------------------------------------------------------------------------------------------------|
| 5   | Font loading redundancy | Roboto loaded in both SCSS and CSS files; Nunito loaded but never used                          |
| 6   | Dead/unused CSS classes | .section-slide, .large-image-slide, .slide-code used in examples but undefined                  |
| 7   | !important overuse      | h2 color, slide-number positioning, and many CSS-file rules use !important unnecessarily        |
| 8   | Hardcoded colors        | #333, #444, #f5f5f5 used outside the SCSS variable system                                      |
| 9   | Table hover effect      | Row highlighting on mouse hover is distracting during presentations                             |

### 1.3 Minor Issues

| #    | Issue                           | Detail                                                                                     |
|------|---------------------------------|--------------------------------------------------------------------------------------------|
| 10   | Spacing comment mismatches      | Comments say 2rem / 1.5rem but $spacing-md is 1rem                                         |
| 11   | Double font-size on code blocks | Both pre and pre code independently set $code-block-font-size, compounding the tiny size   |

---

## 2. Enhancement Specification

### 2.1 Rename Extension and Repository: `unm-revealjs` → `dtslides`

**Rationale:** The extension will support multiple themes beyond UNM. A neutral name allows both UNM-branded and generic academic themes under one extension.

**User-facing format names after rename:**

| Format string                  | Theme                                           |
|--------------------------------|-------------------------------------------------|
| format: dtslides-revealjs      | Generic academic (navy/slate palette)           |
| format: dtslides-unm-revealjs  | UNM-branded (existing cherry/turquoise palette) |

**Actions:**

1. Rename extension directory: `_extensions/unm/` → `_extensions/dtslides/`
2. Update `_extension.yml` title to `dtslides`
3. Rename GitHub repo: `Data-Wise/unm-revealjs` → `Data-Wise/dtslides`
   - GitHub auto-redirects the old URL, so existing links won't immediately break
   - Run: `gh repo rename dtslides`
4. Update the Quarto extension listing submission
   - The listing PR/entry at `quarto-dev/quarto-web` needs the repo URL updated
5. Update `README.md` install command: `quarto add Data-Wise/dtslides`
6. Update local git remote if needed: `git remote set-url origin <new-url>`
7. Rename local project directory: `unm-revealjs/` → `dtslides/`

### 2.2 Consolidate Stylesheets

**Problem:** Two separate files (`custom.scss` and `unm-revealjs.css`) fight each other with `!important` overrides.

**Solution:** Merge useful utility classes from `unm-revealjs.css` into the SCSS source, then delete the CSS file.

**Classes to merge into SCSS:**

- Font sizing: `.largest`, `.larger`, `.large`, `.small`, `.smaller`, `.smallest` with proportional `pre`/`code` variants
- Text alignment: `.left`, `.center`, `.right`
- Vertical alignment: `.v-center`, `.v-top`, `.v-bottom`
- Badge: `.badge`, `.badge-primary`, `.badge-secondary`
- DataTable component sizing
- Panel tabset styling
- Citation/references styling

**Files to delete:** `unm-revealjs.css`, `custom-bk1.scss`

**Add Quarto SCSS markers:** `/*-- scss:defaults --*/` before variables, `/*-- scss:rules --*/` before rules.

### 2.3 Fix R Code Block Styling

| Property                | Before                            | After            | Rationale                                              |
|-------------------------|-----------------------------------|------------------|--------------------------------------------------------|
| `$code-block-font-size` | `0.5rem` (8px)                    | `0.55em`         | Relative to slide, scales with `.small-slide` etc.     |
| `pre code` font-size    | `$code-block-font-size` (duped)   | `inherit`        | Prevent double-application of font-size                |
| `$code-line-height`     | `1.2`                             | `1.4`            | Better readability for R code                          |
| `pre code` background   | `#f5f5f5` (hardcoded)             | `$code-block-bg` | Single variable controls both                          |

#### New: R console output distinction

Add `.cell-output-stdout pre` styling with:

- Slightly lighter background than source code
- Left accent border (using theme accent color)
- This makes code vs. output visually distinguishable at a glance

**Fix:** `output-location: column` spacing between code and output panels.

### 2.4 Fix Broken SCSS and Clean Up

- Fix `$spacing-x` → `$spacing-xs` in emphasis mixin
- Make emphasis mixin use its declared `$padding-y`/`$padding-x` parameters
- Replace hardcoded colors (`#333`, `#444`, `#f5f5f5`) with SCSS variables
- Remove unused Nunito font import, deduplicate Roboto import
- Define missing classes: `.section-slide` (section break), `.slide-code` (code-focused slide)
- Remove table hover effect (`tr:hover` background change)
- Fix spacing comment/value mismatches
- Trim redundant verbose comments

### 2.5 Shared Styles Architecture (`_shared.scss`)

**Create** `_extensions/dtslides/styles/_shared.scss` as a color-agnostic Sass partial.

**Contents:**

- Spacing scale, line heights, font families, font sizes
- Base reveal styles (body, links, images)
- Code block structure (using color variables)
- Table structure (no hover, using color variables for header/stripe)
- R output styling (`.cell-output-stdout pre`)
- Slide type classes: `.small-slide`, `.tiny-slide`, `.quote-slide`, `.image-slide`, `.section-slide`, `.slide-code`
- Emphasis classes: `.alert`, `.fg`, `.bg`, `.highlight`, `.note`, `.warning`
- Callout system: `.callout-example`, `.callout-question`, `.callout-insight`
- Button, list, utility classes (font sizing, alignment, badges, datatables, panel-tabset, citations)
- Logo/footer/slide-number positioning

**Usage pattern:** Both `custom.scss` and `academic.scss` will:

1. Define their color variables
2. `@import '_shared'`
3. Add any theme-specific overrides

### 2.6 Academic Theme (`academic.scss`)

**Create** `_extensions/dtslides/styles/academic.scss` with Navy/Slate color palette.

**Color palette:**

| Variable         | Hex       | Usage                        |
|------------------|-----------|------------------------------|
| `$primary`       | `#1B365D` | Headings (h1, h2), title     |
| `$accent`        | `#4A6FA5` | Links, buttons, h4           |
| `$secondary`     | `#6C757D` | h3, borders                  |
| `$highlight`     | `#E8C547` | Highlights                   |
| `$success`       | `#2E7D32` | Example callouts             |
| `$warning`       | `#E65100` | Warning/caution              |
| `$info`          | `#0277BD` | Insight callouts, `.fg`      |
| `$table-header`  | `#D6DCE5` | Table headers                |
| `$table-stripe`  | `#F5F7FA` | Alternating rows             |

**Title slide:** Clean white background, no logo, no background image. Users can add their own via YAML front matter overrides.

### 2.7 Extension Configuration (`_extension.yml`)

> **Implementation note:** The spec below proposed a single `_extension.yml` with both formats. The actual implementation uses two separate extension directories (`_extensions/dtslides/` and `_extensions/dtslides-unm/`), each with its own `_extension.yml`. This was necessary because Quarto's SCSS `@import` resolves relative to the extension directory, and the UNM theme needs its own `images/` directory for logos and backgrounds.

```yaml
# As originally proposed (single file):
title: dtslides
author: Davood Tofighi
version: 1.1.0
quarto-required: ">=1.6.0"

contributes:
  formats:
    revealjs:                          # → format: dtslides-revealjs (academic)
      theme: [simple, "styles/academic.scss"]
      slide-number: true
      scrollable: true
      code-copy: false
      margin: 0.08

    unm-revealjs:                      # → format: dtslides-unm-revealjs (UNM)
      theme: [simple, "styles/custom.scss"]
      logo: "images/ms-logo.png"
      title-slide-attributes:
        data-background-image: "images/unm-title-slide.png"
        data-background-size: "520px 150px"
        data-background-position: bottom
        data-background-opacity: "0.9"
        data-background-repeat: "no-repeat"
      slide-number: true
      scrollable: true
      code-copy: false
      margin: 0.08

html-math-method:
  method: mathjax
  url: "https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"
```

### 2.8 Update Examples

- **`example.qmd`** — Update format to `dtslides-unm-revealjs`, fix undefined class references
- **`example-long.qmd`** — Update format, fix `.section-slide`, `.slide-code`, `.large-image-slide` references
- **`template.qmd`** — Update format, add comment showing both theme options
- **`example-academic.qmd`** (new) — Example using `format: dtslides-revealjs`

---

## 3. Files Summary

- **Create:** `_extensions/dtslides/styles/_shared.scss`
- **Create:** `_extensions/dtslides/styles/academic.scss`
- **Create:** `example-academic.qmd`
- **Rename:** `_extensions/unm/` → `_extensions/dtslides/`
- **Rename:** GitHub repo `Data-Wise/unm-revealjs` → `Data-Wise/dtslides`
- **Rename:** Local directory `unm-revealjs/` → `dtslides/`
- **Modify:** `_extensions/dtslides/styles/custom.scss`
- **Modify:** `_extensions/dtslides/_extension.yml`
- **Modify:** `template.qmd`, `example.qmd`, `example-long.qmd`
- **Modify:** `README.md` (install command, repo references)
- **Delete:** `_extensions/dtslides/styles/unm-revealjs.css`
- **Delete:** `_extensions/dtslides/styles/custom-bk1.scss`
- **Delete:** `unm-revealjs.Rproj`, `unm-revealjs.code-workspace`
- **Update:** Quarto extension listing submission (repo URL)

---

## 4. Verification Plan

1. `quarto render template.qmd` — UNM theme renders with improved code blocks
2. `quarto render example.qmd` — All emphasis classes, callouts, and code blocks work
3. `quarto render example-long.qmd` — R code slides, tables, and math render correctly
4. `quarto render example-academic.qmd` — Academic theme renders with neutral branding
5. Browser dev tools — No `!important` specificity conflicts remain
6. Visual inspection — R code blocks readable at presentation zoom; output visually distinct from source code

---

## 5. Design Decisions

| Decision             | Choice                 | Rationale                                                |
|----------------------|------------------------|----------------------------------------------------------|
| Extension name       | dtslides               | Neutral personal brand, supports multiple themes         |
| Repo rename          | Data-Wise/dtslides     | Align repo name with extension; GitHub redirects old URL |
| Academic palette     | Navy/Slate             | Professional, classic academic journal aesthetic         |
| Academic title slide | Clean white            | No branding; users add their own via YAML                |
| R output styling     | Distinct from source   | Lighter bg + left accent border for visual clarity       |
| Table hover          | Removed                | Distracting during presentations                         |
| Font Awesome         | Kept                   | Useful for icon shortcodes, minimal overhead             |
| custom-bk1.scss      | Deleted                | Available in git history; reduces confusion              |
| Shared styles        | \_shared.scss partial  | Keeps both themes DRY, easy to add future themes         |
