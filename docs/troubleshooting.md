# Troubleshooting

## Debugging workflow

When a render does not produce the expected PDF, or the PDF differs from the rendered output:

1. Add keep-tex: true under format.neutral-beamer in your .qmd YAML. This preserves the intermediate .tex file for inspection.
2. Render with quarto render . from the directory containing the .qmd and the _extensions/neutral/ folder.
3. Inspect the generated .tex file to verify that the correct \definecolor, \setbeamercolor, and other LaTeX commands are present.
4. If the .tex looks correct but the PDF is wrong, check for pandoc or LaTeX error messages in the terminal output.

Do not use the --keep-tex CLI flag - pandoc rejects it for this format. The only way to keep the .tex is via the YAML option.

## Known-good expectation mismatches

### Section pages are unnumbered but still counted

Section pages (created with # headings) do not display a slide number on the page itself, but they are still counted as frames. The slide after a section page shows a number that may look "off by one" relative to the PDF page index - this is expected behavior. The section page is a frame without a displayed number, so the visible slide number on the next frame starts counting from that offset.

### Callouts are plain transparent blocks on beamer

::: {.callout-note}, ::: {.callout-tip}, and similar Quarto callout blocks render as transparent blocks with teal titles in Beamer. They do not use the icons, background colors, or styling that the same callouts receive in HTML/Quarto output. This is a fundamental limitation of the Beamer backend; the transparent block appearance is the intended design.

### First-render transient 'compilation failed'

The first render of a deck occasionally fails with a "compilation failed" error. This is a transient issue - rerendering the same .qmd succeeds. If the error persists on the second render, investigate the error catalog below.

## Error catalog

### format neutral-beamer not found

Symptom: Quarto cannot find the neutral-beamer format, or renders without extension features.

Cause: Rendering from a directory that does not contain the _extensions/neutral/ folder next to the .qmd file. Quarto resolves extensions relative to the document's directory, not the current working directory.

Fix: Always render from the directory containing both the .qmd and the _extensions/neutral/ folder. For example:

```bash
quarto render template.qmd
```

run from /home/butler/presentation-template/. Rendering from /tmp or another directory without the extension folder will not load the format.

### Option clash for package beamerthememetropolis

Symptom: LaTeX error: "Option clash for package beamerthememetropolis."

Cause: The user adds \usetheme via include-in-header in the .qmd YAML, which causes Beamer to load the metropolis theme (or another theme) a second time. The guard in header.tex (\@ifpackageloaded{beamerthememetropolis}) only prevents the \metroset options from being applied twice, but a second \usetheme command still triggers an Option clash.

Fix: Use the theme: YAML option to change the base theme instead of adding \usetheme via include-in-header. For example, to use the Madrid theme:

```yaml
format:
  neutral-beamer:
    theme: madrid
```

This passes the theme to Quarto, which loads it once before header.tex is included. The guard then simply skips the \metroset call - no clash.

### Missing fonts (XeLaTeX fallback / pdfLaTeX bundled)

Symptom: Missing font warnings or fallback font substitution in the PDF.

Cause: The chosen pdf-engine does not have the expected fonts installed or available.

Fix:
- XeLaTeX (pdf-engine: xelatex): The fontspec fallback engages automatically. If Source Sans 3 or Source Code Pro is not installed, the code falls back to Latin Modern Sans or Latin Modern Mono respectively. No action needed.
- pdfLaTeX (pdf-engine: pdflatex): The sourcesanspro and sourcecodepro packages bundle the fonts. If you see missing font errors, ensure a full TeX Live installation is used, as the bundled packages require specific TFM/VF files that may not be present in minimal TeX installations. Consider switching to xelatex if fonts are unavailable.

### 4:3 vs 16:9 output size questions

Symptom: The output PDF has different dimensions than expected when switching aspect ratios.

Cause: The aspectratio option controls the paper size and slide dimensions via Beamer's \documentclass[aspectratio=...]{article} equivalent. Valid values and their resulting dimensions:

| Value | Aspect ratio |
|-------|-------------|
| 43    | 4:3 (default) |
| 169   | 16:9 widescreen |
| 1610  | 16:10 |
| 149   | 14:9 |
| 141   | 1.41:1 |
| 54    | 5:4 |
| 32    | 3:2 |

Changing aspectratio from the default 43 to 169 will produce a wider PDF. The physical dimensions also depend on the chosen paper size (default A4). To target a specific output size, set both aspectratio and geometry or use the paperwidth/paperheight variables if needed.

---

Verification checklist: After applying any fix, re-render with quarto render . from the document directory and verify the output PDF matches the expected structure (15 pages for the demo deck, correct slide numbers, palette colors applied, etc.).