# `seo-social` Skill Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Add a `seo-social` skill to seo-blog-engine that generates platform-specific social media posts from a blog draft across 7 platforms in 3 tiers.

**Architecture:** Single SKILL.md file at `skills/seo-social/SKILL.md` + one reference file `references/social-media-platforms.md` containing platform constraints. Skill reads `draft.md` + optional `outline.md` + `brand-voice.md`, outputs `social-posts.md`. No external tools (pure LLM).

**Tech Stack:** Markdown skill file (Claude Code plugin system), no code dependencies.

---

### Task 1: Create the platform reference file

**Files:**

- Create: `references/social-media-platforms.md`

**Step 1: Write the reference file**

Write to `references/social-media-platforms.md`:

```markdown
# Social Media Platform Reference

Platform constraints and best practices for content distribution. All data sourced from platform documentation and engagement studies (2025-2026).

## Character Limits

| Platform        | Post Limit          | Notes                                             |
| --------------- | ------------------- | ------------------------------------------------- |
| X (free)        | 280 chars           | Premium: 25,000; Premium+ articles: 100,000       |
| Bluesky         | 300 chars           | Equal for all users, no premium tier              |
| Mastodon        | 500 chars (default) | Instance-configurable; some allow 5,000+          |
| LinkedIn        | 3,000 chars         | "See more" cutoff: ~140 (mobile) / ~210 (desktop) |
| Reddit          | 40,000 chars        | Title: 300 chars. 80,000 for Premium              |
| Patreon         | Unlimited           | Preview text: first 140 chars                     |
| Buy Me a Coffee | Unlimited           | Simpler, shorter-form oriented                    |

## Link Behavior

| Platform        | Link Penalty?        | Strategy                                    |
| --------------- | -------------------- | ------------------------------------------- |
| LinkedIn        | ~60% reach reduction | Post must be self-contained; link secondary |
| X               | Removed Oct 2025     | Links safe; include naturally               |
| Reddit          | Community-moderated  | 90/10 rule: 90% value, 10% promotional      |
| Bluesky         | None                 | Link cards render well                      |
| Mastodon        | None                 | Purely chronological, link-friendly         |
| Patreon         | N/A                  | Direct to subscribers                       |
| Buy Me a Coffee | N/A                  | Direct to supporters                        |

## Platform Tones

### Short-Form Platforms

**X (Twitter):**

- Punchy, opinionated, personality-driven
- 1-2 hashtags max (more looks spammy)
- 70-100 chars optimal for engagement on free tier
- Threads (4-8 tweets) for longer content

**Bluesky:**

- Conversational, authentic, anti-corporate
- Privacy-first, tech-savvy audience
- Minimal hashtags; custom feeds drive discovery
- Threads via reply chains for longer content

**Mastodon:**

- Community-first, respectful, inclusive
- CamelCase hashtags required (screen reader accessibility)
- 3-5 hashtags critical for discovery (no algorithm)
- Content Warnings (CW) used liberally
- Instance culture varies; observe before posting

### Long-Form Platforms

**LinkedIn:**

- Professional but human, not press-release tone
- First 210 chars are the hook (pre-"See more")
- 1,300-1,900 chars is the engagement sweet spot
- 3-5 hashtags, storytelling + professional insight framing
- Carousel/PDF posts get highest engagement (~6.60%)

**Reddit:**

- Anti-marketing, value-first, authentic
- "Lessons learned" / "Here's how we solved it" framing
- No hashtags, no emojis (mostly)
- Must read subreddit rules before posting
- 15-25 quality comments before first self-promotion

### Patronage Platforms

**Patreon:**

- Personal, behind-the-scenes, exclusive
- Teases extended content (templates, code, deeper analysis)
- Tier-aware content suggestions
- Mix: 60-70% durable, 20-30% interactive, 10% exclusive drops

**Buy Me a Coffee:**

- Casual, grateful, simple
- "If you found this valuable" framing
- Lower commitment than Patreon
- Tip-jar model; brief updates work best
```

**Step 2: Verify the file exists**

Run: `ls -la references/social-media-platforms.md`
Expected: File exists with content.

**Step 3: Commit**

