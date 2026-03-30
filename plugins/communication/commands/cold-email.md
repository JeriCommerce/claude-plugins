---
description: Write cold outreach emails targeting Shopify merchants
argument-hint: <target-description> [lang]
---

Parse `$ARGUMENTS` to extract:
- **Target** (`$1`): Description of the prospect (role, company, context)
- **Language** (`$2`, optional): en (default), es, or it

## Steps

1. Read the skill at `${CLAUDE_PLUGIN_ROOT}/skills/cold-email/SKILL.md`
2. Read `${CLAUDE_PLUGIN_ROOT}/references/brand-guide.md`
3. Read `${CLAUDE_PLUGIN_ROOT}/references/product-context.md`
4. Read `${CLAUDE_PLUGIN_ROOT}/references/voc-quote-bank.md`
5. Follow the cold-email skill workflow to produce the email + follow-up sequence
6. Write in the requested language ($2, default: en)
7. Run the quality check from the skill before presenting
