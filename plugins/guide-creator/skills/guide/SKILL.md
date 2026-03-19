---
name: Jeri - Guide Creator
description: >
  Use when the user asks to "create a guide", "write a support article",
  "add a guide to the docs", "publish a guide", "update a guide",
  or needs to create or edit content in the JeriCommerce support docs site.
  Covers markdown formatting, frontmatter fields, available components, and article structure.
version: 202603.19.0
---

# Guide Creator

Create and manage support guide articles for the JeriCommerce static docs site.

## Target Repository

JeriCommerce Support Docs — a Python-based static site generator.

- Content lives in the `content/` directory organized by category
- Each guide is a `.md` file with YAML frontmatter
- Images go in `assets/images/` (WebP preferred)
- Build with `python build.py`, output to `dist/`
- Image optimization: `python scripts/optimize-images.py`

## Frontmatter Fields

Every markdown file starts with YAML frontmatter:

```yaml
---
title: Page Title
description: SEO description
sidebarTitle: Short sidebar title (optional, for titles > 40 chars)
noindex: true/false (optional)
tags: [tag1, tag2] (optional)
source: original URL (optional, informational)
---
```

| Field | Required | Description |
|---|---|---|
| `title` | Yes | Page title, shown in header and browser tab |
| `description` | Yes | Meta description for SEO and search |
| `sidebarTitle` | No | Short title for sidebar (use when title > 40 chars) |
| `noindex` | No | Set to `true` to add `noindex, nofollow` meta tag |
| `tags` | No | Array of tags for categorization |
| `source` | No | Original source URL (informational only) |

## Content Categories

Content is organized into subdirectories under `content/`:

| Category | Path |
|---|---|
| Getting Started | `content/getting-started/` |
| Loyalty | `content/loyalty/` |
| — Earning Flows | `content/loyalty/earning-flows/` |
| — Rewards | `content/loyalty/rewards/` |
| — Coupons | `content/loyalty/coupons/` |
| — Tiers | `content/loyalty/tiers/` |
| — Referral | `content/loyalty/referral/` |
| Wallet | `content/wallet/` |
| Notifications | `content/notifications/` |
| Campaigns | `content/campaigns/` |
| Storefront | `content/storefront/` |
| Integrations | `content/integrations/` |
| — Shopify | `content/integrations/shopify/` |
| — Klaviyo | `content/integrations/klaviyo/` |
| — LoyaltyLion | `content/integrations/loyaltylion/` |
| — API | `content/integrations/api/` |
| Customers | `content/customers/` |
| Analytics | `content/analytics/` |

## Available Components

Use these components directly in markdown. For full examples and visual reference, read `${CLAUDE_PLUGIN_ROOT}/skills/guide/references/styleguide.md`.

### Callouts

Five callout types. Use the tag name directly:

```markdown
<Note>
General information or tips that supplement the main content.
</Note>

<Warning>
Important caveats or things that could go wrong.
</Warning>

<Info>
Additional context or background information.
</Info>

<Tip>
Best practices and recommendations.
</Tip>

<Check>
Success states or confirmed information.
</Check>
```

### Cards

Group cards in a `<CardGroup>` with a `cols` attribute:

```markdown
<CardGroup cols="2">
<Card title="Loyalty Cards" icon="wallet" href="/wallet/create-the-assets-for-an-awesome-wallet">
Create and customize wallet passes for your customers.
</Card>
<Card title="Push Notifications" icon="bell" href="/notifications/notification-delivery-strategies">
Send targeted notifications to engage your audience.
</Card>
</CardGroup>
```

Cards without an `href` render as non-clickable containers.

### Tabs

```markdown
<Tabs>
<Tab title="JavaScript">

\`\`\`javascript
const response = await fetch('/api/loyalty/points');
\`\`\`

</Tab>
<Tab title="Python">

\`\`\`python
response = requests.get('/api/loyalty/points')
\`\`\`

</Tab>
</Tabs>
```

### Code Groups

Fenced code blocks with `title` attributes inside a `<CodeGroup>`:

```markdown
<CodeGroup>

\`\`\`javascript title="Node.js"
const client = new JeriCommerce({ apiKey: 'sk_...' });
\`\`\`

\`\`\`python title="Python"
client = JeriCommerce(api_key='sk_...')
\`\`\`

</CodeGroup>
```

### Accordions

