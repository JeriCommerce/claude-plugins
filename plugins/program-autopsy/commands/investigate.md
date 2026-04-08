---
description: Investigate a program's lifecycle, health, or churn
argument-hint: <slug|domain|uuid>
---

Parse `$ARGUMENTS` to extract:
- **Identifier** (`$1`): A program slug, shop domain, or program UUID

Follow these steps:

1. Read the skill at `${CLAUDE_PLUGIN_ROOT}/skills/program-autopsy/SKILL.md`
2. Use the provided identifier to execute the full investigation protocol
3. Produce the visual autopsy report widget
