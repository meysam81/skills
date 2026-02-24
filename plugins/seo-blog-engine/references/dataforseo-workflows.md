# DataForSEO Tool Workflows

Exact tool sequences with parameters for the SEO blog engine skills. All tools are accessed via the DataForSEO MCP server.

## Workflow 1: Keyword Research

Used by `seo-keywords` skill.

### Step 1 — Seed Expansion (parallel)

**Tool**: `dataforseo_labs_google_keyword_suggestions`

```
keyword: "{{seed keyword}}"
location_code: 2840  (US; adjust per target)
language_code: "en"
include_seed_keyword: true
limit: 50
```

**Tool**: `dataforseo_labs_google_keyword_ideas`

```
keywords: ["{{seed keyword}}"]
location_code: 2840
language_code: "en"
limit: 50
```

Merge results, deduplicate by keyword.

### Step 2 — Intent Classification

**Tool**: `dataforseo_labs_search_intent`

```
keywords: [{{merged keyword list, max 100}}]
language_code: "en"
```

Filter: Keep `informational` and `commercial_investigation` intents. Drop `navigational`. Flag `transactional` as potential product page targets.

### Step 3 — Difficulty Assessment

**Tool**: `dataforseo_labs_bulk_keyword_difficulty`

```
keywords: [{{filtered keywords}}]
location_code: 2840
language_code: "en"
```

Categorize:

- KD 0-30: Easy (target for new domains)
- KD 31-50: Medium (target for established domains)
- KD 51-70: Hard (need strong backlink strategy)
- KD 71+: Very hard (skip unless domain is authoritative)

### Step 4 — Detailed Validation

**Tool**: `dataforseo_labs_google_keyword_overview`

```
keywords: [{{top 20 candidates}}]
location_code: 2840
language_code: "en"
```

Extract: search_volume, cpc, competition_level, keyword_properties (clickstream data). Reject keywords with search_volume < 50/mo for B2B (< 500/mo for B2C).

### Step 5 — Trend Analysis

**Tool**: `kw_data_dfs_trends_explore`

```
keywords: ["{{primary keyword}}", "{{top 3 secondaries}}"]
location_code: 2840
date_from: "{{12 months ago YYYY-MM-DD}}"
date_to: "{{today YYYY-MM-DD}}"
```

Reject if trend declines > 30% over 12 months. Flag seasonal patterns.

### Step 6 — SERP Analysis

**Tool**: `serp_organic_live_advanced`

```
keyword: "{{primary keyword}}"
location_code: 2840
language_code: "en"
device: "desktop"
os: "windows"
depth: 10
```

Extract from top 10 results:

- URL, title, description
- SERP features present (featured_snippet, people_also_ask, video, etc.)
- Position, estimated traffic

### Step 7 — Competitor Gap Analysis

**Tool**: `dataforseo_labs_google_domain_intersection`

```
targets: {
  "1": "{{competitor1.com}}",
  "2": "{{competitor2.com}}",
  "3": "{{competitor3.com}}"
}
exclude: "{{your-domain.com}}"
location_code: 2840
language_code: "en"
limit: 50
```

Identifies keywords competitors rank for that your domain doesn't.

---

## Workflow 2: SERP & Competitor Content Analysis

Used by `seo-keywords` and `seo-outline` skills.

### Step 1 — SERP Results

**Tool**: `serp_organic_live_advanced` (same as Workflow 1, Step 6)

### Step 2 — Content Parsing (top 5 results)

**Tool**: `on_page_content_parsing`

```
url: "{{each top-5 URL}}"
```

For each, extract:

- Heading structure (H1, H2, H3 hierarchy)
- Word count
- Internal/external link count
- Image count
- Schema markup present

### Step 3 — Backlink Benchmark

**Tool**: `backlinks_summary`

```
target: "{{each top-5 URL}}"
```

Extract: referring_domains, backlinks count. Calculate median as benchmark.

### Step 4 — Ranked Keywords per Competitor

**Tool**: `dataforseo_labs_google_ranked_keywords`

