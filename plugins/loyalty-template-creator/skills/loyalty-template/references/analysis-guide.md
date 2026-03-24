# Reference Page Analysis Guide

## What to Extract

When analyzing the reference URL with `WebFetch`, extract:

### 1. Visual Style
- Colors, fonts, layout, spacing, border-radius

### 2. Section Structure
- Hero, earnings, tiers, banners, CTAs

### 3. Color Palette
- Primary, accent, background, text, muted colors

### 4. Asset Inventory

Collect and list ALL reusable asset URLs found on the page:
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

### 5. Font Detection
- Identify web fonts used (Google Fonts, Typekit, etc.) and their weights

## When WebFetch Fails

If `WebFetch` fails (auth wall, SPA, etc.), ask the user for a screenshot or manual description.
