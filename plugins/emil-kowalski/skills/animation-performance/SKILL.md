---
name: animation-performance
description: Keep web animations inside the frame budget - composite-only properties, motion off the main thread, and diagnosing dropped frames. Use when an animation is janky, stutters, or drops frames, or when choosing which property to animate.
---

# Animation Performance

## Hard rules

1. **`transform` and `opacity` by default.** Anything else needs a reason. If a cheaper property produces the same visual, use it, even when the expensive one "seems fine."
2. **Name the cause before changing code.** Every fix names which cause below it addresses. "Added `will-change`, seems smoother" is not a diagnosis.
3. **Never conclude "it's fine" from a fast machine.** Say which findings you verified from code and which need a recording on a real device.
4. **Never `transition: all`.** List the exact properties — `all` animates whatever changes next, including layout properties nobody intended to animate.
5. **Don't trade correctness for a best-effort optimization.** Hardware acceleration is something the browser may grant; which properties you animate is something you control. Spend effort on the second.

## Symptom table

Start here. Match what you're seeing, then apply the fix.

| Problem | Solution |
| --- | --- |
| Animation stutters | Animate `transform`/`opacity`, not `width`/`height`/`top`/`margin` |
| Shadow animation is choppy | `filter: drop-shadow()` instead of animating `box-shadow` |
| Jank only during navigation or loading | Move the animation to CSS or WAAPI so it runs off the main thread |
| Motion (Framer Motion) `x`/`y` drops frames | Animate the full `transform` string |
| Hand-rolled animation drops frames in React | Write to `ref.current.style`, not state |
| Drag gets laggier as the list grows | Stop animating an inherited CSS variable; set `transform` directly |
| Blur transition lags (especially Safari) | Keep animated `blur()` under 20px |
| Unexpected properties animate | Replace `transition: all` with explicit properties |
| Element shifts 1px when motion starts or ends | `will-change: transform`, only once you see it |
| Long list scrolls slowly | Virtualize — only render what's visible |
| Random properties animate | Don't do `transition: all`, list exact properties |

## The frame budget

60fps is what the brain reads as fluid motion: **16.7ms** per frame. On a 120Hz screen — most phones, many laptops — it's **8ms**. Design against the smaller number. For scale: a layout recalculation on a complex page can take over 100ms. That isn't one missed frame, it's a dozen.

Two things decide whether you stay inside the budget: **what you animate** (how much work each frame costs) and **where it runs** (whether that work can be blocked by everything else the page is doing).

## What you animate

The browser runs up to three steps per visual change — layout, paint, composite — and every property enters the pipeline at a different point:

| Tier | Properties | Cost |
| --- | --- | --- |
| Composite only | `transform`, `opacity` (in current Chrome/Firefox also `filter`, `clip-path`, `background-color`) | Cheapest; promoted to their own layer |
| Paint + composite | `box-shadow`, `border-radius`, `color` | Redraw every frame, no re-measuring |
| Layout + paint + composite | `width`, `height`, `padding`, `margin`, `top`, `left`, `border-width` | Most expensive; one growing element moves everything around it |

Swap down a tier:

| Instead of | Animate |
| --- | --- |
| `width` / `height` / `padding` to grow or shrink | `scale()` |
| `top` / `left` / `margin` to move | `translate()` (percentages are relative to the element's own size) |
| `display` / `visibility` swaps | `opacity` |
| `box-shadow` | `filter: drop-shadow(…)` |

The shadow one is the one people miss: an animated shadow looks harmless but repaints the element every frame. And layout properties move the wrong things — animating a sidebar's `margin-left` re-lays-out the content beside it 60 times a second; `translateX` doesn't.

## Where it runs

- **CSS animations and transitions run off the main thread** and stay smooth while the page loads, scripts, or paints. `requestAnimationFrame` animation competes with all of that and drops frames under load.
- **WAAPI** (`element.animate()`) gives JavaScript control with CSS performance — hardware-accelerated, interruptible, no library.
- **Motion/Framer Motion shorthand props** (`x`, `y`, `scale`) use rAF on the main thread and are NOT hardware-accelerated. Use the full `transform` string for motion that must survive a busy page.
- **Never animate through React state.** A state update per frame re-renders the component tree per frame. Write to `ref.current.style` instead.
- **Inherited CSS variables are a trap.** Changing a variable on a parent recalculates styles for all children. In a long list or drawer, set `transform` directly on the moving element.

## `will-change`

A targeted hint for a known expensive animation — apply it once you actually see the 1px shift or layer problem, never as a blanket baseline. Leaving `will-change` on at rest wastes memory on layers nothing is using.

## Workflow

1. Reproduce and name the symptom; match it in the table.
2. Check what the animation animates (tier) and where it runs (thread) — most jank is one of the two.
3. Apply the cheapest fix that addresses the named cause; prefer property swaps over new tooling.
4. Verify under load (page loading, long list, slower device or CPU throttling), not on an idle fast machine.
