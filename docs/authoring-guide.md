# Authoring guide

This guide defines the content contract for `neutral-beamer`. The PDF is the only source of truth: do not rely on speaker notes or an accompanying talk.

## Before writing

Create a brief with the topic, purpose, audience, desired takeaway, intended duration, and evidence/source boundaries. Always write for a reader with no prior background. A brief may identify a narrower audience, but it never removes the requirement to define terms and explain visuals.

Write as a domain-fluent presenter explaining one useful idea to an intelligent outsider. Do not perform expertise. Make the idea understandable.

## Language contract

- Start with content. Do not greet, welcome, thank, congratulate, or add social filler.
- Use direct, concrete language. Avoid hedging and report-speak.
- Keep sentences concise but connect them with reasoning: `because`, `so`, `however`, and `as a result`.
- Avoid deictic phrases such as "as you can see here". A cold reader may not know which page or visual you mean.
- End with a conclusion or consequence, never a courtesy.

## Novice-first contract

- Expand every acronym on first use.
- Define each technical term before it is needed.
- Explain what every figure, table, equation, and code sample shows and why it matters.
- Define every symbol in an equation.
- Prefer a concrete example before an abstraction.
- Avoid forward references such as "we will see later".

## Density and pacing

- One claim or concept per content slide.
- If a slide needs more explanation, add a slide rather than shrinking text or adding bullets.
- Keep lists to three to five items.
- Keep tables small; split wide or tall tables.
- Keep code short and explain it in plain language.
- Preserve generous whitespace. The template's 11pt body and 1.5 line spacing are a limit, not a challenge.

## Structure

- A level-2 heading (`##`) starts a content slide.
- A level-1 heading (`#`) starts a section page.
- A slide title should state the claim, not merely name a topic.

```markdown
## Newton's second law connects force to acceleration

Force equals mass times acceleration: F = ma.

A heavier shopping cart accelerates less when the same force pushes it.
```

## Supported constructs

Keep the extension beside the `.qmd`; Quarto resolves it relative to the document.

### Lists

```markdown
- State the main idea
- Add supporting detail
- Close the set
```

### Callouts

Callouts render as transparent blocks with teal titles. Use them for a definition or takeaway, not for packing extra content.

```markdown
::: {.callout-note}
## Definition

A short explanation that stands on its own.
:::
```

### Columns

Use two balanced columns for a comparison or text beside a visual. Do not use columns to fit twice as much text on one slide.

```markdown
::: {.columns}
::: {.column width="48%"}
**One side**

Short content.
:::
::: {.column width="48%"}
**Other side**

Short content.
:::
:::
```

### Tables

Use a small Markdown table with a caption. Split complex tables across slides.

```markdown
| Method | Result |
|---|---|
| A | 0.82 |
| B | 0.91 |

: A small comparison
```

### Code and math

Use fenced code when showing code; do not require a kernel for a static example. Explain what the code does. Use math only when it adds understanding, and define symbols in plain language.

## Forbidden constructs for self-contained PDF decks

- Speaker notes: `.notes`, raw `\\note{}`, `notes=show`, `notes=only`.
- Pleasantries and social filler.
- Overlays, fragments, `\\pause`, or `incremental: true`; these can produce partial PDF pages.
- Footnotes and wide tables that compete with the frame's limited vertical space.
- Unexplained jargon, acronyms, figures, equations, or code.

## Pre-render checklist

Before rendering, answer yes to every item:

- Does each slide make one claim?
- Can a cold reader understand the slide without a presenter?
- Is every new term defined before use?
- Is the language direct, concise, and natural when read aloud?
- Are pleasantries absent?
- Are notes and overlays absent?
- Would splitting improve the slide? If yes, split it now.

Then follow [Validation guide](validation.md). A successful render is not sufficient; inspect the PDF pages.