```markdown
<AccordionGroup>

<Accordion title="What is JeriCommerce?">
JeriCommerce is a loyalty platform that integrates with Shopify.
</Accordion>

<Accordion title="How do points work?">
Points are credited as a percentage of each purchase.
</Accordion>

</AccordionGroup>
```

### Steps

Use `### Step title` headings inside a `<Steps>` tag:

```markdown
<Steps>

### Create your program

Go to the JeriCommerce dashboard and create a new loyalty program.

### Design your wallet pass

Upload your brand assets and customize the pass layout.

### Launch

Activate your program and start engaging customers.

</Steps>
```

### Frame

Wraps content in a bordered container:

```markdown
<Frame>
Content with a visible border and padding.
</Frame>
```

### Tooltip

```markdown
<Tooltip tip="Explanation text">hover text</Tooltip>
```

### Images

Images are automatically centered. Text immediately after the image tag on the same line becomes a gray caption below:

```markdown
![alt text](/assets/images/file.webp)Caption text here

![alt text](/assets/images/file.webp)
```

Use WebP format. Save images to `assets/images/`.

### Tables

Standard markdown tables:

```markdown
| Column 1 | Column 2 | Column 3 |
|---|---|---|
| Cell | Cell | Cell |
```

### Headings

- `##` (h2) — Major sections
- `###` (h3) — Subsections
- `####` (h4) — Sub-subsections

`#` (h1) is reserved for the page title generated from frontmatter. Never use `#` in the article body.

### Horizontal Rules

Use `---` to create a visual separator between major sections.

## Voice & Tone

All guides must be written in **English**. Read the full voice and tone guide in `${CLAUDE_PLUGIN_ROOT}/skills/guide/references/article-structure.md`. Key points:

- **Target reader: marketing people and merchants** — 99% of guides are for non-technical users. Never assume development knowledge. Avoid code, API references, or technical jargon unless the guide is explicitly developer-focused.
- **Friendly and action-oriented** — reads like a knowledgeable colleague walking you through it, not a manual
- **Imperative and direct** — lead with verbs: "Name your campaign", "Click **Save**"
- **Extra careful on technical steps** — when something involves a setting, integration, or anything slightly technical, slow down. Use `<Steps>` with screenshots, number every click, describe exactly what the user should see at each stage.
- **Short sentences, short paragraphs** — one idea per paragraph, use lists and screenshots liberally
- **Problem-solution framing** — start with the real-world need ("Reward your customers for every purchase") not the feature name ("Earning Flows is a feature that...")
- **Empowering, never intimidating** — assume the reader is smart but not technical. Reinforce confidence ("You're all set!")
- **Bold UI labels** — use `**Label**` for exact UI labels, `→` for navigation paths
- **Language**: English only
- **JeriCommerce voice**: "we" is acceptable when speaking as JeriCommerce. Never "we" for the reader.
- **When in doubt, show a screenshot** — a picture of the right button is worth a paragraph of instructions

## Workflow

### Step 1: Gather information

Ask the user for:
1. **Topic** — What should the guide cover?
2. **Category and tags** — Which content category and tags to assign (show available options)
3. **Source material** — Is there a README, docs, or other source to base the content on?

The default audience is **merchants and marketing teams** — non-technical users. Only ask about technical audience if the topic is explicitly developer-focused (API, webhooks, custom code).

### Step 2: Draft the markdown content

Follow all formatting rules. Read reference files before writing:
- `${CLAUDE_PLUGIN_ROOT}/skills/guide/references/styleguide.md` for available components
- `${CLAUDE_PLUGIN_ROOT}/skills/guide/references/article-structure.md` for content patterns and voice

### Step 3: Write the file

Write the markdown file to `content/{category}/slug.md` with proper YAML frontmatter.

### Step 4: Handle images

Save images to `assets/images/`. Run `python scripts/optimize-images.py` to convert to WebP.

### Step 5: Verify the build

Run `python build.py` to verify the build succeeds and the new guide renders correctly.

### Step 6: Show preview and next steps

Show the user:
- The file path of the new guide
- A preview link if available

Remind the user to **add the page path to `docs.json` navigation** if the guide should appear in the sidebar.

## Navigation

After creating a guide file, the page path must be added to `docs.json` for it to appear in the sidebar navigation. Remind the user of this step every time.

## Reference Files

- `references/styleguide.md` — Complete component reference with examples
- `references/article-structure.md` — Article template, heading hierarchy, voice and tone
