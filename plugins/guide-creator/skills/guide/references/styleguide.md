---
title: Style Guide
description: Internal reference for all available components, colors, and variables.
noindex: true
---

This page is an internal reference showing every component and design token available in the JeriCommerce docs site. It is not indexed by search engines and not linked from the navigation.

## Colors & Variables

### Theme Colors

| Variable | Light | Dark | Usage |
|---|---|---|---|
| `--color-accent` | `#7000FF` | `#7000FF` | Backgrounds with white text (CTA buttons, badges) |
| `--color-accent-fg` | `#7000FF` | `#9B4DFF` | Foreground: links, active states, borders, icons |
| `--color-primary` | `#1C1C1C` | `#1C1C1C` | Brand primary |
| `--color-primary-light` | `#4B5563` | `#4B5563` | Brand light variant |
| `--color-primary-dark` | `#000000` | `#000000` | Brand dark variant |

### Background Colors

| Variable | Light | Dark |
|---|---|---|
| `--color-bg` | `#FFFFFF` | `#111111` |
| `--color-bg-secondary` | `#F9FAFB` | `#191919` |
| `--color-bg-tertiary` | `#F3F4F6` | `#222222` |

### Text Colors

| Variable | Light | Dark |
|---|---|---|
| `--color-text` | `#111827` | `#F9FAFB` |
| `--color-text-secondary` | `#6B7280` | `#9CA3AF` |
| `--color-text-tertiary` | `#9CA3AF` | `#6B7280` |

### Border Colors

| Variable | Light | Dark |
|---|---|---|
| `--color-border` | `#E5E7EB` | `#2D2D2D` |
| `--color-border-light` | `#F3F4F6` | `#222222` |

### Layout Variables

| Variable | Value |
|---|---|
| `--sidebar-width` | `260px` |
| `--toc-width` | `220px` |
| `--topbar-height` | `60px` |
| `--content-max-width` | `768px` |
| `--radius` | `8px` |
| `--radius-sm` | `6px` |

### Typography

| Variable | Value |
|---|---|
| `--font-heading` | Inter |
| `--font-body` | Inter |
| `--font-code` | JetBrains Mono |

---

## Typography

### Headings

The article body supports headings from `##` (h2) through `####` (h4). The `#` (h1) is reserved for the page title from frontmatter.

### Inline Styles

Regular text, **bold text**, *italic text*, ~~strikethrough~~, and `inline code`.

