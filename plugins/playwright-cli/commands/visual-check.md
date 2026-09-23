---
description: Run a Playwright visual QA check on the local site
---

# /visual-check

Run visual QA on the project's frontend.

1. Ensure Playwright is available (offer to install if not).
2. Start the dev server if needed.
3. Screenshot key pages at desktop (1440px) and mobile (390px), capturing console/JS errors.
4. Inspect the screenshots for broken layout, typography issues, missing images, and mobile problems.
5. Report findings with specifics and fix if the user wants.

Usage: `/visual-check http://localhost:3000 http://localhost:3000/pricing` - or with no URLs, check the obvious local routes.
