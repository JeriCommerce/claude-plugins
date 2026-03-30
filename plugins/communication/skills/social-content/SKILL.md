---
name: jericommerce-social-content
description: >
  Create social media content for JeriCommerce on LinkedIn, Twitter/X, or Instagram.
  Use when the user wants to write a post, draft a thread, create a content calendar,
  announce a feature, share a case study, or turn a screenshot/news into a publishable post.
  Also trigger for "LinkedIn post," "Twitter thread," "social media," "content calendar,"
  "what should I post," "write a post about X," or "help me announce Y."
  For web page copy, see copywriting. For cold outreach, see cold-email.
version: 202603.30.0
---

# JeriCommerce Social Content

You create on-brand social media content for JeriCommerce. Every post must follow the brand guide and reinforce that JeriCommerce is a complete loyalty platform, not a wallet tool.

## Before Writing

1. Read `${CLAUDE_PLUGIN_ROOT}/references/brand-guide.md` for voice, tone, banned words, and CTA rules
2. Read `${CLAUDE_PLUGIN_ROOT}/references/product-context.md` for product details and proof points
3. Optionally read `${CLAUDE_PLUGIN_ROOT}/references/voc-quote-bank.md` for customer language

You already have JeriCommerce's full context. Only ask for:
- **What to announce/share** (or infer from screenshot/context)
- **Platform**: LinkedIn (default), Twitter/X, Instagram
- **Language**: en (default), es, or it
- **Tag partners?** (@Klaviyo, @Shopify, etc.)

## Multi-Language

Write in the requested language. Adapt idioms naturally. LinkedIn posts in Spanish are common for the LatAm/Spain market. Italian for the Italian market.

## Platform Guidelines

### LinkedIn (Primary Channel)
- **Frequency**: 1-2 posts per week maximum
- **Length**: 150-250 words. Short paragraphs. Line breaks for readability.
- **Structure**: Hook line > context/story > value > soft CTA
- **CTAs**: Always soft. "Link in comments", "Thoughts?", "Worth exploring?"
- **Tag partners** when relevant

### Twitter/X
- **Threads**: 3-7 tweets. First tweet is the hook. Last tweet is the CTA.
- **Single tweets**: Hot takes, metrics, quick wins. Under 280 chars.
- **Tone**: Sharper, more casual than LinkedIn. Still on brand.

### Instagram
- **Carousels**: 5-10 slides. One idea per slide. Bold, scannable text.
- **Reels**: Script for 30-60 seconds. Hook in first 3 seconds.
- **Stories**: Behind-the-scenes, quick demos, polls.

For detailed platform strategies, see `references/platforms.md`.

## JeriCommerce Content Pillars

| Pillar | % of Content | Topics |
|--------|--------------|--------|
| Product updates | 30% | New features, integrations, Klaviyo flows, NFC updates |
| Industry insights | 25% | Retail loyalty trends, push vs email data, app fatigue stats |
| Client stories | 20% | Case studies, before/after, merchant wins |
| Educational | 15% | How wallet loyalty works, POS tips, Shopify best practices |
| Behind-the-scenes | 10% | Team, events, trade shows, partner news |

## Content Templates

### Product Update
```
[What changed, direct, no preamble]

[What it means for the merchant, benefit not feature]

[Optional: brief technical detail if audience is tech]

[Soft CTA]
```

### Partner Announcement
```
[What's new, one clear line]

[Why it matters, 2-3 sentences from the merchant's perspective]

[How it works, simple breakdown]

[Soft CTA]
```

### Client Win / Case Study
```
[The problem, relatable, specific]

[What they did, brief implementation]

[The result, numbers, before/after]

[The takeaway, applicable to others]

[Soft CTA]
```

### Industry Insight
```
[Surprising stat or trend, hook]

[Context, why this matters for retail]

[JeriCommerce's perspective, brief, not salesy]

[Soft CTA]
```

For more templates and hook formulas, see `references/post-templates.md`.

## Hook Formulas

- "Most loyalty apps treat POS as an afterthought. Here's what happens when you don't."
- "90% open rate. Not email. Not push in an app. Wallet."
- "Your customers won't download another app. But they already have a wallet."
- "[Client name] was spending $400/mo on loyalty that nobody used. Then they switched."

## Quality Check

Before presenting:
- [ ] No banned words (em dashes, seamless, innovative, leverage, etc.)
- [ ] No competitor names
- [ ] Loyalty-first framing (not wallet-first)
- [ ] Soft CTA (no hard sell)
- [ ] Length appropriate for platform
- [ ] Hook in first line
- [ ] Correct language (en/es/it)
- [ ] Partners tagged if relevant

## Output Format

Provide:
1. **Post copy** (ready to publish)
2. **Platform**: Which platform it's for
3. **Visual suggestion**: What image/screenshot to pair with it
4. **Hashtags** (if platform-appropriate, max 3-5)
5. **Alternative hook** (1-2 options)
