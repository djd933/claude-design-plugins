---
name: emil-design-eng
description: Emil Kowalski's design engineering philosophy: animation decisions, easing, component polish, and the invisible details that make interfaces feel right. Use when building or reviewing UI where motion and interaction quality matter.
---

# Design Engineering

Adapted from Emil Kowalski's skills (Vercel, Linear; creator of Sonner and Vaul). Taste is trained, not innate: study why the best interfaces feel the way they do. Unseen details compound — when a feature functions exactly as someone assumes it should, they proceed without a second thought. That is the goal.

## Review format (required)

When reviewing UI code, output a single markdown table with Before / After / Why columns, one row per issue. Never a "Before: / After:" list.

| Before | After | Why |
| --- | --- | --- |
| `transition: all 300ms` | `transition: transform 200ms ease-out` | Specify exact properties; avoid `all` |
| `transform: scale(0)` | `transform: scale(0.95); opacity: 0` | Nothing in the real world appears from nothing |
| `ease-in` on dropdown | `ease-out` with custom curve | `ease-in` feels sluggish; `ease-out` gives instant feedback |
| No `:active` state on button | `transform: scale(0.97)` on `:active` | Buttons must feel responsive to press |

## The animation decision framework

Before writing any animation code, answer these in order.

### 1. Should this animate at all?

| Frequency | Decision |
| --- | --- |
| 100+ times/day (keyboard shortcuts, command palette toggle) | No animation. Ever. |
| Tens of times/day (hover effects, list navigation) | Remove or drastically reduce |
| Occasional (modals, drawers, toasts) | Standard animation |
| Rare/first-time (onboarding, celebrations) | Can add delight |

Never animate keyboard-initiated actions. Raycast has no open/close animation — that is optimal for something used hundreds of times a day.

### 2. What is the purpose?

Every animation needs a clear answer: **spatial consistency** (toast enters and exits from the same direction), **state indication** (a morphing feedback button), **explanation** (marketing animation showing how a feature works), **feedback** (button scales on press), or **preventing a jarring change**. If the purpose is just "it looks cool" and users will see it often, don't animate.

### 3. What easing?

- Entering or exiting → `ease-out` (starts fast, feels responsive)
- Moving/morphing on screen → `ease-in-out`
- Hover/color change → `ease`
- Constant motion (marquee, progress bar) → `linear`
- Default → `ease-out`

**Never use `ease-in` for UI animations.** It starts slow and delays the exact moment the user is watching most closely; `ease-out` at 200ms feels faster than `ease-in` at 200ms.

Built-in CSS easings are too weak. Use custom curves:

```css
--ease-out: cubic-bezier(0.23, 1, 0.32, 1);      /* strong ease-out for UI */
--ease-in-out: cubic-bezier(0.77, 0, 0.175, 1);  /* strong ease-in-out for movement */
--ease-drawer: cubic-bezier(0.32, 0.72, 0, 1);   /* iOS-like drawer curve (Ionic) */
```

Find stronger variants at easing.dev or easings.co instead of inventing curves.

### 4. How fast?

| Element | Duration |
| --- | --- |
| Button press feedback | 100-160ms |
| Tooltips, small popovers | 125-200ms |
| Dropdowns, selects | 150-250ms |
| Modals, drawers | 200-500ms |
| Marketing/explanatory | Can be longer |

**UI animations stay under 300ms.** Perceived performance is real performance: a faster-spinning spinner makes the same load time feel faster, and instant tooltips after the first one is open make the whole toolbar feel faster.

## Springs

Springs simulate physics, settle without fixed durations, and keep velocity when interrupted — ideal for drag interactions, elements that should feel alive, and gestures users can reverse mid-motion. Avoid them for ordinary state changes.

```js
{ type: "spring", duration: 0.5, bounce: 0.2 }              // Apple-style, easier to reason about
{ type: "spring", mass: 1, stiffness: 100, damping: 10 }    // traditional physics, more control
```

Keep bounce subtle (0.1-0.3) and out of most UI. For decorative mouse-tracking, interpolate with a spring (`useSpring` from Motion) instead of mapping mouse position directly — direct mapping feels artificial.

## Component principles

