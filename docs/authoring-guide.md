# Authoring guide

This guide explains how to write slide content for the `neutral-beamer` Quarto
format. It covers the document frontmatter and every content block the template
supports: slides, section pages, lists, emphasis, callouts, columns, tables,
code, math, and speaker notes. Every snippet below is valid for this extension
and is taken from or consistent with `template.qmd`.

## Before you start

Set the format and keep the extension folder in place:

- Use `format: neutral-beamer` in your document's YAML frontmatter.
- Keep the `_extensions/neutral/` folder next to your `.qmd`. Quarto resolves
  extensions relative to the document's directory, so rendering from a different
  folder (for example `/tmp`) fails.
- To render: `quarto render your-deck.qmd`. The output is `your-deck.pdf` in the
  same folder.

The palette and fonts are fixed in `_extensions/neutral/header.tex`. `brand.yml`
is not applied on the Beamer path, so colors and fonts are not changed through
Quarto's branding system. Edit `header.tex` if you need different colors or fonts.

## Document frontmatter

The frontmatter sets the title slide and deck-wide options. The template uses
these fields:

```yaml
---
title: "A Neutral Presentation Template"
subtitle: "Clear structure, low density, easy to read"
author: "Author Name"
institute: "Department, University"
date: today
date-format: long
format:
  neutral-beamer:
    aspectratio: 43
    fontsize: 11pt
    section-titles: true
    slide-level: 2
---
```

Field notes:

- `title`, `subtitle`, `author`, `institute` are plain text or quoted strings.
  They appear on the title slide.
- `date: today` inserts the render date. `date-format: long` selects the long
  written-out form (for example "September 5, 2026").
- `format.neutral-beamer` holds deck options. `aspectratio: 43` is 4:3; use
  `169` for 16:9 widescreen. `fontsize: 11pt` is the body text size.
  `section-titles: true` turns level-1 headings into section pages.
  `slide-level: 2` means level-2 headings start new slides.

The defaults injected by the extension are: theme `metropolis`, aspect ratio
`43`, font size `11pt`, section titles on, numbered sections on, no navigation
symbols, the `xelatex` PDF engine, and `header.tex` included in the header.

## Slide anatomy

The deck is organized by heading level:

- A level-2 heading (`##`) starts a slide.
- A level-1 heading (`#`) starts a section page.
- `slide-level: 2` is what makes level-2 headings become slides.

```markdown
## This is a slide

Body text and a short bullet list go here.

# This is a section page

A level-1 heading becomes a full section page with a progress bar above the
title. Use section pages to divide the talk into parts.
```

Section page behavior you should expect:

- Section pages are not numbered, but they still count as frames in the deck.
- Because of that, the slide number shown after a section page looks "off by
  one" compared with the PDF page index. This is expected, not a bug. For
  example, if a section page is frame 4, the next content slide is frame 5 but
  its printed number may read 4. The printed number tracks content frames, not
  the raw PDF page count.

## Bullet lists

Bullet lists use light-gray circle markers. Indent nested items with two or
more spaces. Nested items keep the same font size as their parent; they do not
shrink at deeper levels.

```markdown
- First point states the main idea
- Second point adds supporting detail
  - Nested points keep the same size as the parent
  - No shrinking at deeper levels
- Third point closes the set
```

Keep each list to three to five items.

## Numbered lists

Numbered lists work for sequences, steps, and methods:

```markdown
1. Define the question or goal
2. Collect and check the evidence
3. Analyse and compare alternatives
4. Summarise the outcome
```

## Emphasis and alert

Use Markdown emphasis for ordinary stress. Use the `.alert` span for a term you
want to stand out in bold terracotta.

```markdown
Regular emphasis uses *italic* or **bold**.

Alerted text combines bold and color: [critical value]{.alert}.

Use alerts sparingly, one per slide at most.
```

Rules:

- `*italic*` and `**bold**` are standard Markdown and never rely on color alone.
- `[term]{.alert}` renders bold in terracotta. It is color plus weight, not color
  alone, so it stays readable for everyone.
