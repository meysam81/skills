# SEO Blog Engine — Claude Code Plugin

A family of 6 skills for producing SEO-optimized blog posts through a structured, multi-session workflow.

## Skills

| #   | Skill        | Command         | Purpose                                         |
| --- | ------------ | --------------- | ----------------------------------------------- |
| 1   | seo-research | `/seo-research` | Generate a deep research prompt for Claude.ai   |
| 2   | seo-keywords | `/seo-keywords` | Keyword research & SERP analysis via DataForSEO |
| 3   | seo-outline  | `/seo-outline`  | Content brief & detailed outline                |
| 4   | seo-write    | `/seo-write`    | Full blog post draft from outline               |
| 5   | seo-review   | `/seo-review`   | SEO audit with 100-point scoring rubric         |
| 6   | seo-optimize | `/seo-optimize` | Post-publish validation via DataForSEO          |

## Workflow

```
/seo-research  →  (paste into Claude.ai deep research)
                        ↓
                  save to docs/seo/research-brief.md
                        ↓
/seo-keywords  →  docs/seo/keyword-data.md
                        ↓
/seo-outline   →  docs/seo/outline.md  →  (user reviews/edits)
                        ↓
/seo-write     →  docs/seo/draft.md
                        ↓
/seo-review    →  100-point score + fix recommendations
                        ↓
               (publish the post)
                        ↓
/seo-optimize  →  post-publish health report
```

## Setup

### Prerequisites

- Claude Code CLI installed
- DataForSEO MCP server configured (required for `/seo-keywords` and `/seo-optimize`)

### Installation

This plugin is installed at `~/.claude/plugins/seo-blog-engine/`. Claude Code discovers it automatically.

### Per-Project Setup

Create `docs/seo/brand-voice.md` in your project with:

```markdown
# Brand Voice

## Company

- Name, product, industry

## Tone

- Professional/casual/technical, personality traits

## Target Audience

- ICP descriptions, pain points, expertise levels

## Terminology

- Preferred terms, terms to avoid

## Competitors

- Names and domains (used for gap analysis)
```

## References

The `references/` directory contains distilled SEO knowledge used by the skills:

| File                         | Content                                                         |
| ---------------------------- | --------------------------------------------------------------- |
| `on-page-seo-checklist.md`   | Title, meta, headers, links, images, URL, schema, mobile, speed |
| `dataforseo-workflows.md`    | 4 tool workflows with exact parameters and thresholds           |
| `eeat-signals.md`            | E-E-A-T implementation guide by content type                    |
| `schema-markup-templates.md` | Article, FAQ, HowTo, BreadcrumbList JSON-LD templates           |

## Per-Project Files

Each project using these skills maintains files in `docs/seo/`:

| File                | Created by            | Purpose                                              |
| ------------------- | --------------------- | ---------------------------------------------------- |
| `brand-voice.md`    | User (manual)         | Tone, audience, terminology, competitors             |
| `research-brief.md` | User (from Claude.ai) | Facts, stats, quotes, case studies                   |
| `keyword-data.md`   | `/seo-keywords`       | Keyword analysis with volumes, difficulty, SERP data |
| `outline.md`        | `/seo-outline`        | Approved content structure and keyword mapping       |
| `draft.md`          | `/seo-write`          | Complete blog post draft with schema markup          |
