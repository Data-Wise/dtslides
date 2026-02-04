# dtslides CSS Class Reference

Quick reference for all user-facing CSS classes. Both themes (Academic and UNM) share these classes.

## Slide types

Apply as a class on any slide heading: `## Title {.small-slide}`

| Class                | Effect                                      |
|----------------------|---------------------------------------------|
| `.small-slide`       | Reduced font size for dense content         |
| `.tiny-slide`        | Even smaller for reference tables           |
| `.quote-slide`       | Centered text with styled blockquote        |
| `.image-slide`       | Minimal chrome, image-focused               |
| `.section-slide`     | Large centered heading for section dividers |
| `.slide-code`        | Optimized for code-heavy slides             |
| `.large-image-slide` | Full-height image display (80vh max)        |
| `.scrollable`        | Enable vertical scrolling on a slide        |

## Emphasis spans

Inline markup: `[text]{.alert}`

| Class        | Effect                                    |
|--------------|-------------------------------------------|
| `.alert`     | Bold, alert color, tinted background      |
| `.fg`        | Bold, accent foreground color             |
| `.bg`        | White text on accent background           |
| `.highlight` | Yellow/gold highlighted text              |
| `.note`      | Italic, tinted background                 |
| `.warning`   | Bold, warning color, tinted background    |

## Font sizing

Apply to a slide or a div to scale all content inside:

| Class       | Text   | Code blocks scale too |
|-------------|--------|-----------------------|
| `.larger`   | 1.50em | Yes                   |
| `.largest`  | 1.20em | Yes                   |
| `.large`    | 1.25em | Yes                   |
| `.small`    | 0.80em | Yes                   |
| `.smaller`  | 0.64em | Yes                   |
| `.smallest` | 0.51em | Yes                   |

## Alignment

### Text (horizontal)

| Class      | Effect         |
|------------|----------------|
| `.left`    | Left-aligned   |
| `.center`  | Centered       |
| `.right`   | Right-aligned  |

### Vertical (grid-based)

| Class        | Effect   |
|--------------|----------|
| `.v-center`  | Centered |
| `.v-top`     | Top      |
| `.v-bottom`  | Bottom   |

## Buttons and badges

```markdown
[[Jump to Slide]{.button}](#target-id)

Inline labels: [v1.1.0]{.badge} [New]{.badge-secondary}
```

| Class               | Effect                               |
|---------------------|--------------------------------------|
| `.button`           | Accent-colored link button with icon |
| `.badge`            | Primary-colored inline label         |
| `.badge-primary`    | Same as `.badge`                     |
| `.badge-secondary`  | Lighter variant                      |

## Custom callouts

```markdown
::: {.callout-example}
Worked example content.
:::
```

| Class               | Title    | Border color           |
|---------------------|----------|------------------------|
| `.callout-example`  | Example  | Green (success)        |
| `.callout-question` | Question | Primary (navy/cherry)  |
| `.callout-insight`  | Insight  | Blue (info/turquoise)  |

Standard Quarto callouts (`.callout-note`, `.callout-warning`, `.callout-tip`, `.callout-important`, `.callout-caution`) are also styled.

## Code blocks

No classes needed. Applied automatically:

- Source code blocks use Fira Code font with syntax highlighting
- R console output gets a lighter background with an accent-colored left border
- Use `output-location: column` in chunk options for side-by-side layout

## Tables

No classes needed. Applied automatically:

- Striped alternating rows
- Uppercase header text
- No hover effect (keeps presentations clean)

## Quick examples

### Dense content slide

```markdown
## Full Regression Output {.small-slide}

- Lots of content here...
```

### Section divider

```markdown
# Methods {.section-slide}
```

### Emphasis in text

```markdown
The result is [statistically significant]{.alert} with
[p < 0.001]{.highlight}.
```

### Font size in a div

```markdown
::: {.small}
This entire block is smaller, including any code inside it.
:::
```

### Cross-reference button

```markdown
## Results

[[See Methods]{.button}](#methods-slide)

## Methods {#methods-slide}
```
