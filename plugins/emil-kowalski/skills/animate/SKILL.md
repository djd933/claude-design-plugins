---
name: animate
description: Build an animation from scratch with the right decisions in order - whether it should exist, its purpose, tool, properties, curve, duration, interruption, and exit. Use when asked to animate something, add motion, or build a transition.
---

# Building Animations

A construction skill: turn a request for motion into an implementation that would survive a strict review (see `review-animations`). Two failure modes, and the first is worse:

1. **Animating something that shouldn't animate.** The gate below exists to produce zero lines of code sometimes. That's a success, not a dodge.
2. **Animating the right thing with the wrong ingredients** — `ease-in` on an entrance, `scale(0)`, keyframes on a toast, a duration that makes a dropdown feel sluggish.

Never present motion options as a menu. Make the call, state the reasoning in one line, write the code.

## Hard rules

- Run the sequence in order. Steps 1 and 2 gate everything.
- No approximated values. Every curve, duration, and spring config comes from the tables below — never invent a `cubic-bezier` because it looks familiar.
- Extend the codebase's existing tokens (`--ease-out`, duration scales) instead of forking a parallel system.
- Reduced motion and hover gating ship with the animation, not as a follow-up.
- Cheapest tool that works. Don't install a motion library for a fade.

## The build sequence

### 1. Should this animate at all?

| Frequency | Decision |
| --- | --- |
| 100+ times/day (keyboard shortcuts, command palette) | **No animation. Ever.** Stop here. |
| Tens of times/day (hover, list navigation) | Near-imperceptible only — fast and subtle, or nothing |
| Occasional (modals, drawers, toasts) | Standard animation |
| Rare / first-time (onboarding, success, celebration) | The delight budget lives here |

Keyboard-initiated actions are a disqualifier, not a judgment call. If the request fails this gate, say so plainly, write no animation, and offer the non-motion alternative (instant state change, static affordance).

### 2. What is the purpose?

Name it in one word before continuing: **feedback**, **spatial consistency**, **state indication**, **preventing a jarring change**, **explanation** (marketing/onboarding only), or **delight** (rare/first-time tier only). Can't name it? Don't build it. Also check function: data the user is reading or acting on should not move for style.

### 3. Pick the tool — cheapest that works

| Need | Tool |
| --- | --- |
| Hover, press, color, a state toggle via class/attribute | CSS transition |
| Entry animation on mount, no JS state | CSS `@starting-style` |
| Predetermined motion that must stay smooth while the page loads | CSS animation (off main thread) |
| Programmatic control with CSS performance, no library | WAAPI (`element.animate()`) |
| Springs, layout animations, exit animations, gesture-driven values | Motion (motion.dev) |

If the task needs a *component* rather than an animation (toast, drawer, command menu, dropdown), reach for a maintained library component instead of hand-rolling one with no focus management.

### 4. Pick the properties

- `transform` and `opacity` only. `clip-path` is the sanctioned extra; `height` is tolerated only for accordions.
- Never `scale(0)`. Start from `scale(0.9-0.97)` + `opacity: 0`.
- `transform-origin` at the trigger for popovers, dropdowns, menus, tooltips (`var(--transform-origin)` in Base UI). Modals exempt.
- Percentages in `translate()` are relative to the element's own size — prefer them over hardcoded pixels.
- In Motion, animate the full `transform` string; `x`/`y`/`scale` shorthands are not hardware-accelerated and drop frames under load.
- Never drive a child's transform from a CSS variable set on the parent (recalc storm). Set `transform` on the element directly.

### 5. Easing and duration — or a spring

| Situation | Easing |
| --- | --- |
| Entering or exiting | `ease-out` |
| Moving / morphing on screen | `ease-in-out` |
| Hover / color change | `ease` |
| Constant motion (marquee, progress) | `linear` |
| Default | `ease-out` |

Never `ease-in` on UI. Built-in easings are too weak:

```css
--ease-out: cubic-bezier(0.23, 1, 0.32, 1);        /* strong ease-out for UI */
--ease-in-out: cubic-bezier(0.77, 0, 0.175, 1);    /* strong ease-in-out for movement */
--ease-drawer: cubic-bezier(0.32, 0.72, 0, 1);     /* iOS-like drawer curve */
```

| Element | Duration |
| --- | --- |
| Button press feedback | 100-160ms |
| Tooltips, small popovers | 125-200ms |
| Dropdowns, selects | 150-250ms |
| Modals, drawers | 200-500ms |
| Marketing / explanatory | Can be longer |

UI animations stay under 300ms. Reach for a spring instead when the motion is a drag with momentum, an element that should feel alive, an interruptible gesture, or decorative mouse-tracking: `{ type: "spring", duration: 0.5, bounce: 0.2 }` (Apple-style) or `{ type: "spring", mass: 1, stiffness: 100, damping: 10 }`. Bounce 0.1-0.3, and avoid it in most UI.

### 6. Interruption and exit

- Transitions, not keyframes, for anything triggered rapidly — transitions retarget from the current value, keyframes restart from zero.
- Springs for gestures, because they carry velocity through an interruption.
- Exit the way it entered. A toast that slides in from the bottom leaves through the bottom.
- Asymmetric timing where the user is deciding: slow on the deliberate phase (hold-to-confirm: 2s linear), snappy on the system response (release: 200ms ease-out).

### 7. Reduced motion and pointer gating

Ships with the animation, every time:

```css
@media (prefers-reduced-motion: reduce) {
  .element { animation: fade 0.2s ease; } /* keep opacity/color, drop transform motion */
}
@media (hover: hover) and (pointer: fine) {
  .element:hover { transform: scale(1.05); }
}
```

Reduced motion means fewer and gentler animations, not zero — keep transitions that aid comprehension.

## Never ship

| Never | Instead |
| --- | --- |
| `transition: all` | Name the exact properties |
| `transform: scale(0)` entrance | `scale(0.95)` + `opacity: 0` |
| `ease-in` on a UI element | `ease-out` or a strong custom curve |
| Animation on a keyboard shortcut or 100+/day action | No animation |
| UI duration over 300ms with no reason | 150-250ms |
| `transform-origin: center` on a trigger-anchored popover | `var(--transform-origin)` (modals exempt) |
| Keyframes on toasts, toggles, rapidly-triggered elements | CSS transitions |
| Animating `width`/`height`/`margin`/`padding`/`top`/`left` | `transform` / `opacity` |
| Motion `x`/`y`/`scale` props under load | Full `transform` string |
| Ungated `:hover` motion | `@media (hover: hover) and (pointer: fine)` |
| Missing `prefers-reduced-motion` | Gentler variant, not zero |
| Everything entering at once | 30-80ms stagger |

## Output

Write the code. Then, in a few lines: the gate result (frequency tier + named purpose), the ingredients (tool, properties, curve, duration or spring), and what to feel-check (play it at 2-5x duration or in the DevTools animation inspector, step frame by frame, test gestures on a real device). The code is the deliverable — don't pad it into a report.
