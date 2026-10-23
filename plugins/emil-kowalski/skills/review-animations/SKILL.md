---
name: review-animations
description: Review animation and motion code against a strict craft bar - justified motion, responsive easing, sub-300ms UI, correct origins, interruptibility, GPU-only properties. Use when reviewing a diff or existing animations; approval is earned, not assumed.
---

# Reviewing Animations

A specialized review skill. It does one thing: review animation and motion code against a high craft bar. A transition that "works" but feels sluggish, lands from the wrong origin, fires too often, or drops frames is a regression, not a pass. Default to flagging.

The values below come from the `emil-design-eng` standards (curves: `--ease-out: cubic-bezier(0.23, 1, 0.32, 1)`, `--ease-in-out: cubic-bezier(0.77, 0, 0.175, 1)`, `--ease-drawer: cubic-bezier(0.32, 0.72, 0, 1)`; durations: press feedback 100-160ms, tooltips/popovers 125-200ms, dropdowns 150-250ms, modals/drawers 200-500ms).

## The ten non-negotiable standards

Every animation in the diff is measured against these. A violation is a finding.

1. **Justified motion.** Every animation answers "why does this animate?" — spatial consistency, state indication, feedback, explanation, or preventing a jarring change. "It looks cool" on a frequently-seen element is a block.
2. **Frequency-appropriate.** Keyboard-initiated and 100+/day actions get **no** animation. Tens/day gets reduced motion. Occasional gets standard. Rare/first-time can have delight.
3. **Responsive easing.** Entering/exiting elements use `ease-out` or a strong custom curve. `ease-in` on UI is a block. Built-in CSS easings are too weak; expect custom cubic-beziers.
4. **Sub-300ms UI.** Anything slower on a UI element needs justification or it's a finding.
5. **Origin & physical correctness.** Popovers/dropdowns/tooltips scale from their trigger, not center. Never animate from `scale(0)` — start from `scale(0.9-0.97)` + opacity. Modals exempt (they stay centered).
6. **Interruptibility.** Rapidly-triggered or gesture-driven motion (toasts, toggles, drags) uses CSS transitions or springs that retarget from current state — not keyframes that restart from zero.
7. **GPU-only properties.** Animate `transform` and `opacity` only. Animating `width`/`height`/`margin`/`padding`/`top`/`left`, or Motion `x`/`y`/`scale` shorthands under load, is a performance finding.
8. **Accessibility.** `prefers-reduced-motion` honored (gentler, not zero — keep opacity/color, drop movement). Hover animations gated behind `@media (hover: hover) and (pointer: fine)`.
9. **Asymmetric enter/exit.** Deliberate actions (a press, a hold) animate slower; system responses snap. Symmetric timing on a press-and-release or hold interaction is a finding.
10. **Cohesion.** Motion matches the component's personality and the product — playful can be bouncier, a dashboard stays crisp. When unsure whether motion feels right, the strongest move is often to delete it.

## Escalation triggers — flag on sight

- `transition: all`
- `scale(0)` or pure-fade entrances
- `ease-in` on UI elements
- `transform-origin: center` on a trigger-anchored popover/dropdown/tooltip
- Keyframes on toasts, toggles, or anything triggered rapidly
- Animating layout properties (`width`/`height`/`margin`/`padding`/`top`/`left`)
- Motion `x`/`y`/`scale` props on motion that runs while the page is busy
- Updating a CSS variable on a parent to drive a child transform (recalc storm)
- Missing `prefers-reduced-motion` handling on movement
- Ungated `:hover` motion
- Symmetric enter/exit timing on a press-and-release or hold interaction
- Everything-at-once entrance where a 30-80ms stagger belongs

## Remedial preference hierarchy

Propose fixes in this order — earlier moves over later ones:

1. **Delete the animation** (high-frequency / no purpose / keyboard-triggered).
2. **Reduce it** — shorter duration, smaller transform, fewer properties.
3. **Fix the easing** — `ease-in` → `ease-out`/custom curve.
4. **Fix the origin/physicality** — correct `transform-origin`; `scale(0)` → `scale(0.95)` + opacity.
5. **Make it interruptible** — keyframes → transitions; springs for gestures.
6. **Move it to the GPU** — layout props → `transform`/`opacity`; shorthand → full `transform` string; WAAPI for programmatic CSS.
7. **Asymmetric timing** — slow the deliberate phase, snap the response.
8. **Polish** — blur to mask crossfades, stagger for groups, `@starting-style` for entry, springs for "alive" elements.
9. **Accessibility & cohesion** — reduced-motion + hover gating; match the component's personality.

## Required output format

**Part 1 — findings table (required).** A single markdown table, one row per issue, citing `file:line` in the Why column where useful. Never a "Before: / After:" list.

| Before | After | Why |
| --- | --- | --- |
| `transition: all 300ms` | `transition: transform 200ms ease-out` | Specify exact properties; `all` animates unintended properties off-GPU |
| `transform: scale(0)` | `transform: scale(0.95); opacity: 0` | Nothing appears from nothing |

**Part 2 — verdict (required).** Group remaining commentary by impact tier, highest first, omitting empty tiers:

1. **Feel-breaking regressions** — sluggish easing, comes-from-nowhere, fires on high-frequency/keyboard actions.
2. **Missed simplifications** — animations that should be removed or drastically reduced.
3. **Performance** — non-GPU properties, dropped-frame risks, recalc storms.
4. **Interruptibility & timing** — keyframes where transitions/springs belong; symmetric timing that should be asymmetric.
5. **Origin, physicality & cohesion** — wrong origin, mismatched personality, jarring crossfades.
6. **Accessibility** — reduced-motion and pointer/hover gating.

Close with an explicit decision:

- **Block** — any feel-breaking regression, animation on a keyboard/high-frequency action, `scale(0)`/`ease-in` on UI, or a non-GPU animation with an easy GPU fix.
- **Approve** — no feel-breaking regressions, no motion that should be deleted, durations and easing within bounds, interruptibility handled, reduced-motion respected.

When a finding needs a precise value, use the exact curve, duration, or spring config from the standards above rather than approximating.
