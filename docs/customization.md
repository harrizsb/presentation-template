# Customization

## Palette

| Element                | Color               | Hex       | definecolor block                      |
|------------------------|---------------------|-----------|----------------------------------------|
| Background             | Off-white           | #F7F7F5   | \definecolor{neutralBg}{HTML}{F7F7F5}  |
| Body text              | Dark gray           | #2C2C2C   | \definecolor{neutralFg}{HTML}{2C2C2C}  |
| Structure              | Teal                | #006B8C   | \definecolor{neutralStructure}{HTML}{006B8C} |
| Alerted text           | Terracotta          | #B84300   | \definecolor{neutralAlert}{HTML}{B84300} |
| Bullets, rules, numbers| Light gray          | #9E9E9E   | \definecolor{neutralGray}{HTML}{9E9E9E} |

WCAG contrast ratios on off-white background #F7F7F5:
- Body text: 13.0:1 AAA
- Structure: 5.6:1 AA normal / 5.6:1 AAA large
- Alerted text: 5.1:1 AA normal / 5.1:1 AAA large

To change a color safely, edit the \definecolor block in _extensions/neutral/header.tex. After changing a color, verify the new hex value meets the desired WCAG level against #F7F7F5. The current palette already satisfies the stated ratios; deviating from these hex values may reduce contrast.

## Typography

### XeLaTeX (pdf-engine: xelatex)

fontspec loads Source Sans 3 for text and Source Code Pro for monospace. Latin Modern Sans and Latin Modern Mono serve as fallbacks when the named fonts are not installed on the system. The fallback engages automatically if a font is absent.

To swap fonts in the xelatex branch, replace \setsansfont{Source Sans 3} and \setmonofont{Source Code Pro} in _extensions/neutral/header.tex with the desired font names. The Latin Modern fallbacks will take over if the new fonts are unavailable.

### pdfLaTeX (pdf-engine: pdflatex)

The sourcesanspro and sourcecodepro packages bundle the fonts. No internet fonts are used; all fonts ship with TeX Live.

To swap fonts in the pdflatex branch, replace \usepackage[default]{sourcesanspro} and \usepackage{sourcecodepro} in _extensions/neutral/header.tex with equivalent LaTeX font packages. The bundled packages are fixed; switching to xelatex is the recommended path for custom fonts.

## Spacing and margins

- Line spacing: \setstretch{1.5} (loaded viausepackage{setspace})
- Text margins: 10mm left and right via \setbeamersize{text margin left=10mm, text margin right=10mm}

To change line spacing, modify \setstretch in _extensions/neutral/header.tex. To change side margins, modify the \setbeamersize values. The top and bottom margins are not explicitly set; they follow the Beamer default for the chosen paper size.

## Footer customization

The footline template displays slide numbers right-aligned in light gray on the off-white background:

\setbeamercolor{footline}{fg=neutralGray, bg=neutralBg}
\setbeamertemplate{footline}{%
  \begin{beamercolorbox}[wd=\paperwidth,ht=2.5ex,dp=1.5ex,%
    leftskip=10mm,rightskip=10mm]{footline}%
    \hfill\color{neutralGray}\normalsize\insertframenumber\hspace*{2mm}%
  \end{beamercolorbox}%
}

- neutralGray (#9E9E9E) is the text color
- neutralBg (#F7F7F5) is the background
- leftskip=10mm,rightskip=10mm provides the side padding
- \insertframenumber renders the slide number

To customize, redefine the footline template or change the setbeamercolor{footline} values. Keep leftskip=10mm,rightskip=10mm unless you intend to alter the side padding.

## Theme override

The extension defaults to theme: metropolis in _extension.yml. header.tex applies metropolis options via a guarded block:

\makeatletter
\@ifpackageloaded{beamerthememetropolis}{%
  \metroset{progressbar=frametitle, titleformat=regular,
            block=transparent, sectionpage=progressbar}%
}{}
\makeatother

This guard checks if beamerthememetropolis is already loaded. If you override the theme via the YAML format.neutral-beamer theme: option (e.g., theme: madrid), the guard simply skips the \metroset call, and no Option clash occurs. The base theme is still loaded by Beamer; only the specific metropolis options are conditional.

Do not add \usetheme via include-in-header, as that would load a second theme and cause an Option clash. Use the theme: YAML option instead.

## highlight-style option

The highlight-style option controls the shading of fenced code blocks. Set it under format.neutral-beamer in your .qmd YAML:

```yaml
format:
  neutral-beamer:
    highlight-style: tango
```

Valid Pygments styles include: tango, monokai, boring, native, friendly, vs, autumn, bw, fruity, algol, algol_nu, emacs, espresso, kate, laTeX, literature, paraiso-light, pastie, rainbow_dash, solarized light, solarized dark, stataq, statau, trac, vim, vs.

## aspectratio

Set the aspectratio under format.neutral-beamer in YAML. Valid values: 43 (default, 4:3), 169, 1610, 149, 141, 54, 32.

Example for widescreen:

```yaml
format:
  neutral-beamer:
    aspectratio: 169
```

The default is 43 (4:3). The template.qmd demonstrates aspectratio: 169 when uncommented.

## brand.yml not applied on beamer path

brand.yml is not auto-applied to Beamer output in this template. Quarto applies brand.yml to HTML formats, but the Beamer path does not read it. Colors and fonts are set in the extension's LaTeX files (_extensions/neutral/header.tex). If you need different colors or fonts, edit header.tex directly - it is the single source of truth for the Beamer path.

---

Changing any of the above: edit the corresponding elements in _extensions/neutral/header.tex. All palette colors, typography commands, layout dimensions, and footer settings are defined in that single file.