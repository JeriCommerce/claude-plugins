---
name: niche-research
description: "Generate a pSEO niche research report from Ahrefs data, ready to copy into the JeriCommerce blog repo. Use this skill when the user says 'new niche', 'niche research', 'add a vertical', 'Ahrefs research for [topic]', 'generate niche report', provides keyword research data or CSV exports from Ahrefs, or wants to plan content for a new industry/topic targeting the blog.jericommerce.com pSEO system. Also trigger when the user pastes keyword tables, competitor analysis, or content gap data and wants it turned into a structured niche plan."
---

# Niche Research Report Generator

Generate structured niche taxonomy files from Ahrefs research data, formatted exactly for the JeriCommerce blog pSEO system (`blog.jericommerce.com`).

## What This Skill Produces

Two ready-to-paste files for the blog repo:

1. **`{niche-slug}.json`** → goes in `.pseo/taxonomy/` — full niche context (audience, pain points, industries, SEO landscape, content strategy)
2. **`_registry-update.json`** → content matrix and rollout plan to merge into `.pseo/taxonomy/_registry.json`

## Input: What You Need From the User

The user provides Ahrefs data in any format (tables, CSV, screenshots, pasted text). Extract these data points:

### Required
- **Keyword list** with volume, KD, CPC (at minimum the top 20-30 keywords)
- **Target niche/topic** (what the blog will cover)

### Helpful but Optional
- Competitor domains with DR, traffic, keyword counts
- Content gap analysis
- Top-performing pages from competitors
- SERP overview for key terms

If the user provides a CSV file, read it directly. If they paste a table, parse it. If data is incomplete, use Ahrefs MCP tools to fill gaps (keywords-explorer-overview, site-explorer-organic-competitors, site-explorer-organic-keywords).

## Output Format

### File 1: `{niche-slug}.json`

Follow this exact structure (matches the existing `shopify-loyalty.json` format):

```json
{
  "slug": "{niche-slug}",
  "name": "{Niche Display Name}",
  "status": "validated",
  "context": {
    "audience": {
      "primary": "...",
      "secondary": "...",
      "demographics": "...",
      "psychographics": "..."
    },
    "pain_points": [
      "10 specific pain points backed by the keyword data"
    ],
    "monetization": {
      "primary": ["How JeriCommerce monetizes in this niche"],
      "secondary": ["..."],
      "business_model": "..."
    },
    "content_that_works": {
      "formats": ["7-10 content formats derived from competitor analysis"],
      "high_performing_angles": ["5-7 angles based on top-ranking content"],
      "content_gaps": "What nobody is covering (derived from Ahrefs data)"
    },
    "subtopics": [
      {
        "slug": "subtopic-slug",
        "name": "Subtopic Display Name",
        "specifics": "What makes this subtopic unique",
        "loyalty_angle": "How JeriCommerce wallet passes apply here",
        "shopify_relevance": "Why this matters for Shopify merchants"
      }
    ],
    "seo_landscape": {
      "competition_level": "low|medium|high (based on avg KD)",
      "long_tail_opportunity": "assessment based on keyword data",
      "seasonal_patterns": "any patterns visible in the data",
      "serp_features": "what dominates SERPs for key terms",
      "current_jericommerce_position": {
        "organic_keywords": 0,
        "organic_traffic": 0,
        "top_performing_content": "none yet",
        "traffic_value": "$0",
        "key_gap": "describe the opportunity"
      },
      "competitor_intel": {
        "competitor_slug": {
          "traffic": 0,
          "dr": 0,
          "strategy": "their content approach",
          "top_page": "their best performing page"
        }
      }
    },
    "unique_considerations": [
      "5-7 things unique to this niche that content must account for"
    ]
  },
  "content_categories": {
    "enabled": ["resources", "guides", "alternatives", "comparisons", "tools"],
    "disabled_reason": {}
  },
  "generation_notes": {
    "tone": "...",
    "avoid": ["4-5 things to avoid"],
    "emphasize": ["5-7 things to emphasize"],
    "cta_strategy": "..."
  }
}
```

### File 2: `_registry-update.json`

Content matrix with page counts, title templates, slug patterns, and rollout batches:

```json
{
  "niche_entry": {
    "slug": "{niche-slug}",
    "name": "{Niche Display Name}",
    "status": "validated",
    "content_types_enabled": 5,
    "estimated_pages": 0,
    "last_updated": "YYYY-MM-DD",
    "notes": "One-line summary from keyword data"
  },
  "content_matrix": {
    "resources": {
      "subtypes": [
        {
          "template": "Title template with {industry} and {year} placeholders",
          "slug_pattern": "slug-pattern-{industry}",
          "industries": 0,
          "pages": 0
        }
      ],
      "total_pages": 0
    },
    "guides": { "subtypes": [], "total_pages": 0 },
    "alternatives": { "subtypes": [], "total_pages": 0 },
    "comparisons": { "subtypes": [], "total_pages": 0 },
    "tools": { "subtypes": [], "total_pages": 0 }
  },
  "rollout_batches": [
    { "batch": 1, "pages": 0, "focus": "...", "timeline": "Week 1-2" }
  ]
}
```

## Process

### Step 1: Parse Input Data

Read whatever the user provided. Extract into structured tables:
- Keywords: term, volume, KD, CPC, intent
- Competitors: domain, DR, traffic, keywords, overlap %
- Content gaps: topics with volume but no good coverage

### Step 2: Analyze & Identify Structure

From the keyword data, identify:
- **Subtopics/verticals** — natural clusters in the keywords (these become the `subtopics` array)
- **Content type fit** — which of the 5 content types (resource, guide, alternatives, comparison, tool) make sense
- **Title templates** — deterministic patterns that cover keyword clusters
- **Page count** — how many pages per content type × subtopic

The golden rule: every page must target a real keyword cluster with proven volume. No speculative pages.

### Step 3: Generate Both Files

Write the two JSON files. Present them to the user as code blocks they can copy into the repo.

### Step 4: Summary

End with a brief summary:
- Total pages planned
- Estimated monthly search volume addressable
- Top 3 highest-opportunity keywords
- Recommended Batch 1 focus

## Quality Checks

Before outputting, verify:
- Every `subtopic` maps to real keywords from the input data
- Page count estimates are realistic (not inflated)
- Title templates produce titles that match actual search queries
- Pain points come from keyword intent, not generic assumptions
- Competitor intel uses actual Ahrefs data, not guesses
- The `loyalty_angle` and `shopify_relevance` fields are specific to JeriCommerce's product (wallet passes, Apple/Google Wallet, NFC, Shopify POS)

## Using Ahrefs Tools

If the user's data is incomplete, you have access to Ahrefs MCP tools. Useful ones:

- `keywords-explorer-overview` — Get volume, KD, CPC for specific keywords
- `keywords-explorer-matching-terms` — Find related keywords
- `site-explorer-organic-competitors` — Find who competes in this space
- `site-explorer-organic-keywords` — See what competitors rank for
- `site-explorer-metrics` — Get DR, traffic for a domain
- `serp-overview` — See who ranks for a specific keyword

Always use `mode=subdomains` when analyzing domains. Check the `doc` tool first if you're unsure about parameters.
