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
