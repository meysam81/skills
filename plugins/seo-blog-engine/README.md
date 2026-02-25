# SEO Blog Engine — Claude Code Plugin

A family of 6 skills for producing SEO-optimized blog posts through a structured, multi-session workflow.

## Skills

| #   | Skill        | Command                   | Purpose                                         |
| --- | ------------ | ------------------------- | ----------------------------------------------- |
| 1   | seo-research | `/seo-research <topic>`   | Generate a deep research prompt for Claude.ai   |
| 2   | seo-keywords | `/seo-keywords <keyword>` | Keyword research & SERP analysis via DataForSEO |
| 3   | seo-outline  | `/seo-outline [slug]`     | Content brief & detailed outline                |
| 4   | seo-write    | `/seo-write [slug]`       | Full blog post draft from outline               |
| 5   | seo-review   | `/seo-review [slug]`      | SEO audit with 100-point scoring rubric         |
| 6   | seo-optimize | `/seo-optimize <url>`     | Post-publish validation via DataForSEO          |

## Workflow

```
/seo-research "DMARC Failed Guide"  →  docs/seo/dmarc-failed-guide/prompt.md
                                              ↓
                                     (paste prompt into Claude.ai deep research)
                                              ↓
                                     save output to docs/seo/dmarc-failed-guide/research-brief.md
                                              ↓
/seo-keywords "dmarc failed"         →  docs/seo/dmarc-failed-guide/keyword-data.md
                                              ↓
/seo-outline dmarc-failed-guide      →  docs/seo/dmarc-failed-guide/outline.md  →  (user reviews)
                                              ↓
/seo-write dmarc-failed-guide        →  docs/seo/dmarc-failed-guide/draft.md
                                              ↓
/seo-review dmarc-failed-guide       →  100-point score + fix recommendations
                                              ↓
                                     (publish the post)
                                              ↓
/seo-optimize <published-url>        →  post-publish health report
```

## Setup

### Prerequisites

- Claude Code CLI installed
- DataForSEO MCP server configured (required for `/seo-keywords` and `/seo-optimize`)

### Installation

This plugin is installed at `~/.claude/plugins/seo-blog-engine/`. Claude Code discovers it automatically.

### Per-Project Setup

Create these files in `docs/seo/`:

**`brand-voice.md`** (required) — tone, audience, terminology, competitors:

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

**`topical-clusters.md`** (optional) — long-term content strategy roadmap:

```markdown
# Topical Clusters

## Pillar 1: [Topic]

- P1.0: [Pillar post title]
- P1.1: [Supporting post]
- P1.2: [Supporting post]

## Pillar 2: [Topic]

...
```

When present, skills use the cluster map to plan internal linking, understand each post's strategic role (TOFU/MOFU/BOFU), and maintain content coherence across the blog.

## References

The `references/` directory contains distilled SEO knowledge used by the skills:

| File                         | Content                                                         |
| ---------------------------- | --------------------------------------------------------------- |
| `on-page-seo-checklist.md`   | Title, meta, headers, links, images, URL, schema, mobile, speed |
| `dataforseo-workflows.md`    | 4 tool workflows with exact parameters and thresholds           |
| `eeat-signals.md`            | E-E-A-T implementation guide by content type                    |
| `schema-markup-templates.md` | Article, FAQ, HowTo, BreadcrumbList JSON-LD templates           |

## Per-Project File Structure

Global files live in `docs/seo/`. Per-article files live in `docs/seo/<slug>/`:

```
docs/seo/
├── brand-voice.md              # shared — tone, audience, terminology
├── topical-clusters.md         # shared — long-term content roadmap
├── dmarc-failed-guide/         # per-article directory
│   ├── prompt.md               # from /seo-research
│   ├── research-brief.md       # user saves from Claude.ai
│   ├── keyword-data.md         # from /seo-keywords
│   ├── outline.md              # from /seo-outline
│   └── draft.md                # from /seo-write
├── best-dmarc-tools-2026/
│   ├── prompt.md
│   └── ...
└── spf-record-setup/
    ├── prompt.md
    └── ...
```

| File                       | Created by            | Purpose                                              |
| -------------------------- | --------------------- | ---------------------------------------------------- |
| `brand-voice.md`           | User (manual)         | Tone, audience, terminology, competitors             |
| `topical-clusters.md`      | User / `/seo-outline` | Long-term content strategy and pillar map            |
| `<slug>/prompt.md`         | `/seo-research`       | Deep research prompt for Claude.ai                   |
| `<slug>/research-brief.md` | User (from Claude.ai) | Facts, stats, quotes, case studies                   |
| `<slug>/keyword-data.md`   | `/seo-keywords`       | Keyword analysis with volumes, difficulty, SERP data |
| `<slug>/outline.md`        | `/seo-outline`        | Approved content structure and keyword mapping       |
| `<slug>/draft.md`          | `/seo-write`          | Complete blog post draft with schema markup          |
