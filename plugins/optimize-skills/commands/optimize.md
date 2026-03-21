---
description: Audit and restructure a Claude Code skill into a modular folder structure
argument-hint: [path-to-SKILL.md]
---

Parse `$ARGUMENTS` to extract:
- **Skill path** (`$1`): Path to the SKILL.md file to optimize. If not provided, ask the user.

Follow these steps:

1. Read the skill at `${CLAUDE_PLUGIN_ROOT}/skills/skill-optimizer/SKILL.md`
2. Read the target SKILL.md file at the provided path
3. Execute the workflow from the skill — audit first, then restructure after confirmation
