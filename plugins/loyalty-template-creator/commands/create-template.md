---
description: Create a new CDN loyalty page template from a reference website
argument-hint: <template-name> <reference-url>
---

Parse `$ARGUMENTS`:
- **First word** (`$1`): Template filename (without `.liquid`)
- **Rest**: Reference URL to replicate

If either is missing, ask the user.

Follow these steps:

1. Read the skill at `${CLAUDE_PLUGIN_ROOT}/skills/loyalty-template/SKILL.md`

2. Read the reference files:
   - `${CLAUDE_PLUGIN_ROOT}/skills/loyalty-template/references/template-engine.md`
   - `${CLAUDE_PLUGIN_ROOT}/skills/loyalty-template/references/template-structure.md`

3. Execute the skill workflow (Steps 0–3): ask environment, fetch docs, analyze reference page, plan template, generate the `.liquid` file

4. Write the template to `theme-extensions/loyalty-page/templates/<name>.liquid`
   - If the `shopify-extensions` repo is not the current working directory, ask the user where to save the file

5. If in the `shopify-extensions` repo, verify:
   - `npm run lint` passes
   - `npm run build:theme-extensions` compiles
   Otherwise, remind the user to run lint and build manually.
