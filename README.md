# Marketing Skills Audit

A Claude Code plugin for comprehensive website auditing across **SEO**, **AEO** (Answer Engine Optimization) & **GEO** (Generative Engine Optimization).

Analyzes **60+ parameters** across 6 categories and generates an interactive HTML report with scores, findings, and actionable recommendations.

---

## Installation

```bash
/plugin install AshishChikane/marketing-skills-audit
```

## Usage

```bash
# Single page audit
/website-audit https://example.com

# Multi-page crawl (up to 10 pages)
/website-audit https://example.com --crawl
```

## What It Audits

| Category | Weight | Checks |
|----------|--------|--------|
| **Technical SEO** | 20% | Title, meta description, canonical, headings, links, HTTPS, Open Graph, Twitter Cards, hreflang, structured data, favicon (16 checks) |
| **GEO** | 20% | Content citability, E-E-A-T signals, author attribution, source citations, unique data, brand entity, topical depth (12 checks) |
| **Design & UX** | 15% | Viewport, responsive CSS, image optimization, navigation, CTAs, semantic layout (7 checks) |
| **Content Readability** | 15% | Content length, HTML ratio, paragraph structure, sentence length, keyword density, list usage (7 checks) |
| **Structured Data** | 15% | JSON-LD, schema types, FAQ/breadcrumb/article schema, semantic HTML, content outline (8 checks) |
| **AEO** | 15% | FAQ schema, question headings, direct answers, snippet readiness, voice search, how-to structure (8 checks) |

## Scoring

- **90-100**: Excellent
- **70-89**: Good
- **50-69**: Needs Work
- **0-49**: Critical

## Report Output

Generates a self-contained interactive HTML report with:
- Overall score gauge
- Category score cards with progress bars
- Detailed pass/fail/warning findings for every parameter
- Priority recommendations (High / Medium / Low)
- Collapsible sections for easy navigation
- Dark theme, responsive, print-friendly

Report is saved as `audit-report-{domain}-{date}.html` in your current directory.

## Project Structure

```
marketing-skills-audit/
├── .claude-plugin/
│   └── plugin.json              # Plugin manifest
├── skills/
│   └── website-audit/
│       ├── SKILL.md             # Audit skill
│       └── templates/
│           └── report-template.html
├── CLAUDE.md
└── README.md
```

## Author

**Ashish Chikane** — [@AshishChikane](https://github.com/AshishChikane)

## License

MIT
