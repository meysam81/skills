# Meysam's Skills

Curated [Agent Skills](https://github.com/anthropics/agent-skills) and plugins for [Claude Code](https://claude.com/claude-code) by [@meysam81](https://github.com/meysam81).

## Install

### Standalone Skills

Install a specific skill:

```bash
claude skills add meysam81/cc --skill google-golang-style
```

Install all standalone skills:

```bash
claude skills add meysam81/cc
```

### Plugins

Add as a marketplace, then install plugins individually:

```bash
/plugin marketplace add meysam81/cc
/plugin install seo-blog-engine@meysam81/cc
```

## Standalone Skills

> Opinionated, hand-maintained

Distilled from official documentation with practical focus on what matters day-to-day.

| Skill                                             | Description                                                            | Source                                                          |
| ------------------------------------------------- | ---------------------------------------------------------------------- | --------------------------------------------------------------- |
| [google-golang-style](skills/google-golang-style) | Go - naming, error handling, imports, code organization, documentation | [Google Go Style Guide](https://google.github.io/styleguide/go) |

## Plugins

### [seo-blog-engine](plugins/seo-blog-engine)

SEO blog post production pipeline — 6 skills for the full content lifecycle:

| Skill        | Command         | Purpose                                         |
| ------------ | --------------- | ----------------------------------------------- |
| seo-research | `/seo-research` | Generate deep research prompt for Claude.ai     |
| seo-keywords | `/seo-keywords` | Keyword research & SERP analysis via DataForSEO |
| seo-outline  | `/seo-outline`  | Content brief & detailed outline                |
| seo-write    | `/seo-write`    | Full blog post draft from outline               |
| seo-review   | `/seo-review`   | SEO audit with 100-point scoring rubric         |
| seo-optimize | `/seo-optimize` | Post-publish validation via DataForSEO          |

**Requires**: [DataForSEO MCP server](https://dataforseo.com/) for `/seo-keywords` and `/seo-optimize`.

See [plugin README](plugins/seo-blog-engine/README.md) for workflow details.

## License

[MIT](./LICENSE)

Skills are derived from their respective upstream documentation, which may have different licenses.
