# Getting Started

Neutral Beamer is a Quarto format extension for PDF slide decks. It renders Beamer slides with the Metropolis base theme, a neutral color palette, and readable typography. No logos, no branding, no topic-specific styling. Use it for teaching, research talks, internal reviews, or any presentation where clarity matters.

## Requirements

- **Quarto** >= 1.4
- **TeX Live** with the following packages (available in most full or medium TeX Live installations):
  - `xelatex` (default engine)
  - `beamertheme-metropolis`
  - `fontspec`
  - `sourcesanspro`
  - `sourcecodepro`

No internet connection is needed at render time. System fonts are not required: the extension falls back to Latin Modern Sans and Latin Modern Mono if Source Sans 3 and Source Code Pro are not installed on the system.

## Installation

**Option A: Copy the extension folder.**

Place `_extensions/neutral/` next to your `.qmd` file. Quarto resolves extensions relative to the document directory, so the folder must sit beside the file you render (or in a parent directory).

```
my-talk/
  _extensions/
    neutral/
      _extension.yml
      header.tex
      before-body.tex
  my-talk.qmd
```

**Option B: Install from a Git repository.**

```bash
quarto add <org>/<repo>
```

This copies `_extensions/neutral/` into your project automatically.

## Minimal working example

Create a file called `deck.qmd` next to the `_extensions/neutral/` folder:

```markdown
---
title: "My First Talk"
author: "Your Name"
date: today
format: neutral-beamer
---

## Introduction

- One idea per slide
- Neutral palette, no branding
- Built on Beamer with Metropolis
```

Render it:

```bash
quarto render deck.qmd
```

The output is `deck.pdf` in the same directory.

## Setting a project default

To use `neutral-beamer` for every `.qmd` in a project without repeating the format, add this to `_quarto.yml`:

```yaml
project:
  type: default

format: neutral-beamer
```

Any `.qmd` in that directory without its own `format` field will use `neutral-beamer`.

## Demo deck tour

The included `template.qmd` exercises every feature. Here is what each slide demonstrates:

| Slide | What it shows |
|-------|---------------|
| Title slide | `title`, `subtitle`, `author`, `institute`, `date: today`, `date-format: long` |
| This template keeps PDF slide decks readable | Opening content slide: bullets plus one short framing sentence |
| Section page: "One idea per slide keeps reading easy" | `#` heading creates a section page with a Metropolis progress bar |
| A level-one heading creates a section page | Content slide explaining section pages |
| A minimal readable default needs no branding | Overview of the neutral palette and typography |
| Bullet lists | Flat and nested bullet items, no size reduction at deeper levels |
| Numbered lists | Ordered list for sequences and steps |
| Emphasis and alert | "Emphasis marks key terms and alerts draw attention": italic, bold, `[text]{.alert}`, plus a `:::: {.callout-note}` block inside the same slide |
| Transparent block | `:::: {.callout-tip}` block showing transparent block styling |
| Two-column layout | `:::: {.columns}` with two `::: {.column}` children at 48% width |
| Simple table | Markdown table with a `: caption` line |
| Code | Fenced `python` code block with syntax highlighting (no Jupyter required) |
| Math | Inline `$...$` and display `$$...$$` LaTeX math |
| Section page: "Extra material belongs after the main story" | Second section page |
| A new deck starts by replacing this example | Final content slide |

Note: outer callout and column fences use four colons (`::::`), inner column fences use three (`:::`).

Section pages (marked with `#`) are unnumbered but still count as frames. This means the slide number on the slide following a section page will appear "off by one" relative to the PDF page index. This is expected Beamer behavior.

## Known issues

A first render occasionally fails with a "compilation failed" message. This is a transient TeX issue. Running `quarto render deck.qmd` again typically succeeds. If it persists, add `keep-tex: true` to your format options to inspect the intermediate `.tex` file:

```yaml
format:
  neutral-beamer:
    keep-tex: true
```

Note: `keep-tex` goes under `format.neutral-beamer` in your YAML, not as a CLI flag (`--keep-tex` is rejected by the pandoc Beamer writer).

## Next steps

- [Format options reference](format-options.md) -- every option, merge semantics, and override recipes
- [Authoring guide](authoring-guide.md) -- writing self-contained slides, lists, callouts, columns, tables, code, and math
- [Validation guide](validation.md) -- PDF text gates and page-by-page visual review
- [Customization](customization.md) -- palette, fonts, spacing, footer, theme override
- [Troubleshooting](troubleshooting.md) -- debugging, known issues, error catalog
