# Process

## Step 1: Parse Input Data

Read whatever the user provided. Extract into structured tables:
- Keywords: term, volume, KD, CPC, intent
- Competitors: domain, DR, traffic, keywords, overlap %
- Content gaps: topics with volume but no good coverage

## Step 2: Analyze & Identify Structure

From the keyword data, identify:
- **Subtopics/verticals** — natural clusters in the keywords (these become the `subtopics` array)
- **Content type fit** — which of the 5 content types (resource, guide, alternatives, comparison, tool) make sense
- **Title templates** — deterministic patterns that cover keyword clusters
- **Page count** — how many pages per content type × subtopic
- **Authoritative sources** — domains that appear in SERPs, competitor backlink profiles, or are recognized authorities in the niche. Categorize them into industry publications, data sources, competitor blogs, and official resources. These domains will be used during content generation to find specific reference URLs for outbound links.

The golden rule: every page must target a real keyword cluster with proven volume. No speculative pages.

## Step 3: Generate Both Files

Write the two JSON files. Present them to the user as code blocks they can copy into the repo.

## Step 4: Summary

End with a brief summary:
- Total pages planned
- Estimated monthly search volume addressable
- Top 3 highest-opportunity keywords
- Recommended Batch 1 focus