- **Buttons must feel responsive.** `transform: scale(0.97)` on `:active` (subtle range 0.95-0.98) with `transition: transform 160ms ease-out`. Applies to any pressable element.
- **Never animate from `scale(0)`.** Start from `scale(0.95)` with `opacity: 0`. Nothing in the real world appears from nothing.
- **Popovers are origin-aware.** They scale from their trigger: `transform-origin: var(--transform-origin)` (Base UI). Modals are the exception — they stay centered.
- **Tooltips skip delay on subsequent hovers.** Delay the first to prevent accidental activation; once one is open, adjacent tooltips open instantly with no animation.
- **Transitions over keyframes for interruptible UI.** Transitions retarget mid-animation; keyframes restart from zero. Anything triggered rapidly (toasts, toggles) uses transitions.
- **Blur masks imperfect crossfades.** When two states overlap during a crossfade you see two objects; a subtle `filter: blur(2px)` during the transition blends them into one transformation. Keep blur under 20px (expensive, especially in Safari).
- **Animate enter states with `@starting-style`** instead of the `useEffect` + `data-mounted` pattern, when browser support allows.
- **Asymmetric timing.** Slow where the user is deciding (hold-to-delete: 2s linear press), snappy where the system responds (release: 200ms ease-out).

## Transforms and clip-path

- Percentages in `translate()` are relative to the element's own size: `translateY(100%)` moves a drawer by its own height regardless of content. Prefer percentages over hardcoded pixels.
- `scale()` scales children too — font, icons, content. That is a feature.
- `clip-path: inset(top right bottom left)` is a first-class animation tool: left-to-right reveals, hold-to-delete overlays, comparison sliders (clip the top image, drive the inset from drag position), seamless tab color transitions (duplicate the tab list styled "active", clip it to the active tab, animate the clip).

## Gestures and drag

- **Velocity-based dismissal:** dismiss when `Math.abs(distance) / elapsedTime > ~0.11`, regardless of distance. A quick flick should be enough; don't require crossing a threshold.
- **Damping at boundaries:** the further past a boundary the user drags, the less the element moves. Friction instead of hard stops.
- **Pointer capture** once a drag starts, and **ignore additional touch points** after the initial drag begins.

## Performance rules

- **Only animate `transform` and `opacity`** (plus `clip-path` as the sanctioned extra). They skip layout and paint. Animating `width`/`height`/`margin`/`padding`/`top`/`left` triggers all three rendering steps.
- **CSS variables are inheritable.** Updating `--swipe-amount` on a container recalculates styles for every child. Set `transform` directly on the element instead.
- **Motion (Framer Motion) `x`/`y`/`scale` shorthands are NOT hardware-accelerated** — they run `requestAnimationFrame` on the main thread. Use the full string: `animate={{ transform: "translateX(100px)" }}`.
- **CSS animations beat JS under load.** CSS runs off the main thread and stays smooth while the page loads; rAF animation drops frames. CSS for predetermined animation, JS for dynamic/interruptible. WAAPI (`element.animate()`) gives JS control with CSS performance.

## Accessibility

- **`prefers-reduced-motion`:** fewer and gentler animations, not zero. Keep opacity and color transitions that aid comprehension; remove movement and position changes.
- **Gate hover motion** behind `@media (hover: hover) and (pointer: fine)` — touch devices fire false hovers on tap.

## Stagger

Elements entering together stagger 30-80ms apart (`animation-delay` per child, `fadeIn 300ms ease-out forwards` from `translateY(8px)` + opacity 0). Stagger is decorative — never block interaction while it plays.

## Debugging

Play animations at 2-5x duration or in the DevTools Animations panel, step frame by frame, check transform-origin and property sync. Test gestures on a real device. Review your work the next day with fresh eyes.

## Review checklist

| Issue | Fix |
| --- | --- |
| `transition: all` | Specify exact properties |
| `scale(0)` entry | Start from `scale(0.95)` + `opacity: 0` |
| `ease-in` on UI element | `ease-out` or custom curve |
| `transform-origin: center` on popover | Trigger location (modals exempt) |
| Animation on keyboard action | Remove it entirely |
| Duration > 300ms on UI element | Reduce to 150-250ms |
| Hover animation without media query | `@media (hover: hover) and (pointer: fine)` |
| Keyframes on rapidly-triggered element | CSS transitions |
| Motion `x`/`y` props under load | Full `transform` string |
| Same enter/exit speed | Exit faster than enter |
| Elements all appear at once | 30-80ms stagger |