- Alerts draw the eye. Use at most one per slide so the emphasis is not diluted.

## Callouts

Quarto callouts render on Beamer as transparent blocks with a teal title. Use
`callout-note` and `callout-tip` for definitions, notes, or takeaways.

```markdown
::: {.callout-note}
## Quarto callout

This is a `::: {.callout-note}` block. In Beamer it renders as a transparent
block with a teal title.

Use callouts for definitions, notes, or takeaways.
:::

::: {.callout-tip}
## Block title

Blocks are transparent by design. The title appears in teal and the body
inherits the normal text color.
:::
```

Notes:

- The `##` inside the callout is the block title; it appears in teal.
- The block body is transparent, so the off-white background shows through.
- Callouts add no visual weight, which fits the low-density layout.

## Two-column layout

Wrap columns in a `columns` div, then put each side in a `column` div with a
`width`. The widths are percentages of the text area; two `48%` columns leave a
small gap.

```markdown
::: {.columns}

::: {.column width="48%"}
**Left column**

- Compact point
- Another point
- Keeps text scannable
:::

::: {.column width="48%"}
**Right column**

- Parallel point
- Supporting detail
- Balanced whitespace
:::

:::
```

Use columns to place text beside a figure, table, or list. Keep the two widths
summing below 100 percent so the columns do not collide.

## Tables

Write a Markdown table and add a caption on a line starting with `:`.

```markdown
| Method | Sample | Accuracy |
|--------|--------|----------|
| A      | 120    | 0.82     |
| B      | 120    | 0.87     |
| C      | 120    | 0.91     |

: A minimal table with a caption
```

Keep tables small. For larger data, split the table across slides or move the
details to an appendix. Wide tables do not shrink to fit, so a few columns and
rows read best.

## Code

There are two ways to show code.

### Fenced code block (no kernel needed)

A plain fenced block renders with monospace font and shading. It needs no
Jupyter kernel, so it always works.

````markdown
```python
def mean(values):
    return sum(values) / len(values)

mean([2, 4, 6, 8])
```
````

### Executable cell (needs Jupyter)

An executable cell uses triple braces and cell options. It requires a Python
(Jupyter) kernel installed on the machine.

````markdown
```{python}
#| echo: true
#| eval: true
import numpy as np
np.mean([2, 4, 6, 8])
```
````

The `#| echo: true` option prints the source; `#| eval: true` runs it. The demo
deck does not exercise executable cells, so test them in your own environment
with a kernel available.

## Math

Inline math uses single dollar signs. Display math uses double dollar signs on
their own lines.

```markdown
Inline math: $y = mx + c$. Display math:

$$
\hat{y} = \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \epsilon
$$
```

Equations use the Beamer math fonts and follow the surrounding text size.

## Speaker notes

Put speaker notes in a `notes` div. They are hidden from the slides and are for
your own reference while presenting.

````markdown
::::: {.notes}
Welcome the audience and outline the session in one sentence.
:::::
````

Notes do not appear in the PDF. Use them for cues, timing, or things you want
to say but not show.

## Debugging a render

Two practical notes when something looks wrong:

- A first render occasionally fails with a transient "compilation failed"
  message. Rerendering the same file usually succeeds. Treat a single failure as
  a known quirk before investigating further.
- To inspect the generated LaTeX, set `keep-tex: true` under
  `format.neutral-beamer` in the YAML. Do not pass the `--keep-tex` CLI flag;
  pandoc rejects it for this format.

```yaml
format:
  neutral-beamer:
    keep-tex: true
```

## Authoring checklist

Use this checklist when building or reviewing a deck:

- One idea per slide. Split a dense slide into two.
- Keep bullet lists to three to five items.
- Use `#` for section pages and `##` for slides.
- Mark key terms with `[term]{.alert}`, at most one per slide.
- Use `::: {.columns}` for side-by-side content.
- Keep tables small; move large data to an appendix.
- Put delivery cues in `::::: {.notes}` so they stay off the slides.
