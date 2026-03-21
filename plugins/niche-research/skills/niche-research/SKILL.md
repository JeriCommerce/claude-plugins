---
name: niche-research
description: "Generate a pSEO niche research report from Ahrefs data, ready to copy into the JeriCommerce blog repo. Use this skill when the user says 'new niche', 'niche research', 'add a vertical', 'Ahrefs research for [topic]', 'generate niche report', provides keyword research data or CSV exports from Ahrefs, or wants to plan content for a new industry/topic targeting the blog.jericommerce.com pSEO system. Also trigger when the user pastes keyword tables, competitor analysis, or content gap data and wants it turned into a structured niche plan."
---

# Niche Research Report Generator

Generate structured niche taxonomy files from Ahrefs research data, formatted exactly for the JeriCommerce blog pSEO system (`blog.jericommerce.com`).

## Workflow

### Step 1: Gather input data

Read `${CLAUDE_PLUGIN_ROOT}/skills/niche-research/references/input-requirements.md` for what data is needed.

Collect the required data from the user. If data is incomplete, use Ahrefs MCP tools to fill gaps.

### Step 2: Check Ahrefs tools (if needed)

Read `${CLAUDE_PLUGIN_ROOT}/skills/niche-research/references/ahrefs-tools.md` for available Ahrefs MCP tools and usage patterns.

Use these tools to supplement missing data points.

### Step 3: Parse and analyze

Read `${CLAUDE_PLUGIN_ROOT}/skills/niche-research/references/process.md` for the 4-step analysis process.

Follow the process to parse input data, identify structure (subtopics, content types, title templates, page counts, authoritative sources).

### Step 4: Generate output files

Read `${CLAUDE_PLUGIN_ROOT}/skills/niche-research/references/output-format.md` for the exact JSON structures.

Generate both files (`{niche-slug}.json` and `_registry-update.json`) following the schemas exactly.

### Step 5: Quality checks

Read `${CLAUDE_PLUGIN_ROOT}/skills/niche-research/references/quality-checks.md` for validation rules.

Verify all checks pass before presenting output to the user.

### Step 6: Present results

Present both JSON files as code blocks the user can copy into the repo. End with the summary (total pages, search volume, top keywords, Batch 1 focus).

## Reference Files

- `references/input-requirements.md` — Required and optional input data from the user
- `references/output-format.md` — JSON structures for both output files
- `references/process.md` — 4-step analysis process (parse, analyze, generate, summarize)
- `references/quality-checks.md` — Validation rules before delivering output
- `references/ahrefs-tools.md` — Available Ahrefs MCP tools and usage patterns
