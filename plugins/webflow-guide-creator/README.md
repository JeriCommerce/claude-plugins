# Webflow Guide Creator Plugin

Create and manage support guide articles in the JeriCommerce Webflow CMS with consistent formatting.

## What it does

Encodes all the formatting knowledge needed to create properly styled support articles in JeriCommerce's Webflow CMS — including styled tables with the `jeri-table` pattern, code blocks with dark theme, section separators, and the correct CMS API structure. Ensures every guide follows the same visual standard.

## Prerequisites

- **Webflow MCP server** must be configured. This plugin uses Webflow MCP tools (`mcp__claude_ai_Webflow__data_cms_tool`) to create and publish CMS items. If the Webflow MCP server is not available, the plugin will not work.

## Usage

### Cowork mode (plugin installed)

```
/create-guide
/create-guide loyalty page templates
/create-guide @path/to/README.md
```

### Chat mode

Describe the guide you want to create to Claude with the Webflow MCP tools available. It will load the formatting rules and guide you through the process.

## Components

| Component | Name | Purpose |
|-----------|------|---------|
| Command | `/create-guide` | Entry point — starts the guide creation workflow |
| Skill | `webflow-guide` | CMS structure, HTML formatting rules, table/code block CSS, article conventions |

## Formatting Reference

The plugin enforces these patterns learned from JeriCommerce's existing guides:

- **Tables**: Wrapped in `<div data-rt-embed-type='true'>` with inline CSS using the `jeri-table` class system
- **Code blocks**: Multi-line code uses `<pre>` with `jeri-code-block` dark theme; inline code uses `<code>`
- **Separators**: `<p>‍</p><hr><p>‍</p>` between major sections
- **Headings**: `<h3>` for sections, `<h4>` for subsections (never `<h1>`/`<h2>`)
- **Language**: English, developer-focused tone
