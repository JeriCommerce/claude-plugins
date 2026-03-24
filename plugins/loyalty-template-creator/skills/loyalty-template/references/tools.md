# Required Tools

This skill uses built-in tools only:

- `WebFetch` — Fetch reference pages and documentation
- `Read` — Read existing templates for reference
- `Write` — Create the template file
- `Bash` — Run lint and build (when in the `shopify-extensions` repo)

## CDN URLs by Environment

| Environment | CDN URL |
|-------------|---------|
| **Staging** | `https://s.cdn.jericommerce.com/shopify/loyalty-page.js` |
| **Production** | `https://cdn.jericommerce.com/shopify/loyalty-page.js` |

## Canonical Documentation Sources

Fetch these using `WebFetch` at the start of each session. If anything in the skill references conflicts with the live docs, **the docs win**.

1. **Template engine docs**: `https://support.jericommerce.com/storefront/loyalty-page-building-custom-templates-with-cdn-scripts/`
   - Covers: root element, data attributes, variable interpolation, earning/tier fields, loading states, section structure, icon system, CSS classes

2. **Widget routes docs**: `https://support.jericommerce.com/storefront/loyalty-page-setting-up-and-customizing-the-default-template/`
   - Covers: all `#jeri=` hash fragment routes for CTAs
