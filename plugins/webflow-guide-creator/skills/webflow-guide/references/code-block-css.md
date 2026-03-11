# Code Block CSS & HTML Structure

## Overview

Code block CSS is loaded globally via Webflow Site Settings → Custom Code → Footer Code. CMS embeds only need a `<pre class="jeri-code-block">` inside the embed wrapper — no inline `<style>` blocks. Syntax highlighting is applied automatically by highlight.js (GitHub light theme). A copy button appears on hover for all code blocks.

## CSS Reference

The following class is available globally:

```css
.jeri-code-block{background-color:#fafafa;border-radius:12px;padding:20px 24px;overflow-x:auto;font-family:'SF Mono',SFMono-Regular,Consolas,'Liberation Mono',Menlo,Courier,monospace;font-size:13px;line-height:1.6;white-space:pre;border:1px solid rgba(143,143,143,0.2);margin:0}
```

Note: no `color` property — highlight.js handles text colors.

## Complete Code Block Template

```html
<div data-rt-embed-type='true'><pre class="jeri-code-block">&lt;div id="jeri-loyalty-page-root"&gt;
  &lt;!-- Your custom HTML here --&gt;
&lt;/div&gt;</pre></div>
```

## Rules

- Wrap every multi-line code block in `<div data-rt-embed-type='true'>` — same as tables
- Do **NOT** include `<style>` blocks — CSS is global
- Use `<pre class="jeri-code-block">` — NOT `<pre><code>`, just `<pre>` with the class
- Escape all HTML entities inside `<pre>`: `&lt;` for `<`, `&gt;` for `>`, `&amp;` for `&`
- Use single quotes for `data-rt-embed-type` attribute value (`'true'`)
- Line breaks inside `<pre>` are literal newlines (or `\n` when building programmatically)
- For inline code within paragraphs, continue using plain `<code>` tags (no embed wrapper needed)

## Examples

### HTML code block

```html
<div data-rt-embed-type='true'><pre class="jeri-code-block">&lt;script src="https://cdn.jericommerce.com/shopify/loyalty-page.js" defer&gt;&lt;/script&gt;
&lt;link rel="stylesheet" href="https://cdn.jericommerce.com/shopify/loyalty-page.css" /&gt;</pre></div>
```

### CSS code block

```html
<div data-rt-embed-type='true'><pre class="jeri-code-block">.jeri-loyalty-page {
  --jlp-card-bg: #1a1a1a;
  --jlp-border: #333;
  --jlp-divider: #2a2a2a;
}</pre></div>
```

### JSON code block

```html
<div data-rt-embed-type='true'><pre class="jeri-code-block">{
  "sections": {
    "loyalty": {
      "type": "jeri-loyalty"
    }
  },
  "order": ["loyalty"]
}</pre></div>
```

## Important Notes

- Light theme (`#fafafa` background). highlight.js applies syntax coloring automatically
- A "Copy" button appears on hover — injected by global JS
- `border-radius: 12px` matches the `jeri-table-container` radius for visual consistency
- `overflow-x: auto` handles long lines without breaking the page layout
- The `margin: 0` prevents Webflow's default `<pre>` margins from adding unwanted spacing
- CSS content inside `<pre>` does NOT need HTML escaping (only HTML angle brackets need escaping)
