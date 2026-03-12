# Niche Research Plugin

Generates pSEO niche taxonomy reports from Ahrefs data, ready to copy into the `JeriCommerce/blog` repo.

## Trigger phrases

- "new niche", "niche research", "add a vertical"
- "Ahrefs research for [topic]"
- "generate niche report"
- Pasting keyword tables or Ahrefs CSV exports

## What it produces

1. **`{niche-slug}.json`** → `.pseo/taxonomy/` — full niche context (includes `authoritative_sources` with trusted domains for outbound links during content generation)
2. **`_registry-update.json`** → content matrix and rollout plan

## Requirements

- Ahrefs MCP connector (for data enrichment when input is incomplete)
- User provides keyword data (CSV, table, or paste)

## Usage

```
> new niche: pet loyalty programs
> [paste Ahrefs keyword export]
```

The skill parses the data, generates both JSON files, and presents them as copyable code blocks.