```bash
git add references/social-media-platforms.md
git commit -m "feat(seo-social): add social media platform reference data"
```

---

### Task 2: Create the seo-social skill directory and SKILL.md

**Files:**

- Create: `skills/seo-social/SKILL.md`

**Step 1: Write the skill file**

Write to `skills/seo-social/SKILL.md`:

```markdown
---
name: seo-social
description: Generate platform-specific social media posts from a blog draft. Covers X, Bluesky, Mastodon, LinkedIn, Reddit, Patreon, and Buy Me a Coffee in 3 tiers (short-form, long-form, patronage).
model: opus
---

# Social Media Post Generator

Generate platform-specific social media content from a published or ready-to-publish blog post. Produces a single file with posts tailored for 7 platforms across 3 tiers.

## References

Read this before generating (relative to this skill's base directory):

- `../../references/social-media-platforms.md`

## Inputs

1. **Draft** — `docs/seo/<slug>/draft.md` (required — the blog post)
2. **Outline** — `docs/seo/<slug>/outline.md` (optional — for primary keyword and keyword data)
3. **Brand voice** — `docs/seo/brand-voice.md` (optional — for tone adaptation)
4. **Article directory** — user specifies `docs/seo/<slug>/` or skill finds the directory containing `draft.md`

## Article Directory Convention

All per-article files live in `docs/seo/<slug>/`. The user provides the slug, or the skill finds it by looking for the directory containing `draft.md`.

## Mode Selection

**Default mode (3-tier):** Generates one post per platform using shared insight extraction per tier, then applies platform-specific formatting and tone adjustments.

**Per-platform mode:** If the user provides `--per-platform` as an argument, generate each of the 7 posts independently with full platform-specific voice, tone, and cultural adaptation. This uses more tokens but produces maximum authenticity per platform.

## Process

### Step 1 — Read and Analyze the Draft

Read `draft.md` completely. Also read `outline.md` and `brand-voice.md` if they exist.

Extract from the draft:

- **Blog title** (from H1 or frontmatter `title`)
- **Primary keyword** (from frontmatter `primary_keyword` or outline)
- **Blog URL/slug** (from frontmatter `slug`)
- **Core insight** — the single most compelling takeaway the reader walks away with
- **Key supporting points** — 3-5 specific facts, stats, or examples that reinforce the core insight
- **Target audience** — inferred from brand voice or draft content
- **Unique angle** — what makes this post different from competitors covering the same topic

### Step 2 — Generate Short-Form Posts (Tier 1)

Generate posts for X, Bluesky, and Mastodon. Each must be a genuinely different take on the core insight, not the same text truncated to different lengths.

**X (Twitter) — 280 chars max:**

- Lead with a punchy, opinionated hook
- Include the blog link naturally
- Add 1-2 relevant hashtags
- Personality-driven; avoid corporate tone
- Aim for 70-100 chars of text before the link for maximum impact

**Bluesky — 300 chars max:**

- Conversational, authentic tone
- Include the blog link (renders as a link card with preview)
- Minimal or no hashtags — discovery happens via custom feeds
- Anti-corporate voice; write like a person, not a brand

**Mastodon — 500 chars max:**

- Slightly more detail than X/Bluesky; use the extra space for context
- 3-5 CamelCase hashtags (e.g., #WebDevelopment, not #webdevelopment)
- Suggest a Content Warning (CW) line if the topic could be sensitive
- Community-first tone; inclusive language
- Include the blog link; Mastodon is link-friendly

### Step 3 — Generate Long-Form Posts (Tier 2)

Generate posts for LinkedIn and Reddit. These are self-contained — a reader should get value without clicking the link.

**LinkedIn — 1,300-1,900 chars:**

- **Hook (first 210 chars)**: Must be compelling and self-contained. This is what appears before "See more" on desktop. Write it as if it's the only thing people will read.
- **Body**: Professional insight framing. Use storytelling or "here's what I learned" structure. Short paragraphs (1-2 sentences). White space between paragraphs.
- **Link placement**: At the very end, after delivering full value. LinkedIn penalizes link-first posts ~60% in reach.
- **Hashtags**: 3-5 relevant hashtags at the bottom
- **Primary keyword**: Include naturally for LinkedIn SEO
- **Do NOT**: Use emojis as bullet points. No "🔥 Hot take:" or "💡 Insight:" patterns.

**Reddit:**

- **Framing**: "Lessons learned" / "Here's how we solved X" / "I built X, here's what happened"
- **Tone**: Value-first, anti-marketing, authentic. Write like you're sharing with peers, not promoting
- **Structure**: Use markdown headers, bullets, and code blocks for readability
- **Link**: At the bottom: "Full write-up with more details: [link]"
- **No hashtags, minimal emojis**
- **Suggested subreddits**: Include 2-4 relevant subreddit suggestions based on the blog topic
- **Note**: Add reminder about checking subreddit rules and the 90/10 rule

### Step 4 — Generate Patronage Posts (Tier 3)

Generate teasers for Patreon and Buy Me a Coffee. These are NOT excerpts — they're framed as exclusive/extended content teasers that drive subscriptions.

**Patreon — ~300 words:**

- Behind-the-scenes framing: share the story behind the blog post
- Tease extended content that patrons get: deeper analysis, source code, templates, raw data, video walkthrough, etc.
- Suggest which membership tier this content fits
- Suggest an "Extras" idea (a digital product tie-in if applicable)
- Personal, intimate tone — patrons are paying for access to you, not just content

**Buy Me a Coffee — ~150 words:**

- Gratitude-focused: "If you found [blog title] helpful..."
- Brief, warm, simple
- Mention what their support enables (more posts, tools, research)
- Clear but soft CTA
- Do not hard-sell; this is a tip jar, not a subscription pitch

### Step 5 — Quality Checks

Before outputting, verify every item:

**Character limits:**

- [ ] X post ≤ 280 chars
- [ ] Bluesky post ≤ 300 chars
- [ ] Mastodon post ≤ 500 chars
- [ ] LinkedIn post 1,300-1,900 chars (within 3,000 max)

**Content quality:**

- [ ] LinkedIn hook (first 210 chars) works as a standalone teaser
- [ ] Reddit post reads as value-first, not promotional
- [ ] Mastodon hashtags are CamelCase
- [ ] Primary keyword appears naturally in LinkedIn and Reddit posts
- [ ] No copy-paste between platforms — each is genuinely adapted to platform culture
- [ ] Brand voice is consistent across all tiers (adapted per platform but recognizably the same author)

**Patronage quality:**

- [ ] Patreon post teases exclusive content, not just repeats the blog
- [ ] Buy Me a Coffee post is gratitude-focused, not sales-focused
- [ ] Both patronage posts have clear but non-aggressive CTAs

**If any check fails:** Fix the post before outputting. Recount characters if edits were made.

## Output

Write to `docs/seo/<slug>/social-posts.md`:
```