```
target: "{{competitor domain}}"
location_code: 2840
language_code: "en"
limit: 20
filters: ["keyword_data.keyword_info.search_volume", ">", 100]
```

### Step 5 — Subdomains Check

**Tool**: `dataforseo_labs_google_subdomains`

```
target: "{{competitor domain}}"
location_code: 2840
```

Identifies if competitors use blog subdomain vs. subfolder.

---

## Workflow 3: Competitor Deep Dive

Used by `seo-keywords` skill for content gap identification.

### Step 1 — Domain Rank Overview

**Tool**: `dataforseo_labs_google_domain_rank_overview`

```
target: "{{competitor domain}}"
location_code: 2840
language_code: "en"
```

### Step 2 — Top Keywords

**Tool**: `dataforseo_labs_google_keywords_for_site`

```
target: "{{competitor domain}}"
location_code: 2840
language_code: "en"
limit: 50
order_by: ["keyword_data.keyword_info.search_volume,desc"]
```

### Step 3 — Page Intersection

**Tool**: `dataforseo_labs_google_page_intersection`

```
pages: {
  "1": "{{your top page URL}}",
  "2": "{{competitor page URL}}"
}
location_code: 2840
```

### Step 4 — Related Keywords

**Tool**: `dataforseo_labs_google_related_keywords`

```
keyword: "{{primary keyword}}"
location_code: 2840
language_code: "en"
limit: 30
```

### Step 5 — SERP Competitors

**Tool**: `dataforseo_labs_google_serp_competitors`

```
keywords: ["{{primary keyword}}", "{{secondary keywords}}"]
location_code: 2840
language_code: "en"
```

### Step 6 — Historical SERP

**Tool**: `dataforseo_labs_google_historical_serp`

```
keyword: "{{primary keyword}}"
location_code: 2840
language_code: "en"
```

Shows SERP volatility — stable SERPs are harder to break into.

### Step 7 — Relevant Pages

**Tool**: `dataforseo_labs_google_relevant_pages`

```
target: "{{your domain}}"
keywords: ["{{primary keyword}}", "{{secondaries}}"]
location_code: 2840
```

Identifies if you already have a page that could rank (update vs. create new).

---

## Workflow 4: Post-Publish Validation

Used by `seo-optimize` skill.

### Step 1 — On-Page Verification

**Tool**: `on_page_content_parsing`

```
url: "{{published URL}}"
```

Verify: title tag, meta description, H1, heading hierarchy, link count, word count match outline targets.

### Step 2 — Lighthouse Audit

**Tool**: `on_page_lighthouse`

```
url: "{{published URL}}"
categories: ["performance", "seo", "accessibility"]
device: "mobile"
```

Targets: SEO > 90, Performance > 50, Accessibility > 80.

### Step 3 — Ranking Check

**Tool**: `dataforseo_labs_google_ranked_keywords`

```
target: "{{your domain}}"
location_code: 2840
filters: ["keyword_data.keyword", "=", "{{primary keyword}}"]
```

Check initial ranking position (may take days to index).

### Step 4 — Content Mentions

**Tool**: `content_analysis_search`

```
keyword: "{{brand name OR article title}}"
search_mode: "as_is"
```

Check for web mentions and sentiment.

### Step 5 — AI Visibility

**Tool**: `ai_opt_llm_ment_search`

```
keyword: "{{primary keyword}}"
```

Check if content appears in AI-generated search results.

### Step 6 — Traffic Estimation

**Tool**: `dataforseo_labs_bulk_traffic_estimation`

```
targets: ["{{published URL}}"]
location_code: 2840
```

Baseline traffic estimate for tracking growth.

---

## Location & Language Codes

| Market    | Location Code | Language Code |
| --------- | ------------- | ------------- |
| US        | 2840          | en            |
| UK        | 2826          | en            |
| Canada    | 2124          | en            |
| Australia | 2036          | en            |
| Germany   | 2276          | de            |
| France    | 2250          | fr            |

Use `ai_opt_kw_data_loc_and_lang` to look up additional location/language codes.
