# Changelog

All notable changes to this project will be documented in this file.

## 1.1.0 - 2026-02-03
### Added
- Academic theme (`dtslides-revealjs`) — navy/slate palette, no institutional branding
- Shared `_shared.scss` partial (712 lines) imported by both themes
- `example-academic.qmd` with scrollable slide demo
- Quarto SCSS section markers (`/*-- scss:defaults --*/`, `/*-- scss:rules --*/`)
- R output styling: lighter background + left accent border for `.cell-output-stdout`
- Font sizing utility classes (`.largest` through `.smallest`)
- Alignment utilities, badge classes, DataTable/panel-tabset styling

### Changed
- Renamed extension from `unm` to `dtslides` (two-directory architecture)
- Renamed GitHub repo from `Data-Wise/unm-revealjs` to `Data-Wise/dtslides`
- UNM theme format: `unm-revealjs` → `dtslides-unm-revealjs`
- Increased code block font size from 0.5rem to 0.55em
- Increased code line height from 1.2 to 1.4
- Improved list spacing (line-height 1.4, margin 0.2em between items)
- Font-size-root reduced from 30px to 26px for better content density

### Fixed
- Conflicting styles between custom.scss and unm-revealjs.css
- Broken `$spacing-x` reference in emphasis mixin (now `$spacing-xs`)
- Overflow-y scrollbar appearing on non-scrollable slides
- Double font-size compounding on code blocks
- Table hover effect removed (distracting during presentations)

### Removed
- `unm-revealjs.css` (consolidated into SCSS)
- `custom-bk1.scss` (backup file, available in git history)
- `unm-revealjs.Rproj`, `unm-revealjs.code-workspace`

## 1.0.2 - 2025-04-27
### Added
- Support for custom themes and styles in Reveal.js presentations.

## 1.0.1 - 2025-01-25
### Added
- Initial release of `unm-revealjs` extension.
- Support for basic Reveal.js features (e.g., slides, transitions).
- Integration with Quarto CLI.
