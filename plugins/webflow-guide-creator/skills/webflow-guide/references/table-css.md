# Table CSS & HTML Structure

## Complete Table CSS

This CSS block must be included inside a `<style>` tag within every `<div data-rt-embed-type='true'>` wrapper. Webflow scopes rich text embeds, so each table needs its own copy.

```css
.jeri-table-container{width:100%;overflow-x:auto;border:1px solid rgba(143,143,143,0.2);border-radius:12px;background:#ffffff}
.jeri-table{width:100%;border-collapse:collapse;text-align:left;font-size:15px;min-width:600px}
.jeri-table thead th{background-color:#fafafa;color:rgb(49,49,49);font-weight:600;font-size:12px;text-transform:uppercase;letter-spacing:0.05em;padding:12px 24px;border-bottom:1px solid rgba(143,143,143,0.2);opacity:0.8}
.jeri-table td{padding:12px 24px;border-bottom:1px solid rgba(143,143,143,0.15);vertical-align:middle}
.section-header td{background-color:#ffffff;font-weight:600;font-size:16px;padding-top:24px;padding-bottom:8px;border-bottom:none}
.path-badge{background-color:rgba(143,143,143,0.15);color:rgb(49,49,49);padding:5px 10px;border-radius:6px;font-size:13px;font-weight:500;display:inline-block;white-space:nowrap}
.details-text{line-height:1.4;opacity:0.9}
.jeri-table tbody tr:last-child td{border-bottom:none}
```

## Minified CSS (copy-paste ready)

Use this single-line version inside the `<style>` tag:

```
.jeri-table-container{width:100%;overflow-x:auto;border:1px solid rgba(143,143,143,0.2);border-radius:12px;background:#ffffff}.jeri-table{width:100%;border-collapse:collapse;text-align:left;font-size:15px;min-width:600px}.jeri-table thead th{background-color:#fafafa;color:rgb(49,49,49);font-weight:600;font-size:12px;text-transform:uppercase;letter-spacing:0.05em;padding:12px 24px;border-bottom:1px solid rgba(143,143,143,0.2);opacity:0.8}.jeri-table td{padding:12px 24px;border-bottom:1px solid rgba(143,143,143,0.15);vertical-align:middle}.section-header td{background-color:#ffffff;font-weight:600;font-size:16px;padding-top:24px;padding-bottom:8px;border-bottom:none}.path-badge{background-color:rgba(143,143,143,0.15);color:rgb(49,49,49);padding:5px 10px;border-radius:6px;font-size:13px;font-weight:500;display:inline-block;white-space:nowrap}.details-text{line-height:1.4;opacity:0.9}.jeri-table tbody tr:last-child td{border-bottom:none}
```

## Complete Table Template

### Basic table (no section headers)

```html
<div data-rt-embed-type='true'><style>.jeri-table-container{width:100%;overflow-x:auto;border:1px solid rgba(143,143,143,0.2);border-radius:12px;background:#ffffff}.jeri-table{width:100%;border-collapse:collapse;text-align:left;font-size:15px;min-width:600px}.jeri-table thead th{background-color:#fafafa;color:rgb(49,49,49);font-weight:600;font-size:12px;text-transform:uppercase;letter-spacing:0.05em;padding:12px 24px;border-bottom:1px solid rgba(143,143,143,0.2);opacity:0.8}.jeri-table td{padding:12px 24px;border-bottom:1px solid rgba(143,143,143,0.15);vertical-align:middle}.path-badge{background-color:rgba(143,143,143,0.15);color:rgb(49,49,49);padding:5px 10px;border-radius:6px;font-size:13px;font-weight:500;display:inline-block;white-space:nowrap}.details-text{line-height:1.4;opacity:0.9}.jeri-table tbody tr:last-child td{border-bottom:none}</style><div class="jeri-table-container"><table class="jeri-table"><thead><tr><th>Column 1</th><th>Column 2</th><th>Column 3</th></tr></thead><tbody><tr><td><span class="path-badge">field.name</span></td><td>Type</td><td class="details-text">Description of the field</td></tr></tbody></table></div></div>
```

### Table with section headers

Section headers group rows visually. Use `class="section-header"` on the `<tr>` and `colspan` on the `<td>`:

```html
<tr class="section-header"><td colspan="4">Section Name</td></tr>
```

Full example:

```html
<div data-rt-embed-type='true'><style>.jeri-table-container{width:100%;overflow-x:auto;border:1px solid rgba(143,143,143,0.2);border-radius:12px;background:#ffffff}.jeri-table{width:100%;border-collapse:collapse;text-align:left;font-size:15px;min-width:600px}.jeri-table thead th{background-color:#fafafa;color:rgb(49,49,49);font-weight:600;font-size:12px;text-transform:uppercase;letter-spacing:0.05em;padding:12px 24px;border-bottom:1px solid rgba(143,143,143,0.2);opacity:0.8}.jeri-table td{padding:12px 24px;border-bottom:1px solid rgba(143,143,143,0.15);vertical-align:middle}.section-header td{background-color:#ffffff;font-weight:600;font-size:16px;padding-top:24px;padding-bottom:8px;border-bottom:none}.path-badge{background-color:rgba(143,143,143,0.15);color:rgb(49,49,49);padding:5px 10px;border-radius:6px;font-size:13px;font-weight:500;display:inline-block;white-space:nowrap}.details-text{line-height:1.4;opacity:0.9}.jeri-table tbody tr:last-child td{border-bottom:none}</style><div class="jeri-table-container"><table class="jeri-table"><thead><tr><th>Setting</th><th>Type</th><th>Default</th><th>Description</th></tr></thead><tbody><tr class="section-header"><td colspan="4">Content</td></tr><tr><td><span class="path-badge">Hero Title</span></td><td>Text</td><td>"Our Loyalty Program"</td><td class="details-text">Main heading displayed at the top</td></tr><tr class="section-header"><td colspan="4">Colors</td></tr><tr><td><span class="path-badge">Primary Color</span></td><td>Color</td><td><code>#0f0f0f</code></td><td class="details-text">Buttons, accents, and badges</td></tr></tbody></table></div></div>
```

## CSS Class Reference

| Class | Used On | Purpose |
|-------|---------|---------|
| `jeri-table-container` | Outer `<div>` | Responsive scroll wrapper with rounded border |
| `jeri-table` | `<table>` | Base table styling |
| `section-header` | `<tr>` | Group header row (bold, no bottom border) |
| `path-badge` | `<span>` | Monospace-style badge for property/field names |
| `details-text` | `<td>` | Description cells with muted line-height |

## Important Notes

- The `data-rt-embed-type='true'` attribute on the outer `<div>` is **mandatory** — without it, Webflow strips the custom HTML from rich text fields
- Every table must include its own `<style>` block — CSS is not shared between embeds
- Use single quotes for the `data-rt-embed-type` attribute value (`'true'` not `"true"`)
- The `.section-header` CSS is only needed if the table uses section header rows — include it anyway for consistency
