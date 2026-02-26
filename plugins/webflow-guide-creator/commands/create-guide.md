---
description: Create a new support guide in Webflow CMS
argument-hint: [topic or source file]
---

Parse `$ARGUMENTS`:
- If provided, use as the topic or source file for the guide
- If empty, ask the user what the guide should cover

Follow these steps:

1. Read the webflow-guide skill at `${CLAUDE_PLUGIN_ROOT}/skills/webflow-guide/SKILL.md`.

2. Read the reference files:
   - `${CLAUDE_PLUGIN_ROOT}/skills/webflow-guide/references/table-css.md` for table styling
   - `${CLAUDE_PLUGIN_ROOT}/skills/webflow-guide/references/code-block-css.md` for code block styling
   - `${CLAUDE_PLUGIN_ROOT}/skills/webflow-guide/references/article-structure.md` for content patterns

3. If `$ARGUMENTS` contains a source file path (starts with `@` or looks like a file path), read that file as source material. If it contains a topic, use it as the starting point.

4. Before writing content, clarify with the user:
   - Target audience (merchants, developers, both?)
   - Which category and tags to assign
   - Whether there's source material (README, docs, etc.) to base the content on

5. Follow the skill workflow: draft HTML content following all formatting rules, create the CMS item as a draft, then publish after user approval.

6. After publishing, remind the user that a Webflow site publish is needed for changes to go live.
