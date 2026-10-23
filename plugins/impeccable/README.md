# impeccable

The Impeccable design language for Claude Code: surface modes, a craft floor of anti-slop rules, and ten playbooks for refining frontend interfaces.

## What it does

- **One core skill** (`impeccable`) with the design vocabulary: work modes (Persuade, Operate, Read, Experience), core principles (the brief wins; refinement preserves, redesign replaces), and a craft floor — checkable rules against the standard "AI slop" tells: purple-to-blue gradients, Inter-for-everything, nested cards, gray text on colored backgrounds, gradient text, eyebrows over every heading, the hero-metric template
- **Ten command playbooks** in the skill's `reference/` folder, run as `/impeccable <cmd>`:
  - `polish` - final quality pass before shipping, triaged by impact
  - `audit` - scored technical quality report (a11y, performance, theming, responsive, code quality); reports, doesn't fix
  - `typeset` - typography roles, hierarchy, scale, and measure
  - `colorize` - strategic color as roles, not swatches, with OKLCH guidance and contrast checks
  - `animate` - purposeful motion from a written motion thesis
  - `bolder` - amplify a bland section using the system's own strongest moves
  - `quieter` - tone down a loud design without making it generic
  - `layout` - spacing, rhythm, grouping, and reading order
  - `clarify` - rewrite unclear UX copy, labels, and error messages
  - `distill` - strip a design to its essence

## Install

```
/plugin install impeccable@design-plugins
```

## Usage

Run `/impeccable` with no arguments for a context-aware menu, or name a command directly: `/impeccable polish`, `/impeccable typeset src/components`. Commands are namespaced under `/impeccable` (upstream's model), so `/impeccable audit` doesn't collide with web-design-guidelines' standalone `/audit`.

## Credits

Adapted from [pbakaus/impeccable](https://github.com/pbakaus/impeccable) by Paul Bakaus, used under the Apache License 2.0 (see `LICENSE` in this folder). This plugin is a playbook adaptation: it includes the core skill, the craft floor, and 10 of upstream's 23 command playbooks. Upstream's bundled detector CLI and design hook, live browser variant mode, init/document/extract context tooling, and native iOS/Android playbooks are not included.
