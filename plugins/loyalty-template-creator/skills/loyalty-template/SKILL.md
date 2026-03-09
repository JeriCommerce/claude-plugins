---
name: Jeri - Loyalty Template Creator
description: >
  Use when the user asks to "create a loyalty template", "build a loyalty page",
  "replicate a loyalty page style", "create a Shopify loyalty template",
  "make a loyalty page template", or needs to create a new CDN loyalty page
  template for a Shopify theme extension based on a reference website.
version: 202603.09.0
---

# Loyalty Template Creator

Create CDN loyalty page templates that replicate the visual style of a reference website, powered by the JeriCommerce loyalty-page JS engine.

**Output**: `theme-extensions/loyalty-page/templates/<name>.liquid` (in the `shopify-extensions` repo)

## Required Tools

This skill uses built-in tools only:

- `WebFetch` — Fetch reference pages and documentation
- `Read` — Read existing templates for reference
- `Write` — Create the template file
- `Bash` — Run lint and build (when in the `shopify-extensions` repo)

## Workflow

### Step 0: Environment and documentation

**Ask the user**:

1. **Template name** — Filename (without `.liquid`)
2. **Reference URL** — The website whose style to replicate
3. **Environment** — Staging or production?
   - **Staging**: CDN URLs use `s.` prefix → `https://s.cdn.jericommerce.com/shopify/loyalty-page.js`
   - **Production**: CDN URLs without prefix → `https://cdn.jericommerce.com/shopify/loyalty-page.js`

Then fetch the latest docs from these two canonical sources using `WebFetch`:

1. **Template engine docs**: `https://www.jericommerce.com/guides/loyalty-page-building-custom-templates-with-cdn-scripts`
   - Covers: root element, data attributes, variable interpolation, earning/tier fields, loading states, section structure, icon system, CSS classes
2. **Widget routes docs**: `https://www.jericommerce.com/guides/how-to-use-the-jericommerce-storefront-widget`
   - Covers: all `#jeri=` hash fragment routes for CTAs

These docs are the **canonical reference**. If anything in this skill conflicts with the live docs, **the docs win**.

### Step 1: Analyze the reference page

Use `WebFetch` to analyze the reference URL. Extract:

1. **Visual style** — colors, fonts, layout, spacing, border-radius
2. **Section structure** — hero, earnings, tiers, banners, CTAs
3. **Color palette** — primary, accent, background, text, muted colors
4. **Asset inventory** — Collect and list ALL reusable asset URLs found on the page:
   - Hero/banner images (full URLs from `<img src>`, CSS `background-image`)
   - Logo images
   - Icons (earning icons, social icons, decorative)
   - Tier badge images
   - Background patterns or textures
   - Any other visual assets

   Present the asset inventory as a table:

   | Asset | URL | Suggested use |
   |-------|-----|---------------|
   | Hero banner | `https://example.com/hero.jpg` | Hero section background |
   | Gold badge | `https://example.com/gold.png` | Tier 3 badge via CSS `:nth-child()` |
   | Instagram icon | `https://example.com/ig.svg` | Social earning icon |

   **Ask the user** which assets to reuse directly (by URL) and which need replacements.

5. **Font detection** — Identify web fonts used (Google Fonts, Typekit, etc.) and their weights

If `WebFetch` fails (auth wall, SPA, etc.), ask the user for a screenshot or manual description.

### Step 2: Plan the template

Present the user with:

- Proposed **color palette** (hex values) and **font stack**
- **Section layout** plan (which sections, in what order)
- **Asset mapping** — which reference assets will be used, which need replacements
- **Earning icon mapping** — one image per earning type (see `references/template-engine.md`)
- **Tier badge mapping** — by position with CSS `:nth-child()`
- Static banners or sections to include
- Which **widget routes** to use for CTAs (see `references/template-engine.md`)

Ask for confirmation before proceeding.

### Step 3: Generate the template

Read the reference files before generating:

- `${CLAUDE_PLUGIN_ROOT}/skills/loyalty-template/references/template-engine.md` — Earning types, CTA routes, data attributes
- `${CLAUDE_PLUGIN_ROOT}/skills/loyalty-template/references/template-structure.md` — HTML skeleton, critical rules, BEM naming

Also read the existing example template for reference patterns:

- Use `Read` on the `siwon-copy.liquid` template in the `shopify-extensions` repo at `theme-extensions/loyalty-page/templates/siwon-copy.liquid` (ask the user for the path if not found)

Generate a single self-contained `.liquid` file following all the rules in the reference files.

### Step 4: Verify (if in shopify-extensions repo)

If you're working in the `shopify-extensions` repo:

1. Run `npm run lint` — must pass
2. Run `npm run build:theme-extensions` — must compile
3. Confirm the file exists at `theme-extensions/loyalty-page/templates/<name>.liquid`

If you're NOT in that repo, remind the user to run lint and build manually after copying the file.

## Reference Files

- `references/template-engine.md` — Earning types, CTA widget routes, data attributes, variable interpolation
- `references/template-structure.md` — HTML skeleton, critical implementation rules, BEM naming, responsive requirements
