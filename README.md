# Neutral Beamer Template

A neutral, readable Quarto format extension for **self-contained PDF slide decks**. No logos, no branding, and no topic-specific styling. The deck must stand on its own: a reader with no presenter and no prior background should understand it from the PDF alone.

![Three slides from the demo deck: title, overview, and the simple table slide](docs/preview.png)

## What it is

A Quarto format extension that renders Beamer PDF slides with the Metropolis base theme, tuned for readability:

- **Neutral palette.** Off-white canvas, dark-gray text, teal structure, terracotta alerts.
- **Readable typography.** Source Sans for body and titles, Source Code Pro for monospace. Body text stays at 11pt with 1.5 line spacing. Nested lists do not shrink.
- **Low visual weight.** No navigation symbols, light-gray circle bullets, transparent blocks, minimal slide numbers.
- **Simple geometry.** 4:3 by default, 10mm side margins, one idea per slide.

## Requirements

- **Quarto** 1.4 or newer.
- **TeX Live** with `xelatex`, Metropolis, `fontspec`, `sourcesanspro`, and `sourcecodepro`.
- **Poppler utilities** (`pdftotext`, `pdfinfo`, `pdftoppm`) for validation and visual review.

No internet connection is needed at render time. Fonts use system lookup with Latin Modern fallbacks.

## Fresh-box setup

The repository and its agent workflow do not assume tools are installed. Check first:

```bash
command -v quarto xelatex pdftotext pdfinfo pdftoppm
quarto --version
```

On Debian/Ubuntu, install the rendering and validation stack:

```bash
sudo apt-get update
sudo apt-get install -y texlive-xetex texlive-latex-extra texlive-fonts-extra \
  texlive-latex-recommended texlive-theme-metropolis poppler-utils
```

Install Quarto from <https://quarto.org/docs/download/>. Verify every command before rendering.

## Quick start

With GitHub CLI:

```bash
gh repo clone harrizsb/presentation-template ~/Downloads/presentation-template
cd ~/Downloads/presentation-template
quarto render template.qmd
```

Without `gh`:

```bash
git clone https://github.com/harrizsb/presentation-template.git
cd presentation-template
quarto render template.qmd
```

The output is `template.pdf` in the same folder.

To use the format in another project, copy `_extensions/neutral/` beside the `.qmd` and set:

```yaml
---
title: "My talk"
format: neutral-beamer
---
```

## Authoring contract

The PDF is the only source of truth. The canonical deck and any deck generated from it must:

- contain no speaker notes, `.notes` blocks, `\\note{}`, `notes=show`, or `notes=only`;
- contain no greetings, welcome messages, thanks, or other pleasantries;
- address a reader with no prior background by defining terms before use and explaining visuals;
- teach one claim or concept per content slide; add slides instead of shrinking or packing;
- use direct, concise, fluent language that flows through reasoning and transitions;
- be rendered and visually inspected page by page before delivery.

Read [Authoring guide](docs/authoring-guide.md) and [Validation guide](docs/validation.md) before creating a deck. `template.qmd` demonstrates the available constructs and the intended density; its content is not a model of domain-specific narration.

## Options

The extension defaults are `theme: metropolis`, `aspectratio: 43`, `fontsize: 11pt`, `section-titles: true`, `numbersections: true`, `navigation: empty`, and `pdf-engine: xelatex`. Override options under `format.neutral-beamer`.

```yaml
format:
  neutral-beamer:
    aspectratio: 169
    fontsize: 11pt
```

The palette and fonts are fixed in `_extensions/neutral/header.tex`. `brand.yml` is not applied automatically to Beamer output.

## Documentation

| Guide | Contents |
|---|---|
| [Getting started](docs/getting-started.md) | Requirements and first render |
| [Format options](docs/format-options.md) | Supported options and overrides |
| [Authoring guide](docs/authoring-guide.md) | Self-contained, novice-first content rules and constructs |
| [Validation guide](docs/validation.md) | PDF text gates and page-by-page visual review |
| [Customization](docs/customization.md) | Palette, fonts, spacing, footer, theme override |
| [Troubleshooting](docs/troubleshooting.md) | Render failures and known issues |

## Files

- `template.qmd` — canonical notes-free demo and starter.
- `template.pdf` — rendered preview.
- `_extensions/neutral/` — format extension.
- `docs/` — documentation.
