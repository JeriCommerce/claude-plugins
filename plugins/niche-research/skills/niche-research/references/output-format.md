# Output Format

## What This Skill Produces

Two ready-to-paste files for the blog repo:

1. **`{niche-slug}.json`** → goes in `.pseo/taxonomy/` — full niche context (audience, pain points, industries, SEO landscape, content strategy)
2. **`_registry-update.json`** → content matrix and rollout plan to merge into `.pseo/taxonomy/_registry.json`

## File 1: `{niche-slug}.json`

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
    "authoritative_sources": {
      "industry_publications": ["Top industry media/publications for this niche"],
      "data_sources": ["Sites with stats, reports, and research data"],
      "competitor_blogs": ["Competitor blogs with quality content to reference"],
      "official_resources": ["Official docs, government sites, or standards bodies"]
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

## File 2: `_registry-update.json`

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