---

title: "Social Posts: {{blog title}}"
source: "docs/seo/<slug>/draft.md"
generated: "{{YYYY-MM-DD}}"
primary_keyword: "{{primary keyword}}"

---

# Social Media Posts

## Short-Form

### X (Twitter)

> {{post text including link}}

**Char count:** {{N}}/280
**Hashtags:** {{#tag1 #tag2}}

### Bluesky

> {{post text including link}}

**Char count:** {{N}}/300

### Mastodon

> {{post text including link}}

**Char count:** {{N}}/500
**CW (optional):** {{topic if applicable, or "None"}}
**Hashtags:** {{#CamelCaseTag1 #CamelCaseTag2 #CamelCaseTag3}}

---

## Long-Form

### LinkedIn

> {{full post text with link at end}}

**Char count:** {{N}}/3,000
**Hook (first 210 chars):** {{preview of what appears before "See more"}}
**Hashtags:** {{#tag1 #tag2 #tag3}}

### Reddit

> {{full post text with link at bottom}}

**Suggested subreddits:** {{r/sub1, r/sub2, r/sub3}}
**Note:** Check each subreddit's rules before posting. Contribute value in comments before self-promoting.

---

## Patronage

### Patreon

> {{~300 word teaser}}

**Tier suggestion:** {{recommended tier}}
**Extras idea:** {{digital product tie-in or "N/A"}}

### Buy Me a Coffee

> {{~150 word update}}

---

## Platform-Specific Notes

- LinkedIn: Post is self-contained; link at end only. Algorithm penalizes link-first posts ~60%.
- X: Link penalty removed Oct 2025. Thread version recommended for deeper engagement.
- Reddit: Rewrite framing per subreddit culture. 90/10 rule applies.
- Bluesky: No link penalty. Consider thread (3-5 posts) for deeper engagement.
- Mastodon: CamelCase hashtags are a cultural norm. Alt text required for any images.
- Patreon: This is a teaser for exclusive content, not a blog promotion.
- Buy Me a Coffee: Keep it simple and grateful. Embed the widget on your blog.

```

## User Action

After generating, tell the user:

> Social posts saved to `docs/seo/<slug>/social-posts.md`. Review each platform's post and adjust for your personal voice before publishing. Pay special attention to:
> - LinkedIn hook (first 210 chars) — this determines 60-70% of engagement
> - Reddit framing — adapt to each subreddit's specific culture
> - Character counts — verify before posting, especially X and Bluesky
>
> For deeper per-platform customization, re-run with `/seo-social <slug> --per-platform`.
```

**Step 2: Verify the file exists and is well-formed**

Run: `head -5 skills/seo-social/SKILL.md`
Expected: YAML frontmatter with `name: seo-social`, `description`, `model: opus`.

**Step 3: Commit**

```bash
git add skills/seo-social/SKILL.md
git commit -m "feat(seo-social): add social media post generator skill"
```

---

### Task 3: Update plugin.json

**Files:**

- Modify: `.claude-plugin/plugin.json`

**Step 1: Update description and keywords**

Update `plugin.json` to reflect the new skill:

```json
{
  "name": "seo-blog-engine",
  "description": "SEO blog post production pipeline: research prompts, keyword analysis, outlines, writing, review, post-publish validation, and social media distribution",
  "version": "1.2.0",
  "author": {
    "name": "Meysam"
  },
  "license": "UNLICENSED",
  "keywords": [
    "seo",
    "blog",
    "content",
    "keywords",
    "dataforseo",
    "writing",
    "social-media"
  ]
}
```

Changes:

- `description`: Added ", and social media distribution" at end
- `version`: Bumped `1.1.0` → `1.2.0` (minor version for new feature)
- `keywords`: Added `"social-media"`

**Step 2: Verify JSON is valid**

Run: `python3 -c "import json; json.load(open('.claude-plugin/plugin.json'))"`
Expected: No error output (valid JSON).

**Step 3: Commit**

```bash
git add .claude-plugin/plugin.json
git commit -m "chore: bump version to 1.2.0, add social-media keyword"
```

---

### Task 4: Verify skill registration

**Step 1: Check skill is discoverable**

Run: `ls -la skills/seo-social/SKILL.md`
Expected: File exists.

**Step 2: Verify directory structure matches plugin conventions**

Run: `find . -name 'SKILL.md' | sort`
Expected output:

```
./skills/seo-keywords/SKILL.md
./skills/seo-optimize/SKILL.md
./skills/seo-outline/SKILL.md
./skills/seo-research/SKILL.md
./skills/seo-review/SKILL.md
./skills/seo-social/SKILL.md
./skills/seo-write/SKILL.md
```

**Step 3: Verify reference file is in place**

Run: `ls references/`
Expected output:

```
dataforseo-workflows.md
eeat-signals.md
on-page-seo-checklist.md
schema-markup-templates.md
social-media-platforms.md
```

**Step 4: Test relative reference path resolves**

Run: `ls skills/seo-social/../../references/social-media-platforms.md`
Expected: File found (relative path from skill to reference works).

---

## Summary

| Task | Creates/Modifies                       | Purpose                        |
| ---- | -------------------------------------- | ------------------------------ |
| 1    | `references/social-media-platforms.md` | Platform constraints reference |
| 2    | `skills/seo-social/SKILL.md`           | The skill itself               |
| 3    | `.claude-plugin/plugin.json`           | Plugin metadata update         |
| 4    | (verification only)                    | Confirm everything works       |

Total new files: 2. Modified files: 1. Commits: 3.
