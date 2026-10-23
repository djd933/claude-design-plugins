---
name: improve-animations
description: Audit all animation and motion code in a codebase and produce a prioritized plan of self-contained fixes. Read-only - it plans improvements, it does not apply them. Use when asked to improve the animations, audit the motion, or make an app feel better.
---

# Improving Animations

Survey a codebase's motion the way a senior motion advisor would, then hand back a prioritized audit and implementation plans that another agent (or a cheaper model) can execute without re-deriving the reasoning. This skill is read-only on source code: it plans, it does not apply fixes. For a single diff, use `review-animations`; for building one animation, use `animate`.

## Hard rules

- Every finding cites `file:line` evidence. No vibes-based findings.
- Every planned fix is self-contained: exact current code, exact replacement values (curves, durations, spring configs from the `emil-design-eng` standards), and the standard it satisfies.
- Prioritize by user impact, not by how easy the fix is. Feel-breaking issues on high-frequency surfaces outrank polish on rare ones.
- Respect the frequency framework: the best plan for a high-frequency surface is often "remove the animation," and that is a valid top-priority item.

## Workflow

### 1. Recon

Map the codebase's motion before judging it:

- Inventory the motion code: CSS transitions/keyframes, `@starting-style`, WAAPI, Motion/Framer Motion, springs, gesture handlers. Note which libraries and versions are in play.
- Extract the existing design tokens for easing and duration, if any. Plans must extend these tokens, not invent a parallel system.
- Note the product's personality — a crisp dashboard earns fewer, subtler changes than a playful consumer app.

### 2. Audit

Sweep the inventory against the ten standards (see `review-animations`):

1. Justified motion — every animation has a named purpose.
2. Frequency-appropriate — nothing animates on keyboard or 100+/day actions.
3. Responsive easing — no `ease-in` on UI; strong custom curves, not weak built-ins.
4. Sub-300ms UI durations, per-element budgets respected.
5. Correct origins; no `scale(0)` entrances.
6. Interruptible where rapidly triggered (transitions/springs, not keyframes).
7. GPU-only properties (`transform`, `opacity`); no layout-property animation; no Motion shorthand under load.
8. `prefers-reduced-motion` honored; hover gated by `(hover: hover) and (pointer: fine)`.
9. Asymmetric enter/exit timing on deliberate vs. system-driven motion.
10. Cohesion with the component's personality and the rest of the product.

Also sweep for: `transition: all`, CSS variables on parents driving child transforms, everything-at-once group entrances that should stagger 30-80ms, tooltips that re-delay on every hover, and crossfades that need a blur bridge.

### 3. Plan

Produce the report:

- **Summary table** — one row per finding: location, issue, standard violated, priority (P0 feel-breaking / P1 noticeable / P2 polish), effort (S/M/L).
- **Implementation plans** — for each P0 and P1, a self-contained plan: the current code, the replacement, and one line on why it will feel different. A plan is complete when an executor needs no further design decisions to apply it.
- **Deletions first** — call out animations that should be removed entirely as their own section; removal is an improvement, not a gap in the plan.
- **What not to touch** — motion that already meets the bar, so the executor doesn't churn it.

## Tone

Direct and specific. If the codebase's motion is already good, say so and keep the plan short — an honest "little to fix" is a valid audit outcome.
