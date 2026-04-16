# Marketing Skills — Website Audit

A Claude Code skill for comprehensive website auditing across SEO, AEO, and GEO.

## Skills

### `/website-audit`
Audits a website for 60+ parameters across 6 categories:
- **Technical SEO** (20%) — meta tags, headings, links, HTTPS, structured data
- **GEO** (20%) — citability, E-E-A-T, author info, topical depth
- **Design & UX** (15%) — responsive design, images, navigation, CTAs
- **Content Readability** (15%) — sentence length, content ratio, keyword density
- **Structured Data** (15%) — Schema.org, FAQ/Breadcrumb/Article schema
- **AEO** (15%) — FAQ schema, question headings, snippet readiness

### Usage
```
/website-audit https://example.com          # Single page audit
/website-audit https://example.com --crawl  # Multi-page crawl (up to 10 pages)
```

### Output
Generates an interactive HTML report saved to `audit-report-{domain}-{date}.html` in the current directory.

## Project Structure
```
marketing-skills/
├── CLAUDE.md                 # This file
├── website-audit.md          # Main skill file
└── templates/
    └── report-template.html  # HTML report template
```
