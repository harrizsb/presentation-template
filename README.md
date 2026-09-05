# Neutral Beamer Template

A neutral, readable Quarto template for PDF slide decks. Use it for any topic: teaching, research talks, internal reviews. No logos, no branding, no topic-specific styling.

## What it is

A Quarto format extension that renders Beamer PDF slides with the Metropolis base theme, tuned for readability:

- **Neutral palette.** Off-white canvas, dark-gray text, teal structure, terracotta alerts. Body text meets WCAG AAA (13.0:1). Structure (5.6:1) and alert (5.1:1) meet WCAG AA for normal text and AAA for large text.
- **Readable typography.** Source Sans for body text and titles, Source Code Pro for monospace. Body text stays at 11pt with 1.5 line spacing; nested lists do not shrink.
- **Low visual weight.** No navigation symbols, circle bullets in light gray, transparent blocks with teal titles, minimal right-aligned slide numbers.
- **Simple geometry.** 4:3 by default, 10mm side margins, one idea per slide.

## Quick start

Copy the folder to a new location, then render the demo:

```bash
quarto render template.qmd
```

The output is `template.pdf` in the same folder.

To use the format in any project, keep the `_extensions/neutral/` folder next to your `.qmd` and set:

```yaml
---
title: "My talk"
format: neutral-beamer
---
```

To set it as the default format for a project, add to `_quarto.yml`:

```yaml
project:
  type: default

format: neutral-beamer
```

Any `.qmd` in that project without an explicit `format` uses `neutral-beamer`.

To install the extension from a Git repository instead of copying the folder:

```bash
quarto add <org>/<repo>
```

## Options

Set options under `format.neutral-beamer` in YAML. Options the extension declares:

| Option | Default | Values | Description |
|--------|---------|--------|-------------|
| `aspectratio` | `43` | `43`, `169`, `1610`, `149`, `141`, `54`, `32` | Slide aspect ratio. Use `169` for widescreen. Pass as a bare number, not a quoted string. |
| `fontsize` | `11pt` | `8pt` to `20pt` | Beamer font size for body text. |
| `theme` | `metropolis` | any Beamer theme name | Base Beamer theme. Defaults to Metropolis with the neutral options baked into `header.tex`. Overriding it (e.g. `madrid`) is safe and will not clash with the defaults. |
| `section-titles` | `true` | `true`, `false` | Render level-1 headings as section pages. |
| `numbersections` | `true` | `true`, `false` | Number section pages. |
| `navigation` | `empty` | `empty`, `horizontal`, `vertical`, `fixed` | Navigation symbols. `empty` hides them. |
| `pdf-engine` | `xelatex` | `xelatex`, `lualatex`, `pdflatex` | LaTeX engine. `xelatex` uses fontspec with Source Sans 3; `pdflatex` uses the bundled `sourcesanspro` package. |

The progress bar (`progressbar=frametitle`) and title format (`regular`) are baked into `header.tex` and are not exposed as YAML options.

Standard Quarto Beamer options such as `toc`, `slide-level`, and ` incremental` also work alongside these.

Example, including the widescreen switch used in `template.qmd`:

```yaml
---
format:
  neutral-beamer:
    aspectratio: 169
    fontsize: 12pt
    toc: true
---
```

## Colors

The palette is fixed for consistency. Change it only by editing `_extensions/neutral/header.tex`.

| Element | Color | Hex |
|---------|-------|-----|
| Background | Off-white | `#F7F7F5` |
| Body text | Dark gray | `#2C2C2C` |
| Structure, block titles, section pages | Teal | `#006B8C` |
| Alerted text | Terracotta | `#B84300` |
| Bullets, rules, slide numbers | Light gray | `#9E9E9E` |

Alerts render bold in terracotta. Emphasis never relies on color alone.

## Fonts

- **XeLaTeX** (default, `pdf-engine: xelatex`): `fontspec` loads Source Sans 3 for text and Source Code Pro for monospace, with Latin Modern Sans and Latin Modern Mono as fallbacks when the fonts are not installed.
- **pdfLaTeX** (`pdf-engine: pdflatex`): the `sourcesanspro` and `sourcecodepro` packages, which bundle the fonts.

No internet-only fonts are used. All fonts ship with TeX Live.

## brand.yml

`brand.yml` is **not** auto-applied to Beamer output in this template. Quarto applies `brand.yml` to HTML formats, but the Beamer path does not read it. Colors and fonts are set in the extension's LaTeX files. If you need different colors or fonts, edit `_extensions/neutral/header.tex`.

## Documentation

Full guides live in the `docs/` folder:

| Guide | Contents |
|-------|----------|
| [Getting started](docs/getting-started.md) | Requirements, installation, minimal example, demo tour |
| [Format options](docs/format-options.md) | Every option, merge semantics, override recipes |
| [Authoring guide](docs/authoring-guide.md) | Slides, lists, callouts, columns, tables, code, math, notes |
| [Customization](docs/customization.md) | Palette, fonts, spacing, footer, theme override |
| [Troubleshooting](docs/troubleshooting.md) | Debugging, known issues, error catalog |

## Authoring tips

- One idea per slide. Split a dense slide into two.
- Keep bullet lists to three to five items.
- Use `#` for section pages and `##` for slides.
- Use `::: {.columns}` and `::: {.column}` for side-by-side content.
- Mark key terms with `[term]{.alert}` for bold terracotta text.
- Put speaker notes in `::: {.notes}` blocks. They do not appear on slides.
- For code you do not need to execute, use a fenced ```` ```python ```` block. Executable `{python}` cells require a Jupyter installation.

## Files

| File | Purpose |
|------|---------|
| `template.qmd` | Demo deck and starter file. Copy it to begin. |
| `_extensions/neutral/` | The format extension. Keep it next to your `.qmd`. |
| `_quarto.yml` | Optional project defaults. |
| `README.md` | This guide. |
