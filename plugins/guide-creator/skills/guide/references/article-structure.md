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

### Target audience

- **Primary**: Marketing teams, store owners, and merchants — people who are smart and capable but **not developers**. This is the audience for 99% of guides.
- **Assumed knowledge**: Basic Shopify admin navigation and general ecommerce concepts. Do NOT assume any knowledge of HTML, CSS, code, APIs, or technical tooling.
- **Technical guides are the exception**: Only write developer-focused content (API, webhooks, custom code) when the topic explicitly requires it. In those rare cases, mark the guide clearly (e.g., "For developers" in the description) and still explain every step.
- **Varying expertise**: Offer multiple entry points when relevant ("from the onboarding flow or the JeriCommerce admin"). Mark advanced or optional sections with headers like "Optional" or "Advanced".

### Core principles

- **Conversational yet professional** — Approachable without being casual. Avoid jargon entirely when a simpler word works. When a technical term is unavoidable, define it briefly the first time ("**Earning flow** — the rule that decides how customers earn points").
- **Imperative and direct** — Lead with verbs: "Name your campaign", "Click **Save**", "Select the audience segment". Not "You should configure..." or "The user needs to...".
- **Short sentences, short paragraphs** — Keep sentences punchy and scannable. If a sentence runs past two lines, split it. One idea per paragraph. Use bullet lists and numbered steps liberally.
- **Show, then tell** — Lead with what to do, then explain why if needed. Screenshots should appear right after the instruction they illustrate, not in a separate section. A picture of the right button is worth a paragraph of instructions.
- **Slow down on technical steps** — When something involves a setting, integration, toggle, or anything slightly technical, use `<Steps>` with screenshots. Number every click. Describe exactly what the user should see at each stage. Never say "just configure X" — show how.
- **Empowering, never intimidating** — Assume the reader is smart but not technical. Never make them feel dumb for not knowing something. Use encouraging phrasing: "You're all set!", "That's it — your campaign is live."
- **Problem-solution framing** — When introducing a feature, start with the real-world need it solves, not the feature name. "Reward your customers for every purchase they make" beats "Earning Flows is a feature that...".
- **No unnecessary code** — Avoid showing code blocks, JSON, or API calls in non-developer guides. If a setting has a technical name in the UI, bold it and explain what it does in plain language.

### Language rules

- **Language**: English only
- **Address**: Use imperative mood primarily. "You" / "your" sparingly for context ("your store", "your customers"), never "we" when referring to the reader.
- **JeriCommerce voice**: "we" is acceptable when speaking as JeriCommerce ("One of JeriCommerce's missions is to simplify..."). This creates a collaborative, partner-like feel.
- **Labels and UI references**: Bold the exact UI label — **Earning Flows**, **Wallet Pass Design**. Use `→` for navigation paths: "JeriCommerce → Loyalty → Rewards" or "Shopify Admin → Online Store → Pages".
- **Notes and warnings**: Use `<Note>`, `<Warning>`, `<Tip>`, or `<Info>` callout components. Avoid ALL CAPS or alarm language. Use `<Tip>` generously to share best practices in plain language.
- **Closing a guide**: End with a brief, encouraging wrap-up. A single line like "You're all set!" or "Your campaign is ready to go." works well.
- **Avoid developer language**: Don't say "configure", "implement", "deploy", "parameter", "endpoint", "payload" in non-dev guides. Say "set up", "turn on", "choose", "option", "setting" instead.
