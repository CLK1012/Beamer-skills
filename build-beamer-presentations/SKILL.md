---
name: build-beamer-presentations
description: Create, modify, or debug polished LaTeX Beamer presentations with reusable slide components. Use when Codex needs to build or adapt Beamer/ctexbeamer decks, including custom navigation bars with section titles and subsection dots, linked progress indicators, boxed text blocks, rounded tag blocks, equal-height horizontal block layouts, image/caption slides, and TikZ process, circular-arrow, or curved-gradient-arrow diagrams.
---

# Build Beamer Presentations

Use this skill to design Beamer decks from composable LaTeX elements rather than forcing every task into a single fixed template.

## Workflow

1. Inspect any existing `.tex` deck before editing. Preserve local theme choices and only replace the components the user asks to change.
2. Choose the required component references:
   - Navigation, section numbering, subsection dots, and hyperlinks: read `references/navigation.md`.
   - Boxed text blocks, rounded tag blocks, equal-height columns, and image/caption layouts: read `references/blocks-and-layouts.md`.
   - TikZ process diagrams, circular-arrow loops, and curved-arrow flow visuals: read `references/tikz-diagrams.md`.
   - Overflow prevention, table-of-contents layout, footer-safe zones, and palette consistency: read `references/layout-quality.md`.
3. Compile with XeLaTeX for Chinese Beamer decks:
   ```bash
   latexmk -xelatex -interaction=nonstopmode deck.tex
   ```
4. Validate visually, not only by log success. Render key pages:
   ```bash
   gs -q -dNOPAUSE -dBATCH -sDEVICE=pngalpha -r170 \
     -dFirstPage=PAGE -dLastPage=PAGE \
     -sOutputFile=/tmp/beamer_check.png deck.pdf
   ```
5. Scan logs and fix real issues:
   ```bash
   rg -n "Warning|Overfull|Underfull|Error|Undefined|Missing character" deck.log
   ```
   Treat `Overfull \hbox` and `Overfull \vbox` as layout failures when they correspond to visible block, page, or footer overlap.

## Design Defaults

- Prefer `ctexbeamer` with `xelatex` for Chinese decks.
- Keep the first screen as the actual slide content unless the user asks for a landing/cover-only experience.
- Use restrained colors: one main color, one accent, and semantic alert/example colors.
- Keep semantic colors within the same palette family unless contrast has a clear purpose. For a brick-red theme, choose nearby reds, rose, coral, amber, or muted brown rather than a sudden saturated green.
- Prefer left-right side-by-side text blocks for content pages when the material can be grouped into two or three comparable parts. Fall back to vertical stacked blocks only when the content is sequential, hierarchical, or too narrow for side-by-side reading.
- Keep block cards compact. Use equal-height minipages in side-by-side layouts to avoid ragged visual rhythm.
- Keep all visible content inside the slide body. Do not let blocks or text cover the lower-right footer/page label area.
- If dense content remains important after compression, split it into additional frames instead of forcing every detail onto one slide.
- For navigation, treat short section titles as "chapters" and subsection dots as "nodes/sections within the chapter"; do not count chapter title pages as dots.
- Use TikZ for reusable vector diagrams. Avoid raster screenshots for process diagrams unless the user provides a required image.
- Choose diagram styles by intent and follow the sample code closely:
  - Linear workflow, project stages, or method sequence: use the horizontal process flow.
  - Iteration, feedback, management loop, or quality improvement cycle: use the circular arrow loop.
  - Development path, capability growth, strategic progress, or research trajectory: use the curved gradient progress arrow.

## Implementation Notes

- If using `smoothbars`, set custom `frametitle` after `\useoutertheme`; otherwise Beamer may overwrite it.
- For subsection dot navigation, hook `\beamer@subsectionentry` and suppress `\slideentry` dots so only `\subsection` commands create progress dots.
- For rounded custom blocks with partial-width labels, use `tcolorbox` instead of hand-built `beamercolorbox` when rounded corners matter.
- For curved gradient arrows, approximate gradients with many short TikZ segments and vary `line width` across the loop.

## Expected Deliverables

When building or modifying a deck, return:

- The modified `.tex` path.
- The compiled `.pdf` path.
- A short note about pages/components changed.
- Any compile warnings that remain, or state that the log is clean.
