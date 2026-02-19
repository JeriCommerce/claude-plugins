---
description: Report a bug or customer issue to Linear
argument-hint: ["optional brief description of the issue"]
---

Parse `$ARGUMENTS`:
- If provided, use as the initial bug description to start the conversation
- If empty, ask the user to describe the issue

Follow these steps:

1. Read the bug-reporter skill at `${CLAUDE_PLUGIN_ROOT}/skills/bug-reporter/SKILL.md`.

2. If `$ARGUMENTS` contains a description, acknowledge it and begin asking follow-up questions to complete the report. If no arguments, ask "What issue would you like to report?"

3. Follow the skill workflow: gather information conversationally (one question at a time), classify priority and labels, show summary for confirmation, then create the issue in Linear.

4. After creating the issue, share the Linear issue URL with the user.
