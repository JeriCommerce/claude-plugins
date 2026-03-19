---
description: Create a new support guide for the JeriCommerce docs site
argument-hint: [topic or source file]
---

Parse `$ARGUMENTS`:
- If provided, use as the topic or source file for the guide
- If empty, ask the user what the guide should cover

Follow these steps:

1. Read the guide skill at `${CLAUDE_PLUGIN_ROOT}/skills/guide/SKILL.md`.

2. Read the styleguide reference at `${CLAUDE_PLUGIN_ROOT}/skills/guide/references/styleguide.md`.

3. Read the article structure reference at `${CLAUDE_PLUGIN_ROOT}/skills/guide/references/article-structure.md`.

4. If `$ARGUMENTS` contains a source file path (starts with `@` or looks like a file path), read that file as source material. If it contains a topic, use it as the starting point.

5. Before writing content, clarify with the user:
   - Target audience (merchants, developers, both?)
   - Which category subdirectory the guide belongs in
   - Tags to assign in the frontmatter
   - Whether there's source material (README, docs, etc.) to base the content on

6. Draft the markdown content following the skill's component guide, voice and tone rules, and frontmatter requirements. Show the draft to the user for approval.

7. After approval, write the markdown file to `content/{category}/slug.md` with proper YAML frontmatter.

8. Save any images to `assets/images/` and run `python scripts/optimize-images.py` to convert them to WebP.

9. Run `python build.py` to verify the build succeeds.

10. Show the user the file path and a preview link. Remind the user to add the page path to `docs.json` navigation if it should appear in the sidebar.
