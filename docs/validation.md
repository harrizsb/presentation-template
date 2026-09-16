# Validation guide

The rendered PDF is the only source of truth. A green render is not evidence. Run text and visual checks after every render, then fix and rerun until all pass.

## Required tools

Text and page checks need:

- `pdftotext`
- `pdfinfo`
- `pdftoppm`

On Debian/Ubuntu, these come from `poppler-utils`. Check them before validation:

```bash
command -v pdftotext pdfinfo pdftoppm
```

If they are missing, install them and rerun validation. Do not declare a deck correct without these checks.

## Text gates

Run `pdftotext deck.pdf -` and inspect the extracted text:

1. **No speaker notes.** Search for strings known to appear only in notes; none may appear in the PDF.
2. **No pleasantries.** Search for `welcome`, `thank you`, `thanks`, `good morning`, `good afternoon`, `let's get started`, `let's begin`, `great question`, `enjoy`; all must be absent.
2b. **Direct register, no filler.** Search for hedged or padded language: `perhaps`, `maybe`, `arguably`, `somewhat`, `quite`, `rather`, `essentially`, `basically`, `in order to`, `utilize`, `leverage`, `delve`, `moreover`, `facilitate`. Any hit is a failure.
3. **No clipping.** Check that the final expected line of every content slide appears in extracted text. Beamer can clip vertical overflow silently.
4. **Page count.** Compare `pdfinfo` against the expected slide count.
5. **No unresolved placeholders.** Search for `TODO`, `TBD`, `XXX`, `lorem`, and any copied starter text that must be replaced.

## Visual gates

Render each PDF page to an image and inspect every page:

```bash
pdftoppm -png -r 100 deck.pdf page
```

Inspect all generated images. A deck passes only when every page shows:

- no text or visuals clipped at the bottom or sides;
- one clear idea with comfortable whitespace;
- correct layout, balanced columns, captions attached to visuals;
- palette and typography as intended;
- a claim a cold reader can understand.

## Decision rule

Any failed gate means the deck is not done. Edit the QMD, rerender, and rerun all checks. Report final status as the gate results, not the render log.
