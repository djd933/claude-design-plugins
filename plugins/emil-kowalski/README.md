# emil-kowalski

Emil Kowalski's design engineering skills for Claude Code: animation decisions, easing curves, strict motion review, and the invisible interaction details that make interfaces feel right.

## What it does

Eight skills, adapted from Emil Kowalski's open-source skill set (built on his work at Vercel and Linear, and libraries like Sonner and Vaul):

- **emil-design-eng** - The core philosophy: the animation decision framework (should it animate, purpose, easing, duration), custom easing curves, springs, component polish rules, gesture physics, and a review checklist
- **animate** - Builds an animation from scratch through a gated sequence: frequency gate, named purpose, cheapest tool, GPU-only properties, curve and duration, interruption and exit
- **review-animations** - Reviews motion code against ten non-negotiable standards with a required findings-table format and a Block/Approve verdict
- **improve-animations** - Audits a whole codebase's motion and returns a prioritized plan of self-contained fixes (read-only)
- **find-animation-opportunities** - Finds places that genuinely should animate, gated by frequency, purpose, speed, and function - and rejects the rest
- **animation-performance** - Frame-budget diagnosis for janky animation: property cost tiers, main-thread traps, and a symptom-to-fix table including the performance cheatsheet
- **animation-vocabulary** - Reverse-lookup glossary: turns "the bouncy thing when a popover opens" into the exact term (Pop in)
- **mobile-native** - Makes web apps feel native on phones: sticky hover, tap highlight, `100dvh`, 16px inputs, safe areas, and the baseline meta/CSS floor

## Install

```
/plugin install emil-kowalski@design-plugins
```

## Usage

The skills activate automatically on frontend and motion work. Or run `/emil` for an explicit design-engineering pass over the current project.

## Credits

Adapted from [emilkowalski/skills](https://github.com/emilkowalski/skills) by Emil Kowalski, used under the MIT License (see `LICENSE` in this folder - the upstream copyright notice is preserved there). This plugin covers the web design, animation, and interaction skills; the upstream React Native/Expo, Swift, Apple-platform, prototype, and Sonner-specific skills are not included.
