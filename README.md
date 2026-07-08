# Beamer Skills

Reusable agent skills for creating polished LaTeX Beamer presentations.

## Included Skill

- `build-beamer-presentations`: custom Beamer navigation bars, section/subsection progress dots, reusable text blocks, equal-height side-by-side layouts, image/caption slides, and TikZ process diagrams.

## Style Defaults

Use `$build-beamer-presentations` when building or adapting a Beamer deck. For ordinary content pages, prefer left-right side-by-side text blocks and keep parallel blocks equal height. Use vertical stacked blocks only when the content is sequential or too dense for columns.

For diagram pages, choose the sample by intent and follow the reference code closely:

- Linear workflow or project stages: horizontal process flow.
- Iteration or feedback cycle: circular arrow loop.
- Development path, capability growth, or strategic progress: curved gradient progress arrow.

Also enforce visual quality: fix visible overflow even when LaTeX only reports a warning, reserve the lower-right footer/page-number area, balance table-of-contents pages in columns, and keep block colors within one coherent palette family.

## Installation

Copy the `build-beamer-presentations` folder into the target agent's skills directory.

- Codex / Antigravity: `~/.codex/skills/build-beamer-presentations`
- Claude Code: `~/.claude/skills/build-beamer-presentations`
