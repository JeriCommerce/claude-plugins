# Article Structure & Patterns

## Standard Article Template

Every guide follows this markdown structure:

```markdown
---
title: Guide Title
description: Brief description of what this guide covers.
tags: [tag1, tag2]
---

Introductory paragraph explaining what this guide covers and who it's for.

## First Section

### Subsection Title

Content...

---

## Second Section

### Subsection Title

Content...
```

## Heading Hierarchy

| Markdown | Usage |
|---|---|
| `##` (h2) | Major sections |
| `###` (h3) | Subsections within an h2 |
| `####` (h4) | Sub-subsections (rare) |

Never use `#` (h1) in the article body — it is reserved for the page title generated from frontmatter.

## Separators

Use `---` (horizontal rule) between major sections for visual breathing room. Do not place a separator before the very first section or between a subsection and its parent section.

## Code Blocks

### Fenced code blocks

Use triple backticks with a language identifier:

````markdown
```json
{
  "program": "loyalty",
  "points_ratio": 0.15
}
```
````

### Code groups

Use `<CodeGroup>` with `title` attributes on fenced blocks:

````markdown
<CodeGroup>

```javascript title="Node.js"
const client = new JeriCommerce({ apiKey: 'sk_...' });
```

```python title="Python"
client = JeriCommerce(api_key='sk_...')
```

</CodeGroup>
````

### Inline code

Use backticks for variable names, CSS properties, short snippets, file paths, and config values:

```markdown
Set the `data-shop` attribute on the root element.
```

## Lists

### Unordered lists

```markdown
- **Bold label** — Description text explaining the point.
- **Another label** — More explanation.
```

### Ordered lists (steps)

```markdown
1. **Step name** in Shopify Admin → Online Store → Pages.
2. **Next step** — explanation of what to do.
```

For multi-step processes, prefer the `<Steps>` component over numbered lists.

## Links

### Cross-linking guides

```markdown
[Guide Title](/category/slug)
```

Use relative paths to link between guides in the docs site.

### External links

```markdown
[Link text](https://example.com)
```

## Images

Images use markdown syntax with WebP format:

```markdown
![Alt text description](/assets/images/filename.webp)Caption text here
```

- Images are automatically centered
- Text immediately after the image tag on the same line becomes a gray caption
- Save images to `assets/images/`
- Use WebP format (run `python scripts/optimize-images.py` to convert)

## Voice & Tone

The JeriCommerce guide voice is **friendly, action-oriented, and empowering** — it reads like a knowledgeable colleague walking you through something, not a corporate manual.

### Core principles

- **Conversational yet professional** — Approachable without being casual. Avoid jargon when a simpler word works, but don't shy away from technical terms when they're the right ones. Introduce new concepts with a brief definition the first time.
- **Imperative and direct** — Lead with verbs: "Name your campaign", "Configure your store settings", "Select the audience segment". Not "You should configure..." or "The user needs to...".
- **Short sentences, short paragraphs** — Keep sentences punchy and scannable. If a sentence runs past two lines, split it. One idea per paragraph. Use bullet lists and numbered steps liberally.
- **Show, then tell** — Lead with what to do, then explain why if needed. Screenshots and code examples should appear right after the instruction they illustrate, not in a separate section.
- **Empowering, not hand-holding** — Assume the reader is capable. Offer escape hatches (links to related guides, support contact) but don't over-explain basics. Phrases like "You're all set!" reinforce confidence.
- **Problem-solution framing** — When introducing a feature, start with the real-world need it solves, not the feature name. "Plan and send wallet notifications at a specific date and time to announce offers, launches, or events" beats "Scheduled Campaigns is a feature that...".

### Language rules

- **Language**: English only
- **Address**: Use imperative mood primarily. "You" / "your" sparingly for context ("your store", "your customers"), never "we" when referring to the reader.
- **JeriCommerce voice**: "we" is acceptable when speaking as JeriCommerce ("One of JeriCommerce's missions is to simplify..."). This creates a collaborative, partner-like feel.
- **Labels and UI references**: Bold the exact UI label — **Earning Flows**, **Wallet Pass Design**. Use `→` for navigation paths: "JeriCommerce → Loyalty → Rewards" or "Shopify Admin → Online Store → Pages".
- **Notes and warnings**: Use `<Note>`, `<Warning>`, `<Tip>`, or `<Info>` callout components. Avoid ALL CAPS or alarm language.
- **Closing a guide**: End with a brief, encouraging wrap-up. A single line like "You're all set!" or "Your campaign is ready to go." works well.

### Audience

- **Primary**: Merchants and their development teams setting up or managing JeriCommerce features on Shopify.
- **Assumed knowledge**: Basic Shopify admin navigation, general understanding of loyalty programs and ecommerce concepts. Do NOT assume HTML/CSS knowledge unless the guide is specifically developer-focused (e.g., custom page templates).
- **Varying expertise**: Offer multiple entry points when relevant ("from the onboarding flow or the JeriCommerce admin"). Mark advanced sections with "Optional Add-on" or similar headers.
