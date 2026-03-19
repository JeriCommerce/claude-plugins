# Guide Creator Plugin

Create and manage support guide articles for the JeriCommerce static docs site.

## What it does

Encodes all the formatting knowledge needed to create properly structured markdown support articles for JeriCommerce's Python-based static site generator. Covers frontmatter fields, available components (callouts, cards, tabs, code groups, accordions, steps), image handling, and the voice and tone standards. Ensures every guide follows the same structure and quality bar.

## Prerequisites

- Access to the JeriCommerce support docs repository (Python static site generator)
- Python available for running `build.py` and `scripts/optimize-images.py`

## Usage

### Cowork mode (plugin installed)

```
/create-guide
/create-guide loyalty page templates
/create-guide @path/to/README.md
```

### Chat mode

Describe the guide you want to create. Claude will load the formatting rules, gather the necessary information, write the markdown file to the correct content directory, optimize images, and verify the build.

## Components

| Component | Name | Purpose |
|-----------|------|---------|
| Command | `/create-guide` | Entry point — starts the guide creation workflow |
| Skill | `guide` | Markdown structure, frontmatter fields, available components, voice and tone, build workflow |

## Build & Optimize Workflow

1. Write the markdown file to `content/{category}/slug.md`
2. Save images to `assets/images/` (WebP preferred)
3. Run `python scripts/optimize-images.py` to convert images to WebP
4. Run `python build.py` to verify the build succeeds
5. Add the page path to `docs.json` navigation if it should appear in the sidebar

## Available Components

- **Callouts**: `<Note>`, `<Warning>`, `<Info>`, `<Tip>`, `<Check>`
- **Cards**: `<CardGroup cols="2">` with `<Card title="" icon="" href="">`
- **Tabs**: `<Tabs>` with `<Tab title="">`
- **Code groups**: `<CodeGroup>` with fenced code blocks
- **Accordions**: `<AccordionGroup>` with `<Accordion title="">`
- **Steps**: `<Steps>` with `### Step title` content
- **Frame**: `<Frame>` for bordered content
- **Tooltip**: `<Tooltip tip="text">hover text</Tooltip>`
- **Images**: `![alt](/assets/images/file.webp)Caption text`
- **Tables**: Standard markdown tables
