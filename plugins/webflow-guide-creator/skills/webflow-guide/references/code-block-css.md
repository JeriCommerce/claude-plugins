# Code Block CSS & HTML Structure

## Complete Code Block CSS

This CSS block must be included inside a `<style>` tag within every `<div data-rt-embed-type='true'>` wrapper for code blocks. Same embed pattern as tables — Webflow rich text only renders custom HTML inside this wrapper.

```css
.jeri-code-block{background-color:#1e1e1e;color:#d4d4d4;border-radius:12px;padding:20px 24px;overflow-x:auto;font-family:'SF Mono',SFMono-Regular,Consolas,'Liberation Mono',Menlo,Courier,monospace;font-size:13px;line-height:1.6;white-space:pre;border:1px solid rgba(143,143,143,0.15);margin:0}
```

## Minified CSS (copy-paste ready)

Use this single-line version inside the `<style>` tag:

```
.jeri-code-block{background-color:#1e1e1e;color:#d4d4d4;border-radius:12px;padding:20px 24px;overflow-x:auto;font-family:'SF Mono',SFMono-Regular,Consolas,'Liberation Mono',Menlo,Courier,monospace;font-size:13px;line-height:1.6;white-space:pre;border:1px solid rgba(143,143,143,0.15);margin:0}
```

## Complete Code Block Template

```html
<div data-rt-embed-type='true'><style>.jeri-code-block{background-color:#1e1e1e;color:#d4d4d4;border-radius:12px;padding:20px 24px;overflow-x:auto;font-family:'SF Mono',SFMono-Regular,Consolas,'Liberation Mono',Menlo,Courier,monospace;font-size:13px;line-height:1.6;white-space:pre;border:1px solid rgba(143,143,143,0.15);margin:0}</style><pre class="jeri-code-block">&lt;div id="jeri-loyalty-page-root"&gt;
  &lt;!-- Your custom HTML here --&gt;
&lt;/div&gt;</pre></div>
```

## Rules

- Wrap every multi-line code block in `<div data-rt-embed-type='true'>` — same as tables
- Include the full `<style>` block in every code embed (styles are scoped per embed)
- Use `<pre class="jeri-code-block">` — NOT `<pre><code>`, just `<pre>` with the class
- Escape all HTML entities inside `<pre>`: `&lt;` for `<`, `&gt;` for `>`, `&amp;` for `&`
- Use single quotes for `data-rt-embed-type` attribute value (`'true'`)
- Line breaks inside `<pre>` are literal newlines (or `\n` when building programmatically)
- For inline code within paragraphs, continue using plain `<code>` tags (no embed wrapper needed)

## Examples

### HTML code block

```html
<div data-rt-embed-type='true'><style>.jeri-code-block{background-color:#1e1e1e;color:#d4d4d4;border-radius:12px;padding:20px 24px;overflow-x:auto;font-family:'SF Mono',SFMono-Regular,Consolas,'Liberation Mono',Menlo,Courier,monospace;font-size:13px;line-height:1.6;white-space:pre;border:1px solid rgba(143,143,143,0.15);margin:0}</style><pre class="jeri-code-block">&lt;script src="https://cdn.jericommerce.com/shopify/loyalty-page.js" defer&gt;&lt;/script&gt;
&lt;link rel="stylesheet" href="https://cdn.jericommerce.com/shopify/loyalty-page.css" /&gt;</pre></div>
```

### CSS code block

```html
<div data-rt-embed-type='true'><style>.jeri-code-block{background-color:#1e1e1e;color:#d4d4d4;border-radius:12px;padding:20px 24px;overflow-x:auto;font-family:'SF Mono',SFMono-Regular,Consolas,'Liberation Mono',Menlo,Courier,monospace;font-size:13px;line-height:1.6;white-space:pre;border:1px solid rgba(143,143,143,0.15);margin:0}</style><pre class="jeri-code-block">.jeri-loyalty-page {
  --jlp-card-bg: #1a1a1a;
  --jlp-border: #333;
  --jlp-divider: #2a2a2a;
}</pre></div>
```

### JSON code block

```html
<div data-rt-embed-type='true'><style>.jeri-code-block{background-color:#1e1e1e;color:#d4d4d4;border-radius:12px;padding:20px 24px;overflow-x:auto;font-family:'SF Mono',SFMono-Regular,Consolas,'Liberation Mono',Menlo,Courier,monospace;font-size:13px;line-height:1.6;white-space:pre;border:1px solid rgba(143,143,143,0.15);margin:0}</style><pre class="jeri-code-block">{
  "sections": {
    "loyalty": {
      "type": "jeri-loyalty"
    }
  },
  "order": ["loyalty"]
}</pre></div>
```

## Important Notes

- The dark theme (`#1e1e1e` background, `#d4d4d4` text) matches VS Code's default dark theme for developer familiarity
- `border-radius: 12px` matches the `jeri-table-container` radius for visual consistency
- `overflow-x: auto` handles long lines without breaking the page layout
- The `margin: 0` prevents Webflow's default `<pre>` margins from adding unwanted spacing
- CSS content inside `<pre>` does NOT need HTML escaping (only HTML angle brackets need escaping)
