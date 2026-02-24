# On-Page SEO Checklist

## Title Tag

- 50-60 characters max
- Primary keyword front-loaded (first 3 words ideal)
- Unique per page — no duplicates across site
- Include brand name at end if space permits: "Topic | Brand"
- Power words for CTR: "Guide", "How to", "Best", year

## Meta Description

- 150-160 characters
- Include primary keyword naturally
- Compelling CTA or value proposition
- Unique per page — never duplicated
- Preview: test in SERP simulator before publishing

## URL Slug

- 3-5 words, hyphen-separated, lowercase
- No dates, no stop words, no special characters
- Include primary keyword
- Flat structure preferred: `/blog/topic-keyword/`
- Permanent — never change after indexing without 301 redirect

## Heading Hierarchy

- **H1**: One per page, contains primary keyword, distinct from title tag
- **H2**: Major sections, each maps to a secondary keyword
- **H3**: Subsections under H2, use LSI terms
- **H4-H6**: Rarely needed; use for deep technical content
- Never skip levels (H1 → H3 without H2)

## Content Body

- Primary keyword in first 100 words
- Primary keyword in conclusion paragraph
- Keyword density: 1-2% max (natural, not forced)
- Secondary keywords distributed across H2 sections
- LSI terms woven throughout body text
- Short paragraphs: 2-4 sentences max
- Scannable: bullets, bold key terms, numbered lists, tables
- Transition sentences between sections

## Internal Links

- 3-5 internal links per post minimum
- Descriptive anchor text (not "click here")
- Link at first natural mention of related topic
- Link to pillar/cornerstone content when relevant
- Avoid linking same page more than once per post
- Open in same tab (internal), new tab (external)

## External Links

- 1-2 links to authoritative sources per post
- Link to primary research, not aggregator articles
- Use rel="noopener" for new-tab links
- Verify links aren't broken before publishing
- Avoid linking to direct competitors

## Images

- WebP format preferred, JPEG fallback
- Compressed: < 100KB per image
- Descriptive alt text: keyword-relevant, not stuffed
- Meaningful file names: `dmarc-policy-diagram.webp` not `image1.webp`
- Width/height attributes set to prevent CLS
- Lazy load below-the-fold images
- At least 1 custom image per post (not stock)

## Schema Markup (JSON-LD)

- **Article**: Every blog post (headline, datePublished, author, image)
- **FAQPage**: Posts with FAQ section (question/answer pairs)
- **BreadcrumbList**: Site navigation path
- **HowTo**: Tutorial/guide posts (step-by-step)
- Validate with Google Rich Results Test before publishing
- Place in `<head>` or before closing `</body>`

## Mobile Optimization

- Responsive design — test at 320px, 768px, 1024px
- Touch targets: minimum 48x48px
- No horizontal scrolling
- Font size: minimum 16px body text
- Viewport meta tag set correctly

## Page Speed

- Target: LCP < 2.5s, INP < 200ms, CLS < 0.1
- Minimize render-blocking CSS/JS
- Preload critical fonts
- Use CDN for static assets
- Enable text compression (gzip/brotli)
