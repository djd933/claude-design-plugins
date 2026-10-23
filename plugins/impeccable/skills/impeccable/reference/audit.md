# Audit

Run systematic **technical** quality checks and generate a comprehensive report. Don't fix issues here — document them for the fixing commands (`polish`, `typeset`, `colorize`, `layout`, `clarify`) to address. This is a code-level audit, not a design critique: check what's measurable and verifiable in the implementation.

Run checks across 5 dimensions. Score each dimension 0-4 using the criteria below, cite evidence (file, selector, computed value) for every finding, and order the report by dimension score, worst first.

## 1. Accessibility

**Check for:** contrast ratios below 4.5:1 (7:1 for AAA); motion sensitivity — `prefers-reduced-motion` needs an intentional alternative that preserves state change and hierarchy (flag a global `0.01ms` kill that destroys useful feedback, flashing content, motion that blocks focus or reading); missing ARIA roles, labels, and states on interactive elements; keyboard navigation — missing focus indicators, illogical tab order, keyboard traps; semantic HTML — heading hierarchy, landmarks, divs instead of buttons; missing or poor alt text; form issues — inputs without labels, poor error messaging, missing required indicators.

**Score:** 0 = Inaccessible (fails WCAG A) · 1 = Major gaps (few ARIA labels, no keyboard nav) · 2 = Partial (some effort, significant gaps) · 3 = Good (WCAG AA mostly met, minor gaps) · 4 = Excellent (AA fully met, approaches AAA)

## 2. Performance

**Check for:** layout thrashing (reading/writing layout properties in loops); expensive animations (casual layout-property animation, unbounded blur/filter/shadow effects, visibly dropped frames); missing optimization (images without lazy loading, unoptimized assets); `will-change` overuse (applied broadly or left on at rest); bundle size (unnecessary imports, unused dependencies); render performance (unnecessary re-renders, missing memoization).

**Score:** 0 = Severe (layout thrash, unoptimized everything) · 1 = Major (no lazy loading, expensive animations) · 2 = Partial (some optimization, gaps remain) · 3 = Good (mostly optimized, minor improvements) · 4 = Excellent (fast, lean, well-optimized)

## 3. Theming

**Check for:** hard-coded colors not using design tokens; broken dark mode (missing variants, poor dark-theme contrast); inconsistent tokens (wrong tokens, mixed token types); theme-switching issues (values that don't update on theme change).

**Score:** 0 = No theming (hard-coded everything) · 1 = Minimal tokens (mostly hard-coded) · 2 = Partial (tokens exist, inconsistently used) · 3 = Good (tokens used, minor hard-coded values) · 4 = Excellent (full token system, dark mode works)

## 4. Responsive design

**Check for:** fixed widths that break on mobile; touch targets under 44×44px; horizontal scroll at 375px; text that overflows or truncates badly at narrow widths; breakpoints that leave intermediate widths broken; images and media that don't scale; tap targets too close together.

**Score:** 0 = Broken on mobile · 1 = Major breakage (fixed widths, tiny targets) · 2 = Partial (works, with overflow or target issues) · 3 = Good (minor issues at some widths) · 4 = Excellent (fluid at every width, targets generous)

## 5. Code quality

**Check for:** duplicated style blocks and one-off values that should be tokens; dead CSS and unused classes; inline styles bypassing the system; specificity wars and `!important` clusters; components re-implementing what a shared component already does.

**Score:** 0 = Spaghetti · 1 = Heavy duplication · 2 = Partial (system exists, often bypassed) · 3 = Good (system followed, minor exceptions) · 4 = Excellent (clean, token-driven, no dead weight)

## Report format

For each dimension: the score, then findings ordered by user impact — each with evidence and the command that would fix it (`/impeccable polish`, `typeset`, `colorize`, `layout`, `clarify`, or a plain code fix). Close with the three highest-impact fixes overall. Report only — do not apply fixes in an audit.
