---
name: skill-optimizer
description: >
  Use when the user asks to "optimize a skill", "restructure a skill", "audit a skill",
  "split a skill into files", "modularize a skill", "improve skill consistency",
  "my skill output is inconsistent", or wants to refactor a Claude Code skill file
  into a modular folder structure. Also trigger when the user mentions that a skill
  "drifts", "is inconsistent", or "works half the time".
version: 202603.21.0
---

# Skill Optimizer

Audit and restructure Claude Code skill files into modular folder structures where SKILL.md is a pure orchestrator and each distinct section lives in its own reference file.

## Workflow

### Step 1: Identify the target skill

Ask the user which skill to optimize. Accept:
- A path to a SKILL.md file
- A skill name (then search for it)
- "my worst skill" or similar — ask which one is most inconsistent

Read the target SKILL.md file completely.

### Step 2: Audit the skill

Read `${CLAUDE_PLUGIN_ROOT}/skills/skill-optimizer/references/audit-checklist.md` for the audit methodology.

Analyze the skill file and identify every distinct section. Present the full audit to the user in the format specified by the checklist. **Do not make any changes yet** — show the audit and wait for confirmation.

### Step 3: Restructure the skill

Read `${CLAUDE_PLUGIN_ROOT}/skills/skill-optimizer/references/restructure-rules.md` for the restructuring rules and conventions.

Read `${CLAUDE_PLUGIN_ROOT}/skills/skill-optimizer/references/examples.md` for before/after examples and common patterns.

Based on the audit and the user's confirmation:

1. Create each reference file under the skill's `references/` directory
2. Rewrite SKILL.md as a pure orchestrator following the rules
3. Preserve ALL original content — nothing should be lost, summarized, or rewritten

### Step 4: Verify the restructure

After creating all files, verify:

1. No content was lost from the original SKILL.md
2. No content is duplicated across files
3. SKILL.md contains only workflow steps and file references
4. All file paths are correct
5. Each reference file is self-contained

Present a summary of what was created and ask the user to test the restructured skill on a real task.

### Step 5: Iterate (if needed)

If the user reports that something drifted or broke after restructuring:

1. Ask which specific aspect is off (voice, formatting, data, etc.)
2. Identify which reference file controls that aspect
3. Adjust only that file — the advantage of the folder structure is surgical fixes

## Reference Files

- `references/audit-checklist.md` — How to identify and categorize sections in a skill file
- `references/restructure-rules.md` — Rules for creating the orchestrator and splitting content into files
- `references/examples.md` — Before/after examples and common restructuring patterns
