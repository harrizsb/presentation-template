# Format Options

This page covers every option the `neutral-beamer` extension declares, how they merge with standard Quarto beamer options, and verified override recipes.

## Extension options

These options are declared in `_extension.yml` and set under `format.neutral-beamer` in your YAML front matter.

| Option | Default | Valid values | Effect |
|--------|---------|--------------|--------|
| `theme` | `metropolis` | Any Beamer theme name | Base Beamer theme. Metropolis options (progress bar, regular title format, transparent blocks, section-page progress bar) are applied conditionally in `header.tex`. A user theme override like `madrid` is safe: the guard detects Metropolis is not loaded and skips its options. |
| `aspectratio` | `43` | `43`, `169`, `1610`, `149`, `141`, `54`, `32` | Slide aspect ratio. Pass as a bare number, not a quoted string. `43` is 4:3 (default); `169` is 16:9 widescreen. |
| `fontsize` | `11pt` | `8pt` through `20pt` | Beamer body text font size. |
| `section-titles` | `true` | `true`, `false` | Render `#` headings as section pages. |
| `numbersections` | `true` | `true`, `false` | Number section pages. |
| `navigation` | `empty` | `empty`, `horizontal`, `vertical`, `fixed` | Beamer navigation symbols. `empty` hides them entirely. |
| `pdf-engine` | `xelatex` | `xelatex`, `lualatex`, `pdflatex` | LaTeX engine. Determines how fonts are loaded (see below). |

The extension also injects `include-in-header: header.tex` (palette, fonts, layout) and `include-before-body: before-body.tex`. These are internal plumbing; you do not need to set them.

## Standard Quarto beamer options

These are not declared by the extension but work alongside it. Set them at the same level under `format.neutral-beamer`.

| Option | Effect |
|--------|--------|
| `toc` | Add a table-of-contents slide after the title slide. |
| `slide-level` | Heading level that starts a new slide (default: `2`). The demo deck sets this explicitly. |
| `incremental` | Reveal list items one at a time. Set `true` globally or per-slide. |

Executable cells (` ```{python} ` with `#| echo`/`#| eval`) require a Jupyter kernel and are not exercised by the demo deck. They work with `neutral-beamer` if your environment has Jupyter installed.

## Merge semantics

Extension defaults from `_extension.yml` are injected first. User YAML under `format.neutral-beamer` overrides them. Quarto project-level defaults in `_quarto.yml` are applied last (project `format:` < document `format:`).

This means:

1. The extension sets `aspectratio: 43`, `fontsize: 11pt`, `theme: metropolis`, etc.
2. Your document YAML can override any of these.
3. Your project `_quarto.yml` can set format defaults that apply to every document in the project.

Standard Quarto beamer options (`toc`, `slide-level`, `incremental`) that you set are merged additively; they do not replace extension defaults.

## Verified override recipes

Each recipe below is safe to use as-is. Copy the YAML block into your document front matter.

### Widescreen (16:9)

```yaml
format:
  neutral-beamer:
    aspectratio: 169
```

Changes the slide geometry from 4:3 to 16:9. Valid values: `43`, `169`, `1610`, `149`, `141`, `54`, `32`. Pass as a bare number.

### Larger body text

```yaml
format:
  neutral-beamer:
    fontsize: 12pt
```

Increases body text from the default 11pt. Valid range: `8pt` to `20pt`.

### Switch base theme (safe, no clash)

```yaml
format:
  neutral-beamer:
    theme: madrid
```

Replaces Metropolis with Madrid. This is safe because `header.tex` guards its Metropolis-specific `\metroset` calls behind `\@ifpackageloaded{beamerthememetropolis}`. If Metropolis is not loaded, those options are skipped entirely. The neutral color palette and font settings still apply.

### pdflatex engine (bundled fonts)

```yaml
format:
  neutral-beamer:
    pdf-engine: pdflatex
```

Switches from XeLaTeX to pdfLaTeX. Font loading changes automatically: pdfLaTeX uses the `sourcesanspro` and `sourcecodepro` LaTeX packages, which bundle the fonts. XeLaTeX uses `fontspec` with system font lookup and Latin Modern fallbacks. Both paths produce equivalent output. No internet access is needed for either engine.

### Syntax highlight style

The extension does not set a highlight style, so pandoc's default syntax highlighting applies. You can override this with any Quarto-supported highlight style:

```yaml
format:
  neutral-beamer:
    highlight-style: monochrome
```

Other valid values include any style name from `pandoc --list-highlight-styles`.

### Combined recipe

A typical widescreen talk with larger text and a table of contents:

```yaml
---
title: "My Talk"
author: "Your Name"
date: today
format:
  neutral-beamer:
    aspectratio: 169
    fontsize: 12pt
    toc: true
---

## First slide

Content here.
```

## Options not exposed

The following are baked into `header.tex` and cannot be changed via YAML. Edit the `.tex` file directly if you need different values.

- Metropolis progress bar style (`progressbar=frametitle`)
- Title format (`regular` instead of `bold`)
- Transparent blocks (`block=transparent`)
- Section-page progress bar (`sectionpage=progressbar`)
- Color palette (background, text, structure, alert, gray)
- Font families (Source Sans 3, Source Code Pro)
- Line spacing (1.5)
- Text margins (10mm)
- Bullet style (light-gray circles)
- Slide-number footline (right-aligned, gray)
