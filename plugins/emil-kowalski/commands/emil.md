---
description: Run an Emil Kowalski design-engineering pass on the current frontend - motion, easing, and interaction details
---

# /emil

Do a design-engineering pass on the current project or the files the user points to, using the emil-kowalski skills.

1. Load the `emil-design-eng` skill and apply its standards to the frontend code: press feedback on buttons, origin-aware popovers, custom easing curves, durations under 300ms, transitions instead of keyframes for interruptible UI.
2. Review existing animations with the `review-animations` standards. Report findings in the required table format (| Before | After | Why |), citing file and line.
3. Fix the clear violations (ease-in on UI, `scale(0)` entrances, `transition: all`, ungated hover, missing `prefers-reduced-motion`), then re-check the fixed code against the checklist.
4. Summarize what changed in 3-5 bullets, and call out anything that needs a feel-check in a real browser or on a real device.

If the user asks to build a specific animation instead, use the `animate` skill's build sequence. If they want a full codebase audit or places that should animate, use `improve-animations` or `find-animation-opportunities`.