Links look like [this is a link](#) and external links [open in a new tab](https://example.com).

### Blockquote

> This is a blockquote. It has a left border using `--color-accent-fg`.

### Lists

Unordered:
- First item
- Second item
  - Nested item
  - Another nested item
- Third item

Ordered:
1. Step one
2. Step two
3. Step three

### Horizontal Rule

---

## Callouts

Five callout types are available. Use the tag name directly in markdown.

<Note>
This is a **Note** callout — used for general information and tips that supplement the main content. Color: `#3B82F6`.
</Note>

<Warning>
This is a **Warning** callout — used for important caveats or things that could go wrong. Color: `#F59E0B`.
</Warning>

<Info>
This is an **Info** callout — used for additional context or background information. Color: `#0EA5E9`.
</Info>

<Tip>
This is a **Tip** callout — used for best practices and recommendations. Color: `#22C55E`.
</Tip>

<Check>
This is a **Check** callout — used for success states or confirmed information. Color: `#10B981`.
</Check>

---

## Cards

### Two Columns (default)

<CardGroup cols="2">
<Card title="Loyalty Cards" icon="wallet" href="/wallet/create-the-assets-for-an-awesome-wallet">
Create and customize wallet passes for your customers.
</Card>
<Card title="Push Notifications" icon="bell" href="/notifications/notification-delivery-strategies">
Send targeted notifications to engage your audience.
</Card>
</CardGroup>

### Three Columns

<CardGroup cols="3">
<Card title="Shopify" icon="store">
Shopify POS and flow integrations.
</Card>
<Card title="Klaviyo" icon="plug">
Email and SMS automation.
</Card>
<Card title="API" icon="info">
Direct API access and webhooks.
</Card>
</CardGroup>

### Cards Without Links

<CardGroup cols="2">
<Card title="Static Card" icon="info">
Cards without an `href` attribute render as non-clickable containers.
</Card>
<Card title="Another Static Card" icon="lightbulb">
Useful for displaying feature lists or summaries.
</Card>
</CardGroup>

---

## Tabs

<Tabs>
<Tab title="JavaScript">

```javascript
const response = await fetch('/api/loyalty/points', {
  method: 'POST',
  headers: { 'Authorization': 'Bearer YOUR_API_KEY' },
  body: JSON.stringify({ customer_id: '123', points: 50 })
});
```

</Tab>
<Tab title="Python">

```python
import requests

response = requests.post(
    'https://api.jericommerce.com/loyalty/points',
    headers={'Authorization': 'Bearer YOUR_API_KEY'},
    json={'customer_id': '123', 'points': 50}
)
```

</Tab>
<Tab title="cURL">

```bash
curl -X POST https://api.jericommerce.com/loyalty/points \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{"customer_id": "123", "points": 50}'
```

</Tab>
</Tabs>

---

## Code Blocks

### Inline Code

Use `inline code` for short references like variable names or commands.

### Fenced Code Block

```json
{
  "program": "loyalty",
  "points_ratio": 0.15,
  "rounding": "up",
  "holding_period_days": 30
}
```

### Code with Language Label

```javascript
// Webhook handler for purchase events
app.post('/webhooks/purchase', (req, res) => {
  const { customer_id, amount } = req.body;
  const points = Math.ceil(amount * 0.15);
  creditPoints(customer_id, points);
  res.status(200).send('OK');
});
```

### Code Group

<CodeGroup>

```javascript title="Node.js"
const client = new JeriCommerce({ apiKey: 'sk_...' });
const customer = await client.customers.get('cust_123');
```

```python title="Python"
client = JeriCommerce(api_key='sk_...')
customer = client.customers.get('cust_123')
```

</CodeGroup>

---

## Accordion

<AccordionGroup>

<Accordion title="What is JeriCommerce?">
JeriCommerce is a loyalty platform that integrates with Shopify to provide wallet passes, points programs, and customer engagement tools.
</Accordion>

<Accordion title="How do points work?">
Points are credited as a percentage of each purchase. The merchant defines the percentage and rounding strategy. Points can have a holding period before becoming redeemable.
</Accordion>

<Accordion title="Can I customize the wallet pass?">
Yes — you can customize colors, logos, images, back-of-card sections, and choose between QR codes, barcodes, or NFC for identification.
</Accordion>

</AccordionGroup>

---

## Steps

<Steps>

### Create your program

Go to the JeriCommerce dashboard and create a new loyalty program. Define your points ratio and earning rules.

### Design your wallet pass

Upload your brand assets and customize the pass layout. Choose your identification method (QR, barcode, or NFC).

### Connect Shopify

Install the JeriCommerce app from the Shopify App Store and connect your store. Enable the purchase earning flow.

### Launch

Activate your program and start engaging customers with wallet-based loyalty.

</Steps>

---

## Tables

| Plan | Customers | Push Notifications | Price |
|---|---|---|---|
| Free | Up to 500 | 1,000/month | $0 |
| Growth | Up to 5,000 | 10,000/month | $49/mo |
| Pro | Up to 25,000 | 50,000/month | $149/mo |
| Enterprise | Unlimited | Unlimited | Custom |

---

## Images

Images are automatically centered. Text immediately after the image tag on the same line becomes a gray caption below.

### Image with Caption

![](/assets/images/flow-card.webp)Earning flow card showing the credit points configuration

### Image without Caption

![](/assets/images/referral.webp)

---

## Frontmatter Reference

Every `.md` file supports these frontmatter fields:

| Field | Required | Description |
|---|---|---|
| `title` | Yes | Page title, shown in header and browser tab |
| `description` | Yes | Meta description for SEO and search |
| `sidebarTitle` | No | Short title for sidebar (use when title > 40 chars) |
| `noindex` | No | Set to `true` to add `noindex, nofollow` meta tag |
| `tags` | No | Array of tags for categorization |
| `source` | No | Original source URL (informational only) |
