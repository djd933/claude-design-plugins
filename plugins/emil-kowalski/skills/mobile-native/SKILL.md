---
name: mobile-native
description: Make a web app feel native on a phone - sticky hover states, tap highlight flashes, the 100vh bug, inputs that zoom the page, laggy taps, safe areas. Use when building or fixing mobile web UI.
---

# Mobile Native

The small fixes that separate a website from an app on a phone. The user's phone is the source of truth: these bugs don't reproduce in Chrome's device emulation, so if you can't run it on hardware, say which fixes you verified from code and which need a real device.

## Hard rules

1. **Every fix ships with the reason.** Apply each rule where its *why* applies, not globally out of habit — `user-select: none` on body text is a defect; on a button it's correct.
2. **Media queries over device sniffing.** `(hover: hover)`, `(pointer: fine)`, `env()`, `dvh` — the platform tells you what it can do. Never branch on user-agent strings.
3. **Touch and mouse are not exclusive.** iPads with trackpads, laptops with touchscreens. Write for both at once; gate by capability, not by device.
4. **Never disable zoom.** `user-scalable=no` and `maximum-scale=1` are accessibility failures. Fix the input font size instead — that's what was causing the zoom.
5. **Reach for CSS or a meta tag before JavaScript.** Almost every item here is one declaration. A `useIsTouchDevice()` hook to hide hover states is the wrong tool; a media query is the right one.

## Symptom table

| Problem | Solution |
| --- | --- |
| Hover state stuck after tap | Wrap in `@media (hover: hover) and (pointer: fine)` |
| Gray/blue flash on tap | Kill `-webkit-tap-highlight-color` |
| Layout has wrong height | `100dvh` (app) or `100svh` (hero) |
| Page zooms into input | Input font size 16px minimum |
| Tap feels laggy | Feedback on pointer-down + `touch-action: manipulation` |
| Pull-to-refresh hijacks scroll | `overscroll-behavior: none` on `html, body` |
| Content stops at the notch | `viewport-fit=cover` + `env(safe-area-inset-*)` |
| Long-press selects button text | Add `user-select: none` (controls only) |
| Carousel scrolls vertically | `touch-action: pan-y` on the gesture surface |
| Status bar color doesn't match | `theme-color` per color scheme |
| Right in Chrome, wrong on phone | Test on real hardware |

## The fixes

**1. Hover stuck after tap.** Touch has no hover; browsers fake it — the first tap applies `:hover` and leaves it until the user taps elsewhere. Gate every hover style behind `@media (hover: hover) and (pointer: fine)`.

**2. Gray/blue flash on tap.** iOS Safari and Android Chrome paint a translucent highlight over any tapped element with a click handler — the loudest "this is a website" signal. Set once, globally: `html { -webkit-tap-highlight-color: transparent; }`. Then make sure every tappable element has its own `:active` state, because you removed the only feedback the browser gave.

**3. Wrong layout height.** `100vh` on mobile is the *largest* viewport (browser chrome collapsed), so on load a `100vh` element overflows and bottom-pinned UI sits under the URL bar. Use `.app { height: 100dvh; }` for shells that track the visible area, and `.hero { min-height: 100svh; }` for heroes — `svh` is stable and never overflows, while `dvh` resizing mid-scroll causes layout shift on marketing content.

**4. Page zooms into the input.** iOS Safari zooms when focus lands on an input under 16px, and doesn't zoom back out. Fix: `input, textarea, select { font-size: 16px; }` (or scope it with `@media (pointer: coarse)` if desktop needs smaller). While you're there, set the keyboard: `inputmode`, `type="email"`/`"tel"`, `autocapitalize="none"` and `autocorrect="off"` on usernames and codes, `enterkeyhint` so the return key says what it does.

**5. Tap feels laggy.** Two causes stack. The click delay: `touch-action: manipulation` on buttons/links tells the browser this element never double-tap-zooms, so `click` fires immediately. And feedback on release: native buttons respond when the finger lands, so style `:active` (scale 0.97, 100-160ms ease-out) and listen to `pointerdown`, not `click`, in JS.

**6. Pull-to-refresh hijacks scroll.** `overscroll-behavior: none` on `html, body` stops the browser gesture from taking over your app's scrolling. Drop it if the page is a scrolling document where pull-to-refresh is welcome.

**7. Content stops at the notch.** Add `viewport-fit=cover` to the viewport meta, then pad with `env(safe-area-inset-top)` etc. Without the meta, the env values are 0.

**8. Long-press selects button text.** `user-select: none` (with `-webkit-` prefix) on buttons and tappable controls — never on body text.

**9. Carousel scrolls vertically.** `touch-action: pan-y` on a horizontal gesture surface tells the browser vertical movement is page scroll and horizontal is yours.

**10. Status bar color doesn't match.** One `theme-color` meta per `prefers-color-scheme`, not a single value for both.

## Baseline

Ship this before the first component of a mobile-facing app:

```html
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover, interactive-widget=resizes-content" />
<meta name="theme-color" media="(prefers-color-scheme: light)" content="#ffffff" />
<meta name="theme-color" media="(prefers-color-scheme: dark)" content="#0a0a0a" />
```

```css
html {
  -webkit-tap-highlight-color: transparent;
  -webkit-text-size-adjust: 100%; /* no font inflation in landscape */
  overscroll-behavior: none;
}
input, textarea, select { font-size: 16px; }
button, a, [role="button"] {
  touch-action: manipulation;
  user-select: none;
  -webkit-user-select: none;
}
@media (hover: hover) and (pointer: fine) {
  /* all :hover rules live here */
}
```

`interactive-widget=resizes-content` makes the software keyboard shrink the layout viewport on Android Chrome, so `100dvh` and bottom-pinned inputs react to it the way they do on iOS.

## Never ship

| Never | Instead |
| --- | --- |
| `user-scalable=no` or `maximum-scale=1` | 16px inputs — fix the cause |
| Ungated `:hover` | `@media (hover: hover) and (pointer: fine)` |
| `100vh` for an app shell or bottom-pinned UI | `100dvh` |
| `100dvh` on a marketing hero | `100svh` (no layout shift on scroll) |
| Press feedback on `click` only | `:active` / `pointerdown` |
| `touchmove` + `preventDefault()` to stop overscroll | `overscroll-behavior` |
| `user-select: none` on `body` | Only on controls |
| `env(safe-area-inset-*)` without `viewport-fit=cover` | Add the meta tag or the value is `0` |
| One `theme-color` for both schemes | One per `prefers-color-scheme` |
| User-agent sniffing to detect touch | `(hover)` / `(pointer)` media queries |
| Declaring it fixed from device emulation | Real hardware |
