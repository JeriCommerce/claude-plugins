---
name: Jeri - Webflow Guide Creator
description: >
  Use when the user asks to "create a guide", "write a support article",
  "add a guide to Webflow", "create a CMS article", "publish a guide", "update a guide",
  or needs to create or edit content in the JeriCommerce Webflow CMS Guides collection.
  Covers Webflow CMS rich text formatting, styled tables, code blocks, and article structure.
version: 202602.26.0
---

# Webflow Guide Creator

Create and manage support guide articles in the JeriCommerce Webflow CMS with proper formatting.

## Required MCP Tools — Webflow

All CMS operations MUST go through the Webflow MCP server tools:

| Tool | Purpose |
|------|---------|
| `mcp__claude_ai_Webflow__data_cms_tool` | Create, update, publish, and delete CMS collection items |
| `mcp__claude_ai_Webflow__data_sites_tool` | Get site information |

If the Webflow MCP server is not available, inform the user that it must be configured for this plugin to work.

## Site & Collection IDs

- **Site ID**: `67f4d21f342a2f326e7e7b1b`
- **Guides Collection ID**: `67f4ff6253b5b42a926d101f`
- **CMS Locale ID**: `67f4d21f342a2f326e7e7b7c`

## Collection Fields

Each guide item has these fields:

| Field | Type | Notes |
|-------|------|-------|
| `name` | PlainText | Article title |
| `slug` | Slug | URL-friendly slug (auto-generated from name if omitted) |
| `subtitle` | PlainText | Short description shown in listings |
| `content` | RichText | Full HTML article body — all formatting rules apply here |
| `category` | Reference | Single category reference ID |
| `tags` | MultiReference | Array of tag IDs |
| `integrationss` | Reference | Single integration reference ID (note the double 's') |

### Available Categories

| Category | ID |
|----------|----|
| Loyalty | `6835bd974a4049d54f6b5f45` |

### Available Tags

| Tag | ID |
|-----|-----|
| Assets | `67f4f9a4a6e53fa3eb1141ce` |
| Set up | `68922a70db7bc641f070896b` |
| Integrations | `689233a005e5a5ecd0844b86` |
| Flows | `689233d3d9a7969b907bbc22` |

### Available Integrations

| Integration | ID |
|-------------|-----|
| Shopify | `67f4f914fd326878c16bcf33` |
| General | `67f4f914a885a4b9faea85f7` |

## Workflow

### Step 1: Gather information

Ask the user for:
1. **Topic** — What should the guide cover?
2. **Target audience** — Merchants, developers, both?
3. **Category and tags** — Which category and tags to assign (show available options)
4. **Source material** — Is there a README, docs, or other source to base the content on?

### Step 2: Draft the HTML content

Follow all formatting rules below strictly. Read reference files for full CSS:
- `${CLAUDE_PLUGIN_ROOT}/skills/webflow-guide/references/table-css.md` for table styling
- `${CLAUDE_PLUGIN_ROOT}/skills/webflow-guide/references/code-block-css.md` for code block styling
- `${CLAUDE_PLUGIN_ROOT}/skills/webflow-guide/references/article-structure.md` for content patterns

### Step 3: Create the CMS item as a draft

Use `mcp__claude_ai_Webflow__data_cms_tool` with `create_collection_items`:

```json
{
  "isDraft": true,
  "isArchived": false,
  "cmsLocaleIds": ["67f4d21f342a2f326e7e7b7c"],
  "fieldData": { ... }
}
```

### Step 4: Confirm and publish

Show the user a summary of what was created. After approval, publish with `publish_collection_items`:

```json
{
  "itemIds": ["<item-id>"]
}
```

Remind the user that they need to **publish the Webflow site** for changes to appear on production.

## Content Formatting Rules

All guide content goes in the `content` field as HTML. Follow these rules strictly.

### 1. Section Structure

Use `<h3>` for main sections and `<h4>` for subsections. Never use `<h1>` or `<h2>` (reserved by the page layout).

### 2. Section Separators

Add a visual separator between every major `<h3>` section:

```html
<p>‍</p><hr><p>‍</p>
```

The `‍` is a zero-width joiner (U+200D) — it creates vertical spacing. Place this before every `<h3>` except the very first section.

### 3. Tables — Styled with `jeri-table`

All tables MUST be wrapped in the embed pattern with inline CSS. Read full CSS from `references/table-css.md`.

```html
<div data-rt-embed-type='true'>
  <style>/* full jeri-table CSS */</style>
  <div class="jeri-table-container">
    <table class="jeri-table">
      <thead>...</thead>
      <tbody>...</tbody>
    </table>
  </div>
</div>
```

Key points:
- `data-rt-embed-type='true'` wrapper is **required**
- Full `<style>` block in **every** table embed (styles are scoped)
- Use `<span class="path-badge">` for property/field names
- Use `class="details-text"` on description `<td>` elements
- Use `class="section-header"` on `<tr>` elements for group headers

### 4. Code Blocks — Styled with `jeri-code-block`

All multi-line code blocks MUST be wrapped in the embed pattern. Read full CSS from `references/code-block-css.md`.

```html
<div data-rt-embed-type='true'>
  <style>/* full jeri-code-block CSS */</style>
  <pre class="jeri-code-block">escaped code here</pre>
</div>
```

Key points:
- `data-rt-embed-type='true'` wrapper is **required**
- Use `<pre class="jeri-code-block">` — NOT `<pre><code>`
- Dark theme (`#1e1e1e` bg, `#d4d4d4` text)
- Escape HTML: `&lt;` for `<`, `&gt;` for `>`, `&amp;` for `&`

**Inline code** uses plain `<code>` tags — no embed wrapper needed.

### 5. Text Formatting

- `<p>` for paragraphs
- `<strong>` for bold
- `<ul>` / `<ol>` for lists
- `<a href="...">` for links
- `<p>‍</p>` for extra vertical spacing

### 6. Language

All guides must be written in **English**. Technical, developer-focused tone — concise, precise, actionable.

## Updating Existing Guides

Use `update_collection_items` with the item `id` and full `fieldData` including complete `content`. Partial content updates are not supported.

## Reference Files

- `references/table-css.md` — Complete jeri-table CSS, templates, and class reference
- `references/code-block-css.md` — Complete jeri-code-block CSS, templates, and examples
- `references/article-structure.md` — Standard article template, heading hierarchy, writing style
