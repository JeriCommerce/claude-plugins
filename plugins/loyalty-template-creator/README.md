# Loyalty Template Creator Plugin

Create CDN loyalty page templates for Shopify theme extensions by replicating the visual style of a reference website.

## What it does

Automates the creation of self-contained `.liquid` templates powered by the JeriCommerce loyalty-page JS engine. Given a reference website URL, it extracts the visual style (colors, fonts, layout, assets) and generates a complete template with earning sections, tier displays, CTAs, loading skeletons, and responsive breakpoints.

## Prerequisites

- Access to the `shopify-extensions` repository (for writing templates and running lint/build)
- `WebFetch` tool available (for analyzing reference pages and fetching engine docs)

## Usage

### Cowork mode (plugin installed)

```
/create-template mystore https://example.com/loyalty
/create-template brand-name https://brand.com/rewards
```

### Chat mode

Ask Claude to create a loyalty page template and provide the reference URL. The skill activates automatically.

## Components

| Component | Name | Purpose |
|-----------|------|---------|
| Command | `/create-template` | Entry point — starts the template creation workflow |
| Skill | `loyalty-template` | Engine docs, template structure, earning types, CTA routes, asset extraction |
| Skill | `shopify-setup-guide` | Step-by-step guide to add a template to Shopify (CDN or theme block) |

## Workflow

1. **Environment setup** — Choose staging or production CDN, fetch canonical engine docs
2. **Analyze reference** — Extract colors, fonts, layout, and build an asset inventory with URLs
3. **Plan template** — Present proposed palette, sections, asset mapping for user approval
4. **Generate template** — Create a self-contained `.liquid` file following all engine rules
5. **Verify** — Run lint and build (if in the `shopify-extensions` repo)

## Shopify Setup

After creating a template, ask Claude "how do I add this to Shopify?" — the `shopify-setup-guide` skill activates automatically and walks through:

- Adding the section to the theme (`sections/` folder or theme editor)
- Creating a page template (`templates/page.loyalty.json`)
- Creating a Shopify page and adding it to navigation
- Configuring settings and verifying the live page
- Troubleshooting common issues
