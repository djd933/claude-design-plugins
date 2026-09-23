# playwright-cli

Visual QA for Claude Code: opens your site, takes screenshots, and catches broken UI automatically.

## What it does

- Screenshots pages at desktop and mobile viewports
- Captures JS and console errors during load
- You (Claude) actually look at the screenshots and report what's broken with specifics

## Install

```
/plugin install playwright-cli@design-plugins
```

## Usage

Run `/visual-check` optionally with URLs. Requires Playwright (`npm i -D @playwright/test`) and a running dev server - it will offer to set both up.
