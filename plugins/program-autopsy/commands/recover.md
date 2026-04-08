---
description: Draft a recovery message for a churned merchant based on autopsy findings
argument-hint: [slug] (optional if autopsy already ran)
---

Parse `$ARGUMENTS` to extract:
- **Slug** (`$1`, optional): Program slug. If omitted, use the autopsy data already in the conversation.

Follow these steps:

1. If no autopsy data exists in the conversation, read `${CLAUDE_PLUGIN_ROOT}/skills/program-autopsy/SKILL.md` and run the investigation first using the provided slug
2. Read `${CLAUDE_PLUGIN_ROOT}/skills/churn-recovery/SKILL.md`
3. Draft the recovery message based on autopsy findings
4. Present the email with subject line options and sending notes
