---
description: Create social media posts for JeriCommerce
argument-hint: <platform> [lang]
---

Parse `$ARGUMENTS` to extract:
- **Platform** (`$1`): linkedin (default), twitter, instagram
- **Language** (`$2`, optional): en (default), es, or it

If the user provides a topic, screenshot, or context instead of a platform, infer the platform (default to LinkedIn) and use the provided content as the post subject.

## Steps

1. Read the skill at `${CLAUDE_PLUGIN_ROOT}/skills/social-content/SKILL.md`
2. Read `${CLAUDE_PLUGIN_ROOT}/references/brand-guide.md`
3. Read `${CLAUDE_PLUGIN_ROOT}/references/product-context.md`
4. Follow the social-content skill workflow to produce the post
5. Write in the requested language ($2, default: en)
6. Run the quality check from the skill before presenting
