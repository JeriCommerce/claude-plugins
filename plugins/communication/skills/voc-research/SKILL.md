---
name: jericommerce-voc-research
description: >
  Build and maintain a voice-of-customer quote bank for JeriCommerce by mining reviews,
  forums, and social media for real merchant language. Use when the user wants to research
  customer language, mine reviews, gather quotes by pain point, or update the VoC quote bank.
  Also trigger for "VoC," "voice of customer," "quote bank," "customer quotes," "review mining,"
  "what customers say," or "customer language research."
version: 202603.30.0
---

# JeriCommerce VoC Research

You help build and maintain a structured quote bank of real merchant language for JeriCommerce. This raw material feeds into better copy, emails, and social content across all communication skills.

## Before Starting

1. Read `${CLAUDE_PLUGIN_ROOT}/references/product-context.md` for product context and known pain points
2. Read `${CLAUDE_PLUGIN_ROOT}/references/voc-quote-bank.md` to see the current quote bank (120+ quotes)

The research scope is pre-configured:
- **Product**: Loyalty programs for Shopify merchants (JeriCommerce and competitors)
- **Audience**: Shopify merchants, e-commerce managers, DTC brand owners, retail operators
- **Core topics**: Customer retention, loyalty apps, repeat purchases, POS sync, wallet passes

Only ask for:
- **Specific source or topic** to research (e.g., "mine Reddit for POS loyalty complaints" or "check latest Smile.io reviews")
- Whether to **update the existing quote bank** or create a separate research document

## Output Language

VoC research output is always in English (it's internal reference material). Quotes should be captured verbatim in their original language.

## High-Signal Sources for Loyalty/Shopify

| Source | What to look for |
|--------|-----------------|
| Shopify App Store reviews | Reviews of Smile.io, LoyaltyLion, Yotpo, Growave, BON, Rivo |
| Reddit r/shopify, r/ecommerce, r/DTC | Threads about loyalty programs, retention, POS |
| G2/Capterra reviews | Loyalty platform reviews |
| Shopify Community forums | Questions about loyalty apps, POS integration |
| Twitter/X | Complaints, questions about loyalty apps |
| YouTube comments | Reviews/tutorials of loyalty apps |

## Known Pain Themes (validate and expand)

From existing research:
1. Pricing that punishes growth
2. Support that disappears
3. Buggy apps / leaked discount codes
4. POS as afterthought
5. Nobody uses the program
6. Integration nightmares (Klaviyo)
7. Feature gating / forced upgrades
8. Fraud / security vulnerabilities

Look for:
- New pain themes not yet captured
- Fresh quotes that are more vivid than existing ones
- Quotes in Spanish or Italian markets (capture in original language)

## What to Capture

For each quote:
- **Exact quote** (verbatim, don't paraphrase)
- **Source** (platform + context)
- **Type**: Pain, Desire, Praise, or Objection
- **Language** of the original quote

**Look for:**
- "I wish...", "I hate when...", "The problem is...", "What finally worked was..."
- Specific situations, not generalities
- Emotional language: frustration, relief, excitement
- Upvoted comments (shared sentiment)

## Output Format

Update the existing quote bank at `${CLAUDE_PLUGIN_ROOT}/references/voc-quote-bank.md` following its current structure:

```markdown
### [Theme Label]

**Summary:** [1 sentence]
**Quote count:** [number]
**Frequency markers:** [recurring words]

#### Pain
> "[exact quote]"
> Source: [platform, context]

#### Desire
> "[exact quote]"
> Source: [platform, context]
```

After updating, also surface:
1. **New frequency markers** found
2. **Emotional peaks** (best quotes for headlines)
3. **Language gaps** (how customers say it vs. how we say it)
4. **Copy opportunities** (specific suggestions for using findings)

## Integration

After updating the quote bank, the other communication skills automatically benefit:
- **copywriting**: References VoC for page copy
- **cold-email**: Uses pain points for email hooks
- **social-content**: Mirrors customer language in posts
