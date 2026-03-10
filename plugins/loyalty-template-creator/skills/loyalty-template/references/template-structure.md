# Template Structure Reference

## HTML Skeleton

Every template follows this structure:

```liquid
{%- comment -%} theme-check-disable RemoteAsset {%- endcomment -%}
<script src="https://s.cdn.jericommerce.com/shopify/loyalty-page.js" defer></script>
{%- comment -%} theme-check-enable RemoteAsset {%- endcomment -%}

<style>
  /* All CSS inline — template must be fully self-contained */
</style>

{%- comment -%} theme-check-disable RemoteAsset {%- endcomment -%}
<div id="jeri-loyalty-page-root" class="<prefix> jeri-loyalty-page--loading"
     data-shop="{{ shop.permanent_domain }}"
     data-locale="{{ request.locale.iso_code }}">

  <!-- Hero section (static Liquid — not JS-powered) -->

  <div class="<prefix>__content">
    <!-- Earnings section (JS-powered, data-jeri="section.earnings") -->
    <!-- Optional: static banners -->
    <!-- Tiers section (JS-powered, data-jeri="section.tiers") -->
    <!-- Optional: referral/promo banners -->
  </div>

  <!-- CTA footer -->

  <!-- Loading skeleton (visible while jeri-loyalty-page--loading is active) -->
</div>
{%- comment -%} theme-check-enable RemoteAsset {%- endcomment -%}

{% if section.settings.custom_css != blank %}
  <style>{{ section.settings.custom_css }}</style>
{% endif %}

{% schema %}
{
  "name": "Template Name",
  "settings": [...],
  "presets": [{ "name": "Template Name" }]
}
{% endschema %}
```

## Critical Implementation Rules

1. **Root element** — Must have `id="jeri-loyalty-page-root"`, `data-shop`, and `data-locale`
2. **Loading class** — Add `jeri-loyalty-page--loading` to root; JS removes it when ready
3. **Text interpolation** — `{earning.title}`, `{tier.name}`, etc. in TEXT NODES only (never in attributes)
4. **HTML content** — Use `data-jeri-html="tier.content"` for tier descriptions (may contain HTML)
5. **Custom icons** — Use `data-jeri="earning.type" data-jeri-show-if="<type>"` with hardcoded `src` per image
6. **Tier images** — Use CSS `:nth-child()` + `content: url()` (JS has no tier image injection)
7. **Flatten earnings** — Use `display: contents` on group/container wrappers for a unified grid
8. **Auto-hide** — `data-jeri="section.earnings"` and `data-jeri="section.tiers"` on section wrappers
9. **Schema name** — Max 25 characters. Must include `"presets"` array for "Add section" picker
10. **CDN script** — Use staging (`s.cdn.jericommerce.com`) or production (`cdn.jericommerce.com`) URL
11. **All CSS inline** — No external stylesheet dependency
12. **Theme-check comments** — Wrap `<script>` and root `<div>` with `theme-check-disable/enable RemoteAsset`

## Schema Rules (CRITICAL — must follow exactly)

The `{% schema %}` block defines theme editor settings. Shopify validates it strictly — any violation causes a `FileSaveError` on deploy.

### Setting type rules

| Type | `default` allowed? | `default` format | Notes |
|------|-------------------|------------------|-------|
| `text` | Yes | String | `"default": "My Title"` |
| `textarea` | Yes | String | `"default": "Some text"` |
| `checkbox` | Yes | Boolean | `"default": true` |
| `number` | Yes | Number | `"default": 10` |
| `range` | Yes | Number | Must be within `min`/`max` |
| `select` | Yes | String | Must match one of `options[].value` |
| `color` | Yes | Hex string | `"default": "#0f0f0f"` |
| `url` | **NO** | — | **NEVER set `default` on `url` type settings.** Use Liquid fallback instead: `{{ section.settings.guest_cta_url \| default: '/account/login' }}` |
| `image_picker` | No | — | No default |
| `richtext` | Yes | HTML string | `"default": "<p>Text</p>"` |

### Common schema errors and how to avoid them

