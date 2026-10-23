---
description: Run an Impeccable design pass - polish, audit, typeset, colorize, animate, bolder, quieter, layout, clarify, or distill
argument-hint: "[polish|audit|typeset|colorize|animate|bolder|quieter|layout|clarify|distill] [target]"
---

# /impeccable

Run one Impeccable design command against the current project or the target the user names.

Usage: `/impeccable polish src/pages/pricing.tsx`, `/impeccable typeset`, `/impeccable audit`.

1. Load the `impeccable` skill. Read the project context (PRODUCT.md / DESIGN.md if present, plus tokens and representative components) and identify the surface's mode (Persuade, Operate, Read, Experience).
2. If the user named a subcommand, load its playbook from the skill's `reference/` folder and follow it exactly. Available: `polish`, `audit`, `typeset`, `colorize`, `animate`, `bolder`, `quieter`, `layout`, `clarify`, `distill`.
3. If no subcommand was given, present the context-aware menu from the skill — 2-3 highest-value commands for this project with a one-line reason each, then the full list — and wait for the user's pick. Never auto-run a command.
4. Before any UI edit, read `reference/craft-floor.md` and hold the work to it: no nested cards, no gradient text, no gray text on colored surfaces, contrast and state checks on the built result.
5. Verify by looking at the rendered result (or the closest source evidence) at the sizes it ships at, and report what changed with specifics.

Note: commands run through `/impeccable <cmd>` on purpose — so `/impeccable audit` never collides with other plugins' standalone commands.
