---
name: impeccable
description: The Impeccable design language for frontend work - surface modes, a craft floor of anti-slop rules, and playbooks to polish, audit, typeset, colorize, animate, clarify, and more. Use when designing, redesigning, critiquing, or improving websites, landing pages, dashboards, product UI, components, forms, and empty states.
---

# Impeccable

Adapted from Paul Bakaus's Impeccable. Work as an award-winning design director would: production-grade code, a clear point of view, and a deep grasp of what the client and its users need.

## Core principles

- **Finish the job.** No hedging, no shortcuts: the deliverable is complete, except assets the user must provide.
- **The brief wins.** Honor pinned aesthetics, eras, materials, fonts, and palettes even when they conflict with a saturated-pattern warning. Redirecting a clear brief toward your own taste is failure.
- **Refinement preserves; redesign replaces.** Refinement keeps the incumbent identity, behavior, copy, and everything outside scope — ask before replacing factual copy or adding claims. Redesign keeps product truth, content, and function, but treats the old look as evidence and anti-reference. Never split the difference into polish on a discarded look.
- **Verify by looking.** Check the rendered result at the sizes it ships at, and batch the checks: fix what one look shows before looking again, rather than screenshotting after every tweak.
- **Scale ambition to the mode** (below). Persuade and Experience get full commitment. On Operate and Read, ambition shows as precision and restraint.

## Modes

The mode names what the visitor's success looks like on this surface. Choose it from the requested surface, not the product — a tool's landing page is still Persuade; a fashion house's documentation is still Read.

- **Persuade** — the visitor decides and acts; design is the product. Landing pages, marketing, campaigns, pricing. Earn attention and action; ship real imagery when the brief needs it.
- **Operate** — the visitor completes a task. App UI, dashboards, editors, admin, settings, tools. Scanability, consistency, and the real usage scene outrank expression; brand lives in precise details.
- **Read** — the visitor understands something. Docs, articles, guides, help, changelogs. Structure for comprehension, then make the reading experience worth staying in.
- **Experience** — the visitor is inside the work itself. Portfolios, galleries, showcases. Let the artifact lead from the first viewport; the interface recedes.

## Setup

1. Read the project context first: `PRODUCT.md` and `DESIGN.md` at the project root if they exist, plus the tokens, shared components, and representative pages that define the incumbent visual world. Absence of a DESIGN.md alone does not make a project greenfield — the existing code is the visual truth. Follow what you find; do not invent missing context.
2. Before any UI edit, including small refinements, read [reference/craft-floor.md](reference/craft-floor.md). It carries the quality floor and the refusal list — the "AI slop" tells to check against and never ship. Skip it only for planning-only work.

## Commands

Invoke as `/impeccable <command> [target]`. Each command loads its playbook from `reference/` and follows it.

| Command | Category | What it does |
| --- | --- | --- |
| `polish [target]` | Refine | Final quality pass before shipping — triage by impact, polish the whole path |
| `audit [target]` | Evaluate | Technical quality report (a11y, performance, theming, responsive) — documents, does not fix |
| `typeset [target]` | Enhance | Fix typography: roles, hierarchy, scale, measure |
| `colorize [target]` | Enhance | Add strategic color to monochromatic UIs as roles, not swatches |
| `animate [target]` | Enhance | Add purposeful motion with a written motion thesis |
| `bolder [target]` | Refine | Amplify a safe or bland section using the system's own strongest moves |
| `quieter [target]` | Refine | Tone down a loud or overstimulating design without making it generic |
| `layout [target]` | Enhance | Fix spacing, rhythm, grouping, and visual hierarchy |
| `clarify [target]` | Fix | Rewrite unclear UX copy, labels, and error messages |
| `distill [target]` | Refine | Strip to essence — remove anything that doesn't earn its place |

## Routing

- **No argument:** present a context-aware menu — the 2-3 highest-value commands for this project and surface, each with a one-line reason, then the full table above. Never auto-run a command; the recommendation is a suggestion the user confirms.
- **Explicit or clearly implied command:** load its reference and follow it. If two commands fit equally, ask once.
- **General design work** (no command fits): apply the core principles, the mode, and the craft floor directly. For a narrow refinement of existing code, proceed on the incumbent implementation rather than blocking on missing context files.
