# SEO Blog Engine — Claude Code Plugin

A family of 9 skills for producing SEO-optimized blog posts, managing internal linking, and executing backlink outreach through a structured, multi-session workflow.

## Skills

### Content Production Pipeline

| #   | Skill        | Command                   | Purpose                                         |
| --- | ------------ | ------------------------- | ----------------------------------------------- |
| 1   | seo-research | `/seo-research <topic>`   | Generate a deep research prompt for Claude.ai   |
| 2   | seo-keywords | `/seo-keywords <keyword>` | Keyword research & SERP analysis via DataForSEO |
| 3   | seo-outline  | `/seo-outline [slug]`     | Content brief & detailed outline                |
| 4   | seo-write    | `/seo-write [slug]`       | Full blog post draft from outline               |
| 5   | seo-review   | `/seo-review [slug]`      | SEO audit with 100-point scoring rubric         |
| 6   | seo-optimize | `/seo-optimize <url>`     | Post-publish validation via DataForSEO          |
| 7   | seo-social   | `/seo-social [slug]`      | Platform-specific social media posts            |

### Site-Wide SEO

| #   | Skill                 | Command                                      | Purpose                                                     |
| --- | --------------------- | -------------------------------------------- | ----------------------------------------------------------- |
| 8   | seo-internal-linking  | `/seo-internal-linking [content-dir]`        | Audit all pages & generate internal linking recommendations |
| 9   | seo-backlink-outreach | `/seo-backlink-outreach [slug\|--full-site]` | Discover backlink prospects & generate outreach templates   |

## Workflow

### Content Production

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
                                              ↓
/seo-social dmarc-failed-guide       →  docs/seo/dmarc-failed-guide/social-posts.md
```

### Internal Linking (run after publishing multiple posts)

```
/seo-internal-linking                →  explores all content pages via subagents
                                              ↓
                                     builds keyword-to-page map
                                              ↓
                                     docs/seo/internal-linking-report.md  →  (user reviews)
                                              ↓
                                     (user confirms → links applied to content files)
```

### Backlink Outreach (run after building content foundation)

```
/seo-backlink-outreach --full-site   →  explores all pages, scores backlink worthiness
                                              ↓
                                     SERP analysis + web search for prospects
                                              ↓
                                     docs/seo/backlink-outreach/strategy-overview.md
                                     docs/seo/backlink-outreach/{slug}-outreach.md (per page)
                                              ↓
                                     (load templates into outreach tool, send campaigns)
```

## Setup

### Prerequisites

- Claude Code CLI installed
- DataForSEO MCP server configured (required for `/seo-keywords`, `/seo-optimize`, `/seo-backlink-outreach`)

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
| `internal-linking-guide.md`  | Readability-first linking principles, density, anchor text      |
| `backlink-outreach-guide.md` | Prospect qualification, email templates, spintax, follow-ups    |
| `social-media-platforms.md`  | Platform constraints and best practices for 7 platforms         |

## Per-Project File Structure

Global files live in `docs/seo/`. Per-article files live in `docs/seo/<slug>/`:

```
docs/seo/
├── brand-voice.md              # shared — tone, audience, terminology
├── topical-clusters.md         # shared — long-term content roadmap
├── internal-linking-report.md  # from /seo-internal-linking
├── backlink-outreach/          # from /seo-backlink-outreach
│   ├── strategy-overview.md    # priority matrix + campaign plan
│   ├── dmarc-failed-guide-outreach.md
│   └── spf-record-setup-outreach.md
├── dmarc-failed-guide/         # per-article directory
│   ├── prompt.md               # from /seo-research
│   ├── research-brief.md       # user saves from Claude.ai
│   ├── keyword-data.md         # from /seo-keywords
│   ├── outline.md              # from /seo-outline
│   ├── draft.md                # from /seo-write
│   └── social-posts.md         # from /seo-social
├── best-dmarc-tools-2026/
│   ├── prompt.md
│   └── ...
└── spf-record-setup/
    ├── prompt.md
    └── ...
```

| File                         | Created by               | Purpose                                              |
| ---------------------------- | ------------------------ | ---------------------------------------------------- |
| `brand-voice.md`             | User (manual)            | Tone, audience, terminology, competitors             |
| `topical-clusters.md`        | User / `/seo-outline`    | Long-term content strategy and pillar map            |
| `internal-linking-report.md` | `/seo-internal-linking`  | Site-wide linking audit and recommendations          |
| `backlink-outreach/*.md`     | `/seo-backlink-outreach` | Prospect lists and outreach email templates          |
| `<slug>/prompt.md`           | `/seo-research`          | Deep research prompt for Claude.ai                   |
| `<slug>/research-brief.md`   | User (from Claude.ai)    | Facts, stats, quotes, case studies                   |
| `<slug>/keyword-data.md`     | `/seo-keywords`          | Keyword analysis with volumes, difficulty, SERP data |
| `<slug>/outline.md`          | `/seo-outline`           | Approved content structure and keyword mapping       |
| `<slug>/draft.md`            | `/seo-write`             | Complete blog post draft with schema markup          |
| `<slug>/social-posts.md`     | `/seo-social`            | Platform-specific social media posts                 |
