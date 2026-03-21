# Optimize Skills

Audit and restructure Claude Code skill files into modular folder structures for better consistency and maintainability.

## The Problem

Large, monolithic SKILL.md files tend to produce inconsistent output. When voice rules, formatting instructions, data schemas, and workflow steps are all in one file, Claude can drift — nailing the intro but losing the tone by the middle, or getting the format right but missing quality checks.

## The Solution

This plugin restructures skills into a folder where:

- **SKILL.md** is a pure orchestrator — contains no rules, just a step-by-step workflow pointing to reference files
- **Each distinct section** becomes its own file under `references/`
- **Examples stay separate** from instructions
- **Debugging is surgical** — when something drifts, you know exactly which file to fix

## Usage

### Automatic (skill triggers when relevant)

Just describe what you want:

- "This skill is inconsistent, help me fix it"
- "Audit my newsletter skill"
- "Restructure my research skill into a folder"

### Via command

```
/optimize-skills:optimize path/to/skills/my-skill/SKILL.md
```

## What It Does

1. **Audits** the skill — identifies every distinct section (rules, templates, examples, tools, voice, data)
2. **Shows the audit** — you review before any changes happen
3. **Restructures** — creates reference files and rewrites SKILL.md as an orchestrator
4. **Verifies** — checks nothing was lost or duplicated
5. **Iterates** — if output drifts after restructuring, helps you fix the specific file

## Prerequisites

- A Claude Code project with at least one skill file to optimize
- No external tools or MCP servers required

## Example

Before:
```
skills/my-skill/
└── SKILL.md              (200+ lines, everything in one file)
```

After:
```
skills/my-skill/
├── SKILL.md              (~40 lines, orchestrator only)
└── references/
    ├── tools.md
    ├── workflow.md
    ├── templates.md
    ├── classification.md
    └── voice-tone.md
```
