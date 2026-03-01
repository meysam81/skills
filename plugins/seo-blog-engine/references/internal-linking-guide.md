# Internal Linking Guide

## North Star: Readability

Every internal link must answer YES to: "Does following this link genuinely help the reader right now?"

A link is warranted when:

- The linked page explains a concept the current page mentions but doesn't cover in depth
- The reader would naturally want to learn more about the linked topic at this point in their reading
- The linked content adds context, evidence, or practical detail that supports the current argument

A link is NOT warranted when:

- The topic is only tangentially related (same industry ≠ same topic)
- The link would interrupt the reader's flow or train of thought
- The link exists solely because both pages share a keyword
- The anchor text feels forced or unnatural in the sentence

## Anchor Text

| Do                                        | Don't                                         |
| ----------------------------------------- | --------------------------------------------- |
| Descriptive phrases: "DMARC policy types" | Generic: "click here", "read more", "this"    |
| Natural sentence flow                     | Keyword-stuffed: "best DMARC tools 2026 free" |
| Vary anchor text across links to same URL | Identical anchor text on every mention        |
| Match the target page's actual topic      | Misleading anchor promising different content |

## Link Density Guidelines

| Content Length  | Suggested Range | Notes                                       |
| --------------- | --------------- | ------------------------------------------- |
| < 800 words     | 1-3 links       | Short content — don't overwhelm             |
| 800-1500 words  | 3-5 links       | Standard blog post                          |
| 1500-3000 words | 5-8 links       | Comprehensive guide                         |
| 3000+ words     | 7-12 links      | Pillar content — more linking opportunities |

These are guidelines, not rules. A page with 2000 words might warrant 3 links or 10, depending on how many genuinely relevant topics it touches.

## Link Placement

- **First natural mention**: link a topic the first time it appears in context, not every mention
- **Same page once**: never link the same target URL more than once per page
- **Spread across sections**: avoid clustering all links in one paragraph or section
- **Introduction**: one link to set context (pillar or related overview)
- **Body sections**: links where subtopics connect to dedicated pages
- **Conclusion**: one link for next-step or related deep-dive

## Topical Cluster Integration

When `topical-clusters.md` exists, prioritize links within the cluster:

1. **Pillar ↔ Supporting**: supporting posts link up to pillar; pillar links down to supporting posts
2. **Sibling links**: supporting posts in the same pillar link to each other when contextually relevant
3. **Cross-pillar links**: only when there's genuine topical overlap (e.g., a DMARC post linking to an email deliverability post in another pillar)

When no topical cluster file exists, infer relationships from:

- Shared primary/secondary keywords between pages
- Overlapping H2 topics
- Same audience intent (both TOFU, both MOFU, etc.)

## Orphan Page Detection

An orphan page has **zero incoming internal links** from other pages on the site.

**Why it matters**: Search engines discover pages through links. An orphan page may never get crawled, indexed, or receive any link equity from the rest of the site.

**Action**: Every content page should have at least 1 incoming internal link. Orphans should be linked from the most contextually relevant page.

## Semantic Relevance Scoring

When evaluating whether a link is warranted, consider:

| Factor                     | Weight | Description                                               |
| -------------------------- | ------ | --------------------------------------------------------- |
| Keyword overlap            | Medium | Shared primary/secondary keywords between source & target |
| Topic continuity           | High   | Target page continues or deepens the source's narrative   |
| Search intent alignment    | High   | Both pages serve the same or adjacent user intent         |
| Reader journey             | High   | Link represents a logical next step for the reader        |
| Same topical cluster       | Medium | Both pages belong to the same pillar                      |
| Content depth relationship | Medium | One page is an overview, the other a deep-dive            |

**Rule**: A link should score High on at least one factor and not score Low on any High-weight factor.

## Common Anti-Patterns

| Anti-Pattern              | Problem                                                 |
| ------------------------- | ------------------------------------------------------- |
| Link stuffing             | 15+ links in a 1000-word post; overwhelms reader        |
| Forced anchors            | Rewriting sentences just to fit a link                  |
| Footer/sidebar link farms | Mass links with no contextual relevance                 |
| Orphaned pillar pages     | Pillar exists but no supporting posts link to it        |
| Circular-only linking     | A links to B, B links to A, nothing else                |
| Outdated target pages     | Linking to content that's stale, deprecated, or removed |
