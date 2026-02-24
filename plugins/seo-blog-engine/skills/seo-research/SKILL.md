---
name: seo-research
description: Generate a deep research prompt for Claude.ai to gather facts, stats, expert quotes, and competitor analysis for a blog post topic
model: sonnet
---

# SEO Deep Research Prompt Generator

Generate a ready-to-paste prompt for Claude.ai's deep research mode that collects verified facts, statistics, expert quotes, case studies, and competitor content analysis for a blog post topic.

## Inputs

1. **Topic or keyword** — provided by user in their message
2. **Brand voice** — read from `docs/seo/brand-voice.md` (if exists)

## Process

### Step 1 — Gather Context

Read `docs/seo/brand-voice.md` if it exists. Extract:

- Industry/niche
- Target audience (ICPs)
- Product context
- Key terminology
- Competitors

If the file doesn't exist, ask the user for: industry, target audience, and 2-3 competitor names.

### Step 2 — Generate Deep Research Prompt

Create a structured prompt optimized for Claude.ai's deep research mode. The prompt MUST include these sections:

**Anti-hallucination instructions** (place at the top of the generated prompt):

```
CRITICAL INSTRUCTIONS:
- Cite ONLY verifiable, published sources with URLs
- For every statistic, include: the exact number, the source name, the publication year, and URL
- If you cannot find a verifiable source for a claim, explicitly state "No verifiable source found"
- Prefer primary sources (original research, official reports) over secondary sources (blog posts citing others)
- Include a confidence level (High/Medium/Low) for each fact based on source quality
- Flag any data older than 2 years as potentially outdated
```

**Research sections to request** (adapt based on topic):

1. **Industry statistics & market data**
   - Market size, growth rates, adoption percentages
   - Recent surveys and reports (last 2 years)
   - Trend data with year-over-year changes

2. **Expert quotes & authoritative opinions**
   - Named experts with credentials
   - Quotes from conference talks, interviews, published articles
   - Contrasting viewpoints where they exist

3. **Case studies & real-world examples**
   - Companies/organizations that implemented the topic
   - Quantified results (before/after metrics)
   - Implementation timelines and challenges

4. **Regulatory & compliance data** (if applicable)
   - Relevant standards, RFCs, regulations
   - Compliance requirements and deadlines
   - Penalties or consequences of non-compliance

5. **Competitor content analysis**
   - What do the top 5 ranking articles for this keyword cover?
   - What angles do they take?
   - What's missing from their coverage? (content gaps)

6. **People Also Ask questions**
   - Common questions people search for around this topic
   - Questions from Reddit, Stack Overflow, forums
   - Emerging questions (new concerns, recent developments)

7. **Counterarguments & nuance**
   - Common misconceptions about the topic
   - Legitimate criticisms or limitations
   - Edge cases and exceptions

### Step 3 — Output the Prompt

Write the generated prompt to the user's conversation with clear instructions:

```
## How to Use This Prompt

1. Open Claude.ai (https://claude.ai)
2. Start a new conversation
3. Click the "Deep Research" button (or use the research mode toggle)
4. Paste the prompt below
5. Wait for the research to complete (typically 3-5 minutes)
6. Save the output to `docs/seo/research-brief.md` in your project
```

Then output the full prompt text, ready to copy-paste.

## Output Format

The generated prompt should produce research organized as:

```markdown
# Research Brief: {{Topic}}

## Key Statistics

- [Stat] — Source: [Name] ([Year]) [URL] — Confidence: High/Medium/Low

## Expert Perspectives

- "[Quote]" — [Name], [Title] at [Org] — Source: [URL]

## Case Studies

### [Company/Org Name]

- Context: ...
- Implementation: ...
- Results: ... (quantified)
- Source: [URL]

## Regulatory Landscape

- [Standard/Regulation]: [Key requirement] — Source: [URL]

## Content Gap Analysis

- Top articles cover: ...
- Missing from existing coverage: ...

## People Also Ask

1. [Question]?
2. [Question]?

## Counterarguments & Nuance

- [Misconception]: Actually, ... — Source: [URL]
```

## Quality Checks

Before outputting, verify the prompt:

- [ ] Anti-hallucination instructions are at the top
- [ ] All 7 research sections are included (adapt if some don't apply)
- [ ] Brand context is woven in (industry terms, audience focus)
- [ ] Output format is specified in the prompt
- [ ] Source attribution format is defined
- [ ] Recency requirements are stated (prefer last 2 years)
