# Claude Design Plugins

7 Claude Code plugins that make your websites look 10x better. The first five are built from ideas in the viral "5 Claude Code Plugins" reel; plugins 6-7 adapt Emil Kowalski's design skills and Paul Bakaus's Impeccable.

## Plugins

1. **taste-skill** - Applies premium design taste references so Claude's output doesn't look like generic AI templates
2. **web-design-guidelines** - Audits your UI against modern design principles and catches weak areas before you ship
3. **awesome-design** - Complete design system covering colors, typography, spacing, buttons, and components
4. **image-to-code** - Turns any design reference image into usable code while preserving details
5. **playwright-cli** - Lets Claude open your site, take screenshots, and catch broken UI automatically
6. **emil-kowalski** - Emil Kowalski's design engineering skills: animation decisions, easing curves, strict motion review, and the invisible details that make interfaces feel right
7. **impeccable** - The Impeccable design language: anti-slop craft rules plus playbooks to polish, typeset, colorize, animate, and clarify your UI

## Install

Add this marketplace in Claude Code:

```
/plugin marketplace add github.com/djd933/claude-design-plugins
```

Then install what you want:

```
/plugin install taste-skill@design-plugins
/plugin install web-design-guidelines@design-plugins
/plugin install awesome-design@design-plugins
/plugin install image-to-code@design-plugins
/plugin install playwright-cli@design-plugins
/plugin install emil-kowalski@design-plugins
/plugin install impeccable@design-plugins
```

Or install all seven at once - see each plugin's README for details.

## Structure

```
plugins/
  taste-skill/
  web-design-guidelines/
  awesome-design/
  image-to-code/
  playwright-cli/
  emil-kowalski/
  impeccable/
```

Each plugin follows the standard Claude Code plugin structure with `plugin.json`, skills, and slash commands.
