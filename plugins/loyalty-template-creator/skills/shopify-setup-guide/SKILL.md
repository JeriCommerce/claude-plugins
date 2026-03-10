---
name: Jeri - Shopify Setup Guide
description: >
  Use when the user asks to "add the template to Shopify", "deploy the loyalty page",
  "install the template", "set up the loyalty page in Shopify", "how to use the template",
  "configure the loyalty page", or needs guidance on adding a CDN loyalty page template
  or the default theme app extension block to a Shopify store.
version: 202603.10.0
---

# Shopify Loyalty Page Setup Guide

Guide the user through adding a loyalty page template to their Shopify store.

## Required Tools

- No MCP tools required — this is a guidance-only skill
- Optionally use `WebFetch` to verify the page is live after setup

## Workflow

### Step 1: Determine the template type

Ask the user which type of template they want to set up:

| Type | When to use | Setup complexity |
|------|-------------|-----------------|
| **CDN template** (custom `.liquid`) | After creating a custom template with the `loyalty-template` skill | Medium — requires adding code to the theme |
| **Theme app extension block** | Using the default JeriCommerce loyalty page | Easy — no code, just theme editor |

### Step 2a: CDN template setup (custom `.liquid`)

This is the most common flow after creating a template with the `loyalty-template` skill.

Read the detailed steps from `${CLAUDE_PLUGIN_ROOT}/skills/shopify-setup-guide/references/setup-steps.md`.

Guide the user through:

1. **Add the section to the theme** — Copy the `.liquid` file into the theme's `sections/` folder
2. **Create a page template** — Create `templates/page.loyalty.json` referencing the section
3. **Create a Shopify page** — In Shopify Admin, create a page using the "loyalty" template
4. **Add to navigation** — Add the page to the store's main menu or footer
5. **Configure via theme editor** — Open the theme editor to adjust settings (colors, CTAs, visibility)
6. **Verify** — Check the live page loads correctly

For each step, provide the exact instructions and any code snippets needed. Adapt the section name based on the template the user created (e.g., if the template is `mystore.liquid`, the section name in the JSON would be `mystore`).

### Step 2b: Theme app extension block setup (default)

Guide the user through the simpler flow:

1. **Create a page** — Shopify Admin → Online Store → Pages → Add page
2. **Open theme editor** — Customize → Pages → select the page
3. **Add the block** — Click "Add block" → find "JeriCommerce Loyalty Page" under Apps
4. **Configure settings** — Adjust titles, colors, CTA behavior, section visibility
5. **Save and publish**

### Step 3: Verification checklist

After setup, walk through this checklist with the user:

- [ ] Page loads without errors (check browser console)
- [ ] Earning rules appear (purchases, social, referral, etc.)
- [ ] Tier cards display correctly (if program has tiers)
- [ ] CTA button works for both guest and logged-in states
- [ ] Mobile layout is responsive
- [ ] Loading skeleton shows briefly before data loads
- [ ] Widget routes work (`#jeri=loyalty/rewards` opens the widget)

### Common Issues

| Issue | Cause | Fix |
|-------|-------|-----|
| Blank page, no content | JS not loading | Check CDN script URL, ensure `defer` attribute is present |
| "Section not found" error | Template name mismatch | Verify the section name in `page.loyalty.json` matches the `.liquid` filename |
| Earnings/tiers not showing | Wrong `data-shop` value | Ensure `data-shop="{{ shop.permanent_domain }}"` is set correctly |
| CTA opens nothing | Widget not installed | The JeriCommerce widget must be active on the storefront for `#jeri=` routes to work |
| Styles look wrong | CSS not inline | CDN templates must include all CSS inline — no external stylesheet dependency |
| Theme check warnings | RemoteAsset errors | Wrap `<script>` and root `<div>` with `theme-check-disable/enable RemoteAsset` comments |

## Reference Files

- `references/setup-steps.md` — Detailed step-by-step instructions with code snippets
