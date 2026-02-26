# Article Structure & Patterns

## Standard Article Template

Every guide follows this HTML structure:

```html
<p>Introductory paragraph explaining what this guide covers and who it's for.</p>
<p>‍</p><hr><p>‍</p>
<h3>First Section</h3>
<h4>Subsection Title</h4>
<p>Content...</p>
<p>‍</p><hr><p>‍</p>
<h3>Second Section</h3>
<h4>Subsection Title</h4>
<p>Content...</p>
<!-- ... more sections ... -->
```

## Section Separator Pattern

Between every `<h3>` section, insert:

```html
<p>‍</p><hr><p>‍</p>
```

- `‍` = zero-width joiner (U+200D) — creates vertical breathing room
- Do NOT place a separator before the very first `<h3>` if it immediately follows the intro paragraph — place it after the intro instead
- Do NOT place a separator between an `<h4>` and its parent `<h3>` — the separator is between major sections only

## Heading Hierarchy

| Tag | Usage |
|-----|-------|
| `<h3>` | Major sections (Table of Contents level) |
| `<h4>` | Subsections within an `<h3>` |
| `<h5>` | Rare — sub-subsections |

Never use `<h1>` or `<h2>` — these are reserved by the Webflow page layout.

## Code Blocks

### Multi-line code — Styled with `jeri-code-block`

All multi-line code uses the Webflow embed wrapper with custom CSS (dark theme). See `references/code-block-css.md` for full CSS.

```html
<div data-rt-embed-type='true'><style>.jeri-code-block{background-color:#1e1e1e;color:#d4d4d4;border-radius:12px;padding:20px 24px;overflow-x:auto;font-family:'SF Mono',SFMono-Regular,Consolas,'Liberation Mono',Menlo,Courier,monospace;font-size:13px;line-height:1.6;white-space:pre;border:1px solid rgba(143,143,143,0.15);margin:0}</style><pre class="jeri-code-block">&lt;div id="jeri-loyalty-page-root"
  class="jeri-loyalty-page"
  data-shop="{{ shop.permanent_domain }}"&gt;
  &lt;!-- Your custom HTML here --&gt;
&lt;/div&gt;</pre></div>
```

Rules:
- Wrap in `<div data-rt-embed-type='true'>` — same as tables
- Include the full `<style>` block in every code embed (scoped)
- Use `<pre class="jeri-code-block">` — NOT `<pre><code>`
- Escape HTML entities: `<` → `&lt;`, `>` → `&gt;`, `&` → `&amp;`
- CSS content does NOT need escaping
- Preserve indentation with spaces (not tabs)
- Use `\n` for line breaks when building content programmatically

### Inline code

```html
<p>Set the <code>data-shop</code> attribute on the root element.</p>
```

Use plain `<code>` for: variable names, CSS properties, short snippets, file paths, config values. No embed wrapper needed for inline code.

## Lists

### Unordered lists

```html
<ul>
  <li><strong>Bold label</strong> — Description text explaining the point.</li>
  <li><strong>Another label</strong> — More explanation.</li>
</ul>
```

### Ordered lists (steps)

```html
<ol start="1">
  <li><strong>Step name</strong> in Shopify Admin → Online Store → Pages.</li>
  <li><strong>Next step</strong> — explanation of what to do.</li>
</ol>
```

Always include `start="1"` on `<ol>` elements.

## Links

### Cross-linking guides

```html
<a href="https://www.jericommerce.com/guides/SLUG">Guide Title</a>
```

Link to related JeriCommerce guides when referencing concepts covered in other articles.

### External links

```html
<a href="https://example.com">Link text</a>
```

## Images

Webflow CMS supports images in rich text via the standard figure pattern:

```html
<figure class="w-richtext-figure-type-image w-richtext-align-center" data-rt-type="image" data-rt-align="center">
  <div><img src="URL" loading="lazy" alt="Description" width="auto" height="auto"></div>
  <figcaption>Caption text</figcaption>
</figure>
```

## Voice & Tone

The JeriCommerce guide voice is **friendly, action-oriented, and empowering** — it reads like a knowledgeable colleague walking you through something, not a corporate manual.

### Core principles

- **Conversational yet professional** — Approachable without being casual. Avoid jargon when a simpler word works, but don't shy away from technical terms when they're the right ones. Introduce new concepts with a brief definition the first time.
- **Imperative and direct** — Lead with verbs: "Name your campaign", "Configure your store settings", "Select the audience segment". Not "You should configure..." or "The user needs to...".
- **Short sentences, short paragraphs** — Keep sentences punchy and scannable. If a sentence runs past two lines, split it. One idea per paragraph. Use bullet lists and numbered steps liberally.
- **Show, then tell** — Lead with what to do, then explain why if needed. Screenshots and code examples should appear right after the instruction they illustrate, not in a separate section.
- **Empowering, not hand-holding** — Assume the reader is capable. Offer escape hatches (links to related guides, support contact) but don't over-explain basics. Phrases like "You are all set up and ready to launch!" reinforce confidence.
- **Problem-solution framing** — When introducing a feature, start with the real-world need it solves, not the feature name. "Plan and send wallet notifications at a specific date and time to announce offers, launches, or events" beats "Scheduled Campaigns is a feature that...".

### Language rules

- **Language**: English only
- **Address**: Use imperative mood primarily. "You" / "your" sparingly for context ("your store", "your customers"), never "we" when referring to the reader.
- **JeriCommerce voice**: "we" is acceptable when speaking as JeriCommerce ("One of JeriCommerce's missions is to simplify..."). This creates a collaborative, partner-like feel.
- **Labels and UI references**: Bold the exact UI label — **Earning Flows**, **Wallet Pass Design**. Use `→` for navigation paths: "JeriCommerce > Loyalty > Rewards" or "Shopify Admin → Online Store → Pages".
- **Notes and warnings**: Use a simple blockquote or bold "Note:" prefix. Avoid ALL CAPS or alarm language. Placement and whitespace make them visible enough.
- **Closing a guide**: End with a brief, encouraging wrap-up. A single line like "You're all set!" or "Your campaign is ready to go." works well.

### Audience

- **Primary**: Merchants and their development teams setting up or managing JeriCommerce features on Shopify.
- **Assumed knowledge**: Basic Shopify admin navigation, general understanding of loyalty programs and ecommerce concepts. Do NOT assume HTML/CSS knowledge unless the guide is specifically developer-focused (e.g., custom page templates).
- **Varying expertise**: Offer multiple entry points when relevant ("from the onboarding flow or the JeriCommerce admin"). Mark advanced sections with "Optional Add-on" or similar headers.
