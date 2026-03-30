---
description: Write on-brand marketing copy for a JeriCommerce page
argument-hint: <page-type> [lang]
---

Parse `$ARGUMENTS` to extract:
- **Page type** (`$1`): homepage, landing, pricing, feature, about, app-store, case-study
- **Language** (`$2`, optional): en (default), es, or it

## Steps

1. Read the skill at `${CLAUDE_PLUGIN_ROOT}/skills/copywriting/SKILL.md`
2. Read `${CLAUDE_PLUGIN_ROOT}/references/brand-guide.md`
3. Read `${CLAUDE_PLUGIN_ROOT}/references/product-context.md`
4. Read `${CLAUDE_PLUGIN_ROOT}/references/voc-quote-bank.md`
5. Follow the copywriting skill workflow to produce page copy
6. Write in the requested language ($2, default: en)
7. Run the quality check from the skill before presenting