| Error | Cause | Fix |
|-------|-------|-----|
| `setting with id="X" default must be a string or datasource access path` | `default` set on a `url` type setting | **Remove the `default` field entirely.** Use Liquid `\| default:` filter in the HTML instead |
| `Invalid schema: name must be 25 characters or less` | Schema `"name"` too long | Shorten to 25 chars max |
| `Invalid schema: presets must be an array` | Missing or malformed `presets` | Add `"presets": [{ "name": "..." }]` |
| `Invalid schema: setting default is invalid` | `default` value doesn't match type constraints | Check type rules above |

### Correct schema pattern

```json
{
  "name": "Jeri Loyalty Brand",
  "class": "jeri-loyalty-brand-section",
  "settings": [
    {
      "type": "header",
      "content": "Hero"
    },
    {
      "type": "text",
      "id": "hero_title",
      "label": "Hero Title",
      "default": "Our Loyalty Program"
    },
    {
      "type": "textarea",
      "id": "hero_subtitle",
      "label": "Hero Subtitle",
      "default": "Join and start earning rewards today."
    },
    {
      "type": "text",
      "id": "guest_cta_text",
      "label": "Guest CTA Text",
      "default": "Join Now"
    },
    {
      "type": "url",
      "id": "guest_cta_url",
      "label": "Guest CTA URL",
      "info": "Where to redirect guests. Leave blank for /account/login"
    },
    {
      "type": "header",
      "content": "Sections"
    },
    {
      "type": "checkbox",
      "id": "show_earning_rules",
      "label": "Show Earnings Section",
      "default": true
    },
    {
      "type": "checkbox",
      "id": "show_tiers",
      "label": "Show Tiers Section",
      "default": true
    },
    {
      "type": "header",
      "content": "Logged-in CTA"
    },
    {
      "type": "text",
      "id": "logged_in_cta_label",
      "label": "Logged-in CTA Label",
      "default": "See Rewards",
      "info": "Opens the rewards widget when clicked."
    },
    {
      "type": "header",
      "content": "Colors"
    },
    {
      "type": "color",
      "id": "primary_color",
      "label": "Primary Color",
      "default": "#000000"
    },
    {
      "type": "color",
      "id": "accent_color",
      "label": "Accent Color",
      "default": "#FFFF42"
    },
    {
      "type": "header",
      "content": "Advanced"
    },
    {
      "type": "textarea",
      "id": "custom_css",
      "label": "Custom CSS"
    }
  ],
  "presets": [
    {
      "name": "Jeri Loyalty Brand"
    }
  ]
}
```

### URL fallback pattern in HTML

Since `url` settings cannot have defaults, always use Liquid `| default:` in the template HTML:

```liquid
<a href="{{ section.settings.guest_cta_url | default: '/account/login' }}">
  {{ section.settings.guest_cta_text | default: 'Join Now' }}
</a>
```

## BEM Naming

Use a unique prefix per template in `kebab-case`:

```css
.siwon-copy { }              /* block = root */
.siwon-copy__hero { }        /* element */
.siwon-copy__hero--dark { }  /* modifier */
```

- Prefix = template name (e.g., `.mystore-`, `.brand-name-`)
- Add the prefix as a class on the root element alongside `jeri-loyalty-page--loading`
- All CSS selectors scoped under this prefix to avoid conflicts

## Responsive Design

Include mobile breakpoints:

```css
@media (max-width: 768px) {
  /* Tablet adjustments */
}

@media (max-width: 480px) {
  /* Mobile adjustments */
}
```

Key responsive patterns:
- Grid columns collapse (3 → 2 → 1)
- Font sizes reduce
- Padding/margins shrink
- Hero images may change aspect ratio or hide
- Tier cards stack vertically

## Loading Skeleton

Include a skeleton UI visible only while `jeri-loyalty-page--loading` is active:

```css
.jeri-loyalty-page--loading .prefix__content { display: none; }
.jeri-loyalty-page--loading .prefix__skeleton { display: block; }
.prefix__skeleton { display: none; } /* hidden once loaded */
```

## Example Template

For a complete working example with custom icons, tier images, static banners, and all engine features, see `siwon-copy.liquid` in the `shopify-extensions` repo:

```
shopify-extensions/theme-extensions/loyalty-page/templates/siwon-copy.liquid
```
