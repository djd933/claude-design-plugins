# Claude Design Plugins

5 Claude Code plugins that make your websites look 10x better. Built from ideas in the viral "5 Claude Code Plugins" reel.

## Plugins

1. **taste-skill** - Applies premium design taste references so Claude's output doesn't look like generic AI templates
2. **web-design-guidelines** - Audits your UI against modern design principles and catches weak areas before you ship
3. **awesome-design** - Complete design system covering colors, typography, spacing, buttons, and components
4. **image-to-code** - Turns any design reference image into usable code while preserving details
5. **playwright-cli** - Lets Claude open your site, take screenshots, and catch broken UI automatically

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
```

Or install all five at once - see each plugin's README for details.

## Structure

```
plugins/
  taste-skill/
  web-design-guidelines/
  awesome-design/
  image-to-code/
  playwright-cli/
```

Each plugin follows the standard Claude Code plugin structure with `plugin.json`, skills, and slash commands.
