# Schema Markup Templates (JSON-LD)

All templates use JSON-LD format placed in `<head>` or before `</body>`. Replace placeholder values (`{{...}}`) with actual content.

## Article Schema

Every blog post should include this.

```json
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "{{Title — 110 chars max for Google}}",
  "description": "{{Meta description — 150-160 chars}}",
  "image": "{{Featured image URL — min 1200x630px}}",
  "datePublished": "{{YYYY-MM-DD}}",
  "dateModified": "{{YYYY-MM-DD}}",
  "author": {
    "@type": "Person",
    "name": "{{Author Name}}",
    "url": "{{Author profile URL}}",
    "jobTitle": "{{Author Title}}",
    "sameAs": ["{{LinkedIn URL}}", "{{Twitter URL}}"]
  },
  "publisher": {
    "@type": "Organization",
    "name": "{{Company Name}}",
    "logo": {
      "@type": "ImageObject",
      "url": "{{Logo URL — min 112x112px}}"
    }
  },
  "mainEntityOfPage": {
    "@type": "WebPage",
    "@id": "{{Canonical URL of this page}}"
  },
  "wordCount": "{{Word count as integer}}",
  "keywords": "{{primary keyword, secondary keyword 1, secondary keyword 2}}"
}
```

**Required fields**: headline, datePublished, author, publisher, image
**Recommended fields**: dateModified, wordCount, keywords, description

## FAQPage Schema

Add when the post includes a FAQ section with 2+ Q&A pairs.

```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "{{Question text — exactly as shown in H3/heading}}",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "{{Answer text — plain text or basic HTML, 40-60 words optimal for featured snippets}}"
      }
    },
    {
      "@type": "Question",
      "name": "{{Question 2}}",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "{{Answer 2}}"
      }
    }
  ]
}
```

**Tips**:

- Match question text exactly to the heading in content
- Keep answers concise (40-60 words) for featured snippet eligibility
- 3-7 questions per FAQ section is optimal
- Can be combined with Article schema on the same page

## HowTo Schema

Add for tutorial/guide posts with clear sequential steps.

```json
{
  "@context": "https://schema.org",
  "@type": "HowTo",
  "name": "{{How to title — matches H1}}",
  "description": "{{Brief description of what this guide achieves}}",
  "totalTime": "PT{{N}}M",
  "estimatedCost": {
    "@type": "MonetaryAmount",
    "currency": "USD",
    "value": "{{0 for free, or cost}}"
  },
  "supply": [
    {
      "@type": "HowToSupply",
      "name": "{{Required tool/resource 1}}"
    }
  ],
  "tool": [
    {
      "@type": "HowToTool",
      "name": "{{Required tool 1}}"
    }
  ],
  "step": [
    {
      "@type": "HowToStep",
      "name": "{{Step 1 title}}",
      "text": "{{Step 1 detailed instructions}}",
      "image": "{{Step 1 screenshot URL (optional)}}",
      "url": "{{Page URL}}#step-1"
    },
    {
      "@type": "HowToStep",
      "name": "{{Step 2 title}}",
      "text": "{{Step 2 detailed instructions}}",
      "url": "{{Page URL}}#step-2"
    }
  ]
}
```

**Tips**:

- Include `totalTime` in ISO 8601 duration (PT30M = 30 minutes)
- Each step should have a clear, actionable name
- Images per step significantly increase rich result eligibility
- Add URL anchors for each step (#step-1, #step-2)

## BreadcrumbList Schema

Add to every page for navigation context.

```json
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    {
      "@type": "ListItem",
      "position": 1,
      "name": "Home",
      "item": "{{https://example.com/}}"
    },
    {
      "@type": "ListItem",
      "position": 2,
      "name": "Blog",
      "item": "{{https://example.com/blog/}}"
    },
    {
      "@type": "ListItem",
      "position": 3,
      "name": "{{Post Title}}",
      "item": "{{https://example.com/blog/post-slug/}}"
    }
  ]
}
```

**Tips**:

- Always start with Home
- Match visible breadcrumb navigation on the page
- Last item can omit `item` property (it's the current page)
- Position must be sequential integers starting at 1

## Combining Schemas

Multiple schemas on one page: wrap in an array.

```json
<script type="application/ld+json">
[
  { "@context": "https://schema.org", "@type": "Article", ... },
  { "@context": "https://schema.org", "@type": "FAQPage", ... },
  { "@context": "https://schema.org", "@type": "BreadcrumbList", ... }
]
</script>
```

## Validation

Before publishing, validate all schema markup:

1. **Google Rich Results Test**: https://search.google.com/test/rich-results
2. **Schema.org Validator**: https://validator.schema.org/
3. Check for warnings, not just errors — warnings often indicate missed opportunities
