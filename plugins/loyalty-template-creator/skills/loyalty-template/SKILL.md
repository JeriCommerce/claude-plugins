---
name: Jeri - Loyalty Template Creator
description: >
  Use when the user asks to "create a loyalty template", "build a loyalty page",
  "replicate a loyalty page style", "create a Shopify loyalty template",
  "make a loyalty page template", or needs to create a new CDN loyalty page
  template for a Shopify theme extension based on a reference website.
version: 202603.24.0
---

# Loyalty Template Creator

Create CDN loyalty page templates that replicate the visual style of a reference website, powered by the JeriCommerce loyalty-page JS engine.

**Output**: `theme-extensions/loyalty-page/templates/<name>.liquid` (in the `shopify-extensions` repo)

## Workflow

### Step 0: Environment and documentation

Read `${CLAUDE_PLUGIN_ROOT}/skills/loyalty-template/references/tools.md` for required tools, CDN URLs by environment, and canonical documentation sources.

Ask the user for the **template name**, **reference URL**, and **environment** (staging/production). Then fetch the canonical docs listed in `references/tools.md` using `WebFetch`.

### Step 1: Analyze the reference page

Read `${CLAUDE_PLUGIN_ROOT}/skills/loyalty-template/references/analysis-guide.md` for what to extract from the reference page.

Use `WebFetch` on the reference URL and follow the analysis guide to extract visual style, assets, and fonts.

### Step 2: Plan the template

Present the user with the proposed color palette, font stack, section layout, asset mapping, earning icon mapping, tier badge mapping, and CTA widget routes (from `references/template-engine.md`).

Ask for confirmation before proceeding.

### Step 3: Generate the template

Read these reference files before generating:

- `${CLAUDE_PLUGIN_ROOT}/skills/loyalty-template/references/template-engine.md` — Earning types, CTA routes, data attributes
- `${CLAUDE_PLUGIN_ROOT}/skills/loyalty-template/references/template-structure.md` — HTML skeleton, critical rules, BEM naming, schema validation

Also read the `siwon-copy.liquid` example template in the `shopify-extensions` repo at `theme-extensions/loyalty-page/templates/siwon-copy.liquid` (ask the user for the path if not found).

Generate a single self-contained `.liquid` file following all the rules in the reference files.

### Step 4: Verify (if in shopify-extensions repo)

If working in the `shopify-extensions` repo, run `npm run lint` and `npm run build:theme-extensions`. Otherwise, remind the user to run lint and build manually.

## Reference Files

- `references/tools.md` — Required tools, CDN URLs by environment, and canonical documentation sources
- `references/template-engine.md` — Earning types, CTA widget routes, data attributes, variable interpolation
- `references/template-structure.md` — HTML skeleton, critical implementation rules, BEM naming, responsive requirements
- `references/analysis-guide.md` — How to analyze a reference page and extract visual style, assets, and fonts
