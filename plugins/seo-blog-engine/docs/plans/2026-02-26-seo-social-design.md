# Design: `seo-social` Skill

**Date:** 2026-02-26
**Status:** Approved
**Plugin:** seo-blog-engine
**Position in pipeline:** After `/seo-write` → `draft.md`, optionally after `/seo-review`

## Purpose

Generate platform-specific social media content from a blog post draft for distribution across 7 platforms: X, Bluesky, Mastodon, LinkedIn, Reddit, Patreon, and Buy Me a Coffee.

## Inputs

| File                         | Required | Source                            |
| ---------------------------- | -------- | --------------------------------- |
| `docs/seo/<slug>/draft.md`   | Yes      | `/seo-write`                      |
| `docs/seo/<slug>/outline.md` | No       | `/seo-outline` (for keyword data) |
| `docs/seo/brand-voice.md`    | No       | Shared (for tone adaptation)      |

## Output

Single file: `docs/seo/<slug>/social-posts.md`

## 3-Tier Generation Model

### Tier 1: Short-Form Social (X, Bluesky, Mastodon)

Extracts the single most compelling insight/hook. Platform adjustments:

- **X (280 chars)**: Punchy, opinionated hook + link. 1-2 hashtags max.
- **Bluesky (300 chars)**: Conversational, authentic + link card. Minimal hashtags.
- **Mastodon (500 chars)**: More detail, 3-5 CamelCase hashtags, optional CW field.

### Tier 2: Long-Form Social (LinkedIn, Reddit)

Self-contained posts repurposing core insights:

- **LinkedIn (1,300-1,900 chars)**: Professional insight framing. Hook in first 210 chars (pre-"See more"). Link at end. 3-5 hashtags. Algorithm penalizes link-first posts ~60%.
- **Reddit**: "Lessons learned" / "Here's how we solved it" framing. Value-first native post, link at bottom. No hashtags. Includes suggested subreddit targets. Must respect 90/10 rule.

### Tier 3: Patronage (Patreon, Buy Me a Coffee)

Not excerpts — "exclusive extended" teasers:

- **Patreon (~300 words)**: Behind-the-scenes framing, teases extended content/extras (templates, code, deeper analysis). Tier suggestion + extras idea.
- **Buy Me a Coffee (~150 words)**: Gratitude-focused update. "If you found this valuable" framing with supporter CTA.

## Output File Format

```markdown
---
title: "Social Posts: {{blog title}}"
source: "docs/seo/<slug>/draft.md"
generated: "{{YYYY-MM-DD}}"
primary_keyword: "{{from outline or inferred}}"
---

# Social Media Posts

## Short-Form

### X (Twitter)

> {{280-char post}}

**Char count:** {{N}}/280
**Hashtags:** #tag1 #tag2

### Bluesky

> {{300-char post}}

**Char count:** {{N}}/300

### Mastodon

> {{500-char post}}

**Char count:** {{N}}/500
**CW (optional):** {{topic if applicable}}
**Hashtags:** #CamelCaseTag1 #CamelCaseTag2

---

## Long-Form

### LinkedIn

> {{1,300-1,900 char post}}

**Char count:** {{N}}/3,000
**Hook (first 210 chars):** {{preview text}}
**Hashtags:** #tag1 #tag2 #tag3

### Reddit

> {{native text post, value-first}}

**Suggested subreddits:** r/sub1, r/sub2, r/sub3
**Note:** Check each subreddit's rules before posting.

---

## Patronage

### Patreon

> {{~300 word teaser with exclusive content angle}}

**Tier suggestion:** {{which tier this fits}}
**Extras idea:** {{digital product tie-in if applicable}}

### Buy Me a Coffee

> {{~150 word gratitude-focused update}}

---

## Platform-Specific Notes

- LinkedIn: Post is self-contained; link at end. Algorithm penalizes link-first posts ~60%.
- X: Link penalty removed Oct 2025. Thread version recommended for deeper engagement.
- Reddit: Rewrite framing for each subreddit's culture. 90/10 rule applies.
- Bluesky: No link penalty. Consider thread (3-5 posts) for more engagement.
- Mastodon: CamelCase hashtags are cultural norm. Alt text required for images.
- Patreon: Teaser for exclusive content, not blog promotion.
- Buy Me a Coffee: Keep it simple and grateful. Embed widget on blog itself.
```

## Quality Checks

1. All character counts within platform limits
2. LinkedIn hook (first 210 chars) is compelling and self-contained
3. X post fits within 280 chars (no truncation)
4. Bluesky post fits within 300 chars
5. Mastodon hashtags are CamelCase
6. Reddit post reads as value-first, not promotional
7. Primary keyword appears naturally in long-form posts
8. Brand voice consistent across all tiers
9. No copy-paste between platforms — each genuinely adapted

## Per-Platform Flag

Default: 3-tier generation (shared base per tier, platform-specific adjustments).

`/seo-social --per-platform`: Generate each of 7 posts independently with full platform-specific voice/tone/culture adaptation. More tokens, maximum authenticity.

## Platform Constraints (Research-Backed)

| Platform        | Char Limit    | Link Penalty        | Tone                        |
| --------------- | ------------- | ------------------- | --------------------------- |
| X (free)        | 280           | Removed Oct 2025    | Punchy, opinionated         |
| Bluesky         | 300           | None                | Conversational, authentic   |
| Mastodon        | 500 (default) | None                | Community-first, inclusive  |
| LinkedIn        | 3,000         | ~60% reach loss     | Professional but human      |
| Reddit          | 40,000        | Community-moderated | Anti-marketing, value-first |
| Patreon         | Unlimited     | N/A                 | Personal, exclusive         |
| Buy Me a Coffee | Unlimited     | N/A                 | Casual, grateful            |

## Decisions Made

- **Output format:** Single `social-posts.md` file (all platforms in one file)
- **Scope:** All 7 platforms in v1
- **Strategy:** 3-tier default, per-platform optional via flag
- **Voice config:** Reuse `brand-voice.md` (consistent with existing skills)
