# Distill

Strip a design to its essence. Remove anything that doesn't earn its place: redundant elements, repeated information, decorative noise, cosmetic complexity.

## Assess the current state

Analyze what makes the design feel complex or cluttered:

- **Too many elements:** competing buttons, redundant information, visual clutter.
- **Excessive variation:** too many colors, fonts, sizes, styles without purpose.
- **Information overload:** everything visible at once, no progressive disclosure.
- **Visual noise:** unnecessary borders, shadows, backgrounds, decorations.
- **Confusing hierarchy:** unclear what matters most.
- **Feature creep:** too many options, actions, or paths forward.

Then find the essence: What's the primary user goal? (There should be ONE.) What's actually necessary vs. nice-to-have? What can be removed, hidden, or combined? What's the 20% that delivers 80% of the value?

If any of these are unclear from the codebase, do not guess — ask.

Simplicity is not about removing features. It's about removing obstacles between users and their goals. Every element should justify its existence.

## Plan the simplification

- **Core purpose:** What's the ONE thing this should accomplish?
- **Essential elements:** What's truly necessary to achieve that purpose?
- **Progressive disclosure:** What can be hidden until needed?
- **Consolidation opportunities:** What can be combined or integrated?

Expect to say no to good ideas to make room for great execution.

## Simplify

**Information architecture** — Remove secondary actions, optional features, and redundant information. Hide complexity behind clear entry points (accordions, step-through flows). Merge similar buttons, consolidate forms, group related content. One primary action, few secondary actions, everything else tertiary or hidden. If it's said elsewhere, don't repeat it here.

**Visual simplification** — Reduce the palette to 1-2 colors plus neutrals, not 5-7. Limit typography to one family, 3-4 sizes, 2-3 weights. Eliminate borders, shadows, and backgrounds that don't serve hierarchy or function. Flatten structure: reduce nesting, remove unnecessary containers — never nest cards inside cards. Remove cards that exist only as layout habit; use spacing and alignment instead.

**Interaction** — Cut steps between the user and the goal. Replace multi-control patterns with the single obvious control. Remove confirmations where undo is safe. Keep the primary path free of optional branches.

## Verify

- The ONE primary goal is now unmistakable.
- Everything removed was genuinely dispensable — no capability silently lost.
- The result is simpler *and* still unmistakably this product, not a generic skeleton.

When the essence holds, finish with `/impeccable polish`.
