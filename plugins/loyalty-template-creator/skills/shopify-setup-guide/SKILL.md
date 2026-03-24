---
name: Jeri - Shopify Setup Guide
description: >
  Use when the user asks to "add the template to Shopify", "deploy the loyalty page",
  "install the template", "set up the loyalty page in Shopify", "how to use the template",
  "configure the loyalty page", or needs guidance on adding a CDN loyalty page template
  or the default theme app extension block to a Shopify store.
version: 202603.24.0
---

# Shopify Loyalty Page Setup Guide

Guide the user through adding a loyalty page template to their Shopify store. No MCP tools required — this is a guidance-only skill. Optionally use `WebFetch` to verify the page is live after setup.

## Workflow

### Step 1: Determine the template type

Ask the user which type of template they want to set up:

| Type | When to use | Setup complexity |
|------|-------------|-----------------|
| **CDN template** (custom `.liquid`) | After creating a custom template with the `loyalty-template` skill | Medium — requires adding code to the theme |
| **Theme app extension block** | Using the default JeriCommerce loyalty page | Easy — no code, just theme editor |

### Step 2a: CDN template setup (custom `.liquid`)

Read `${CLAUDE_PLUGIN_ROOT}/skills/shopify-setup-guide/references/setup-steps.md` for the detailed step-by-step instructions, code snippets, and troubleshooting guide.

Guide the user through all 6 steps in that file. Adapt the section name based on the template the user created.

### Step 2b: Theme app extension block setup (default)

Read `${CLAUDE_PLUGIN_ROOT}/skills/shopify-setup-guide/references/setup-steps.md` for the theme app extension setup flow (see "Theme App Extension Block Setup" section).

Guide the user through that simpler flow.

### Step 3: Verification

Read `${CLAUDE_PLUGIN_ROOT}/skills/shopify-setup-guide/references/setup-steps.md` for the verification checklist and common issues troubleshooting table.

Walk through the checklist with the user and resolve any issues using the troubleshooting guide.

## Reference Files

- `references/setup-steps.md` — Detailed CDN and theme app extension setup steps, verification checklist, and troubleshooting
