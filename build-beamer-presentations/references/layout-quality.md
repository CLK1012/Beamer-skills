# Layout Quality, Overflow, TOC, And Palettes

Use this reference whenever a deck has dense text blocks, a table-of-contents page, a visible footer/page label, or a non-blue theme palette.

## Compile Warnings Are Not Enough

LaTeX often compiles even when text visibly leaves a block or overlaps another element. `Overfull \hbox` and `Overfull \vbox` usually appear as warnings, not hard errors. Treat these warnings as visual defects when they correspond to content blocks, columns, footers, navigation bars, captions, or page boundaries.

Always render the affected PDF page after compiling and inspect it. A clean-looking slide matters more than a merely successful compile.

## Measured Text Capacity

These estimates are based on `ctexbeamer` with XeLaTeX in a 16:9 Beamer deck where `\textwidth` measured about `398.34pt`. Measured CJK character widths and baselines were:

- `\small`: CJK char about `10pt`, baseline about `12pt`.
- `\footnotesize`: CJK char about `9pt`, baseline about `11pt`.
- `\scriptsize`: CJK char about `8pt`, baseline about `9.5pt`.

The table below is intentionally conservative for bullet-list blocks. It reserves room for itemize indentation, box padding, and visual breathing space. For plain paragraphs without bullets, allow about 2 to 4 extra CJK characters per line.

### Suggested CJK Characters Per Line

Use the block body width as a fraction of `\textwidth`.

| Body width | Typical use | `\small` | `\footnotesize` | `\scriptsize` |
|---:|---|---:|---:|---:|
| `0.31\textwidth` | 3-column block | 8 | 9 | 11 |
| `0.36\textwidth` | loose 3-column block | 10 | 11 | 12 |
| `0.42\textwidth` | image/text left block | 12 | 13 | 15 |
| `0.48\textwidth` | 2-column block | 13 | 15 | 17 |
| `0.54\textwidth` | wide 2-column block | 15 | 17 | 19 |
| `0.62\textwidth` | wide text block | 17 | 19 | 22 |
| `0.90\textwidth` | near-full-width body | 25 | 28 | 32 |

### Suggested Lines By Inner Text Height

Use this when the `minipage` height is the actual text body height inside a block.

| Inner text height | `\small` | `\footnotesize` | `\scriptsize` |
|---:|---:|---:|---:|
| `2.0cm` | 4 | 4 | 5 |
| `2.5cm` | 5 | 5 | 6 |
| `3.0cm` | 6 | 6 | 7 |
| `3.5cm` | 7 | 7 | 9 |
| `4.0cm` | 8 | 9 | 10 |

If the height is the outer block height including the colored title bar, subtract about `0.75cm` before using the table. For example, a `3.0cm` outer block leaves roughly `2.25cm` of body text, so plan for about 4 to 5 lines, not 6 to 7.

### Capacity Rule Of Thumb

For each block, estimate:

```text
capacity ~= suggested characters per line * suggested lines
```

Use no more than about 80 percent of that capacity in the first draft. Example: a `0.31\textwidth` three-column block with `\footnotesize` and `3.0cm` inner height supports about `9 * 6 = 54` CJK characters in bullet text; keep the first draft around 40 to 45 CJK characters, then render and inspect.

## Overflow Decision Rules

- If a block, box, or text region exceeds the slide page, first reduce or compress the content and keep the box inside the page.
- Before laying out dense slides, estimate each block's capacity from the tables above and keep the first draft below about 80 percent of that capacity.
- If content is too verbose for a block, shorten bullets before shrinking the font. Prefer noun phrases and short clinical/research terms over full sentences.
- If the overflowing details are important and cannot be responsibly removed, split the material into more frames instead of over-compressing one slide. Create additional `\subsection` nodes when the new pages should appear as progress dots.
- If the content still does not fit, reduce local font size one step at a time: `\small`, then `\footnotesize`, then `\scriptsize`. Avoid going smaller than `\scriptsize` for normal reading slides.
- If a box is too small but the page has room, increase the box height or width. For parallel boxes, set all boxes to the height required by the longest box and let shorter boxes keep whitespace.
- If several parallel boxes have different text amounts, do not let one box protrude downward. Use the longest-content box as the shared minipage height and leave empty space in shorter boxes.
- Do not solve overflow by letting text run into slide margins, navigation bars, captions, or footers.

## Footer-Safe Zone

Many decks place a section label and page number in the lower-right corner. Reserve a footer-safe zone even when the template does not draw a visible boundary.

Use these habits:

- Keep block bottoms at least `0.45cm` above the bottom edge when a footer/page label exists.
- Keep text and captions away from the lower-right corner; do not place notes, legends, or long captions there.
- For dense content pages, prefer columns with bounded heights rather than filling all vertical space.
- After rendering, explicitly check that no bullet, caption, or block background covers the footer label.

Example bounded block body:

```tex
\begin{minipage}[t][3.0cm][t]{\linewidth}
  \footnotesize
  \begin{itemize}
    \item 精简后的第一条要点。
    \item 精简后的第二条要点。
    \item 精简后的第三条要点。
  \end{itemize}
\end{minipage}
```

Increase `3.0cm` only if the resulting box remains above the footer-safe zone.

## Dense Box Text

For content such as diagnosis, symptoms, treatment, or research findings:

- Limit each box to three to five bullets.
- Keep each bullet to one or two short lines.
- Move secondary details into a smaller note below the box only if the note does not enter the footer-safe zone.
- Prefer bold keywords plus short explanations, for example `\textbf{STEMI}: ST 段抬高型心梗`.
- Avoid unbreakable long strings in CJK slides; insert spaces around long Latin terms or use shorter abbreviations.

## Table Of Contents Layout

Do not allow the table of contents to pile up on the left when there are many sections. Prefer a balanced three-column layout for reusable templates and decks with three or more major chapters.

Pattern:

```tex
\begin{frame}{目录}
  \begin{columns}[T,onlytextwidth]
    \begin{column}{0.31\textwidth}
      \tableofcontents[sections={1-2}]
    \end{column}
    \begin{column}{0.31\textwidth}
      \tableofcontents[sections={3-4}]
    \end{column}
    \begin{column}{0.31\textwidth}
      \tableofcontents[sections={5-6}]
    \end{column}
  \end{columns}
\end{frame}
```

Adjust the section ranges to balance visual weight, not merely section count. If the deck has exactly three major chapters, put one chapter per column. If it has five chapters, use `1-2`, `3-4`, `5`.

## Palette Consistency

Select colors as a coherent family. Avoid using a saturated unrelated hue for one block just because it is semantically "example" or "success".

Use one of these approaches:

- Blue theme: navy, medium blue, cyan-blue, muted teal.
- Brick-red theme: dark red, brick red, coral, rose, muted amber.
- Purple theme: deep purple, violet, plum, muted magenta.
- Neutral academic theme: charcoal, slate, steel blue, muted gray.

For a brick-red deck, prefer:

```tex
\definecolor{maincolor}{RGB}{143,0,0}
\definecolor{accentcolor}{RGB}{190,48,35}
\definecolor{alertcolor}{RGB}{165,28,28}
\definecolor{examplecolor}{RGB}{205,92,60}
\definecolor{softaccent}{RGB}{217,139,88}
```

Avoid dropping in bright green unless the whole palette intentionally includes green as a secondary system color and it appears elsewhere too.
