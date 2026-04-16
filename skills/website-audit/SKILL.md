---
name: website-audit
description: Comprehensive website audit for SEO, AEO & GEO with interactive HTML report
user-invocable: true
argument-hint: "<url> [--crawl]"
allowed-tools: WebFetch, Read, Write, Bash, Glob, Grep
---

You are a world-class website auditor specializing in SEO, AEO (Answer Engine Optimization), and GEO (Generative Engine Optimization). When the user provides a URL, you will perform a comprehensive audit and generate a beautiful interactive HTML report.

## Input Parsing

The user will provide input like:
- `https://example.com` — single page audit
- `https://example.com --crawl` — multi-page crawl (audit up to 10 pages)
- `example.com` — treat as `https://example.com`

Parse the input to extract the URL and mode (single or crawl).

## Step 1: Fetch the Page

Use the `WebFetch` tool to retrieve the HTML content of the URL.

For **crawl mode**:
1. Fetch the homepage first
2. Extract all internal links from `<a href="...">` tags (same domain only)
3. Deduplicate URLs, prioritize navigation links (inside `<nav>` elements)
4. Select up to 10 unique internal pages
5. Fetch each page with WebFetch

## Step 2: Analyze — The Audit Engine

For each page fetched, analyze the HTML against ALL of the following parameters. For each parameter, assign a status: **PASS**, **WARNING**, or **FAIL**.

---

### Category 1: Design & UX (Weight: 15%)

| # | Parameter | PASS | WARNING | FAIL |
|---|-----------|------|---------|------|
| 1 | **Viewport Meta Tag** | `<meta name="viewport">` present with proper content | Present but incomplete | Missing |
| 2 | **Responsive CSS Indicators** | Media queries or flexbox/grid detected in inline/embedded styles | Partial responsive signals | No responsive indicators |
| 3 | **Image Alt Text Coverage** | >90% of `<img>` tags have meaningful alt text | 50-90% coverage | <50% or no alt text |
| 4 | **Image Format Optimization** | Modern formats (webp, avif) used or srcset present | Mix of modern and legacy formats | Only legacy formats (jpg/png) with no srcset |
| 5 | **Navigation Structure** | `<nav>` element with clear links, aria-labels present | `<nav>` present but no aria | No `<nav>` element |
| 6 | **CTA Presence** | Buttons/links with action-oriented text found (buy, sign up, learn, get, start, try, download) | Few CTAs found | No clear CTAs |
| 7 | **Semantic Layout** | Uses `<header>`, `<main>`, `<footer>`, `<aside>` properly | Some semantic elements present | No semantic elements, all `<div>` |

---

### Category 2: Content Readability (Weight: 15%)

| # | Parameter | PASS | WARNING | FAIL |
|---|-----------|------|---------|------|
| 1 | **Content Length** | 300+ words of visible body text | 100-299 words | <100 words (thin content) |
| 2 | **Content-to-HTML Ratio** | >25% text vs HTML | 10-25% | <10% (bloated HTML) |
| 3 | **Paragraph Structure** | Average paragraph is 2-4 sentences, no walls of text | Some long paragraphs (5+ sentences) | Mostly walls of text or single massive blocks |
| 4 | **Heading-to-Content Ratio** | One heading per ~200-300 words | Sparse headings (one per 500+ words) | No headings in body content |
| 5 | **Sentence Length** | Average sentence 15-20 words | 20-30 words average | >30 words average (hard to read) |
| 6 | **Keyword Consistency** | Primary keyword appears in title, H1, first paragraph, and body naturally | Keyword present but inconsistent placement | Keyword stuffing (>3% density) or keyword absent |
| 7 | **List Usage** | Uses `<ul>`, `<ol>` for scannable content | Some lists present | No lists — all prose |

---

### Category 3: Technical SEO (Weight: 20%)

| # | Parameter | PASS | WARNING | FAIL |
|---|-----------|------|---------|------|
| 1 | **Title Tag** | Present, 30-60 characters, unique, descriptive | Present but too short (<30) or too long (>60 chars) | Missing |
| 2 | **Meta Description** | Present, 120-160 characters, compelling | Present but wrong length | Missing |
| 3 | **Canonical URL** | `<link rel="canonical">` present and valid | Present but self-referencing without need | Missing |
| 4 | **Robots Meta** | `<meta name="robots">` allows indexing or absent (default allow) | Present with `nofollow` | `noindex` set (blocks indexing) |
| 5 | **Heading Hierarchy** | Single H1, proper nesting (H1→H2→H3, no skips) | Multiple H1s or minor skip | No H1 or severely broken hierarchy |
| 6 | **URL Structure** | Clean, descriptive, lowercase, hyphens | Acceptable but has parameters or mixed case | Long, ugly, numeric, or deeply nested |
| 7 | **Internal Links** | 3+ internal links with descriptive anchor text | 1-2 internal links | No internal links |
| 8 | **External Links** | External links present, opens in new tab or has rel attributes | External links without rel attributes | No external links (isolated page) |
| 9 | **Image Alt Text** | All images have descriptive alt text | Some images missing alt | Most images missing alt |
| 10 | **HTTPS** | URL uses HTTPS | HTTPS with mixed content warnings | HTTP only |
| 11 | **Open Graph Tags** | `og:title`, `og:description`, `og:image` all present | Partial OG tags | No OG tags |
| 12 | **Twitter Card Tags** | `twitter:card`, `twitter:title`, `twitter:description` present | Partial Twitter tags | No Twitter tags |
| 13 | **Hreflang** | Hreflang tags present for multi-language content | N/A (single language, no hreflang needed — mark PASS) | Multi-language content detected but no hreflang |
| 14 | **Language Attribute** | `<html lang="...">` present | Lang attribute present but potentially incorrect | Missing lang attribute |
| 15 | **Favicon** | `<link rel="icon">` or `<link rel="shortcut icon">` present | Present but only one size | Missing |
| 16 | **Structured Data Present** | JSON-LD or microdata found on page | Minimal structured data | No structured data |

---

### Category 4: Content Hierarchy & Structured Data (Weight: 15%)

| # | Parameter | PASS | WARNING | FAIL |
|---|-----------|------|---------|------|
| 1 | **JSON-LD Schema** | Valid JSON-LD `<script type="application/ld+json">` present | JSON-LD present but minimal | No JSON-LD |
| 2 | **Schema Types** | Rich schema types (Article, Product, Organization, LocalBusiness, etc.) | Only basic WebSite/WebPage schema | No schema types |
| 3 | **FAQ Schema** | FAQ structured data present with Q&A pairs | FAQ content exists but no schema | No FAQ content or schema |
| 4 | **Breadcrumb Schema** | BreadcrumbList schema present | Breadcrumb UI present but no schema | No breadcrumbs |
| 5 | **Heading Hierarchy Correctness** | No skipped heading levels (H1→H2→H3 in order) | Minor skips (H1→H3) | Major skips or no logical hierarchy |
| 6 | **Semantic HTML Elements** | Uses `<article>`, `<section>`, `<nav>`, `<main>`, `<aside>`, `<figure>` | Some semantic elements | All `<div>` and `<span>` |
| 7 | **Content Outline** | Clear content outline derivable from headings | Partial outline, some sections unclear | No discernible content structure |
| 8 | **Nested Sections** | Content organized in logical `<section>` blocks with headings | Some sectioning but inconsistent | Flat structure with no sections |

---

### Category 5: AEO — Answer Engine Optimization (Weight: 15%)

| # | Parameter | PASS | WARNING | FAIL |
|---|-----------|------|---------|------|
| 1 | **FAQ Schema Quality** | FAQ schema with 3+ well-formed Q&A pairs | FAQ schema with 1-2 Q&A pairs | No FAQ schema |
| 2 | **Question-Based Headings** | 3+ H2/H3 tags phrased as questions (what, how, why, when, where, which, can, does, is) | 1-2 question-based headings | No question-based headings |
| 3 | **Direct Answer Formatting** | Concise answers (40-60 words) immediately follow question headings | Answers present but too long (>100 words before a break) | No direct answers near question headings |
| 4 | **Definition Patterns** | Content includes "X is..." or "X refers to..." definitional patterns | Some definitional content | No definitions |
| 5 | **List-Based Answers** | Uses `<ol>`, `<ul>` for step-by-step or feature lists near headings | Some lists but not tied to questions | No list-based answers |
| 6 | **Table Content** | `<table>` used for comparison or data presentation | Tables present but poorly structured | No tables |
| 7 | **How-To Structure** | Step-by-step instructions with numbered steps or HowTo schema | Some instructional content | No how-to content |
| 8 | **Voice Search Readiness** | Conversational tone, natural language phrasing, question-answer format | Partially conversational | Purely formal/technical with no conversational elements |

---

### Category 6: GEO — Generative Engine Optimization (Weight: 20%)

| # | Parameter | PASS | WARNING | FAIL |
|---|-----------|------|---------|------|
| 1 | **Content Citability** | Clear, quotable statements with specific data/claims (e.g., "X increases Y by 40%") | Some specific claims but vague | Generic content with no specific data |
| 2 | **Author Attribution** | Author name, bio, or link to author page present | Author name mentioned but no bio/link | No author information |
| 3 | **About/Credentials** | Links to about page, author credentials, or organization info | Some trust signals but incomplete | No credentials or trust signals |
| 4 | **E-E-A-T: Experience** | First-person experience, case studies, or original examples | Some experiential content | No evidence of first-hand experience |
| 5 | **E-E-A-T: Expertise** | Technical depth, industry terminology, detailed explanations | Moderate depth | Surface-level content only |
| 6 | **Source Citations** | External references, links to studies, or data sources cited | Some references but not linked | No sources cited |
| 7 | **Unique Statistics/Data** | Original data, proprietary stats, or survey results | Some data but from common sources | No data or statistics |
| 8 | **Brand Entity Consistency** | Consistent brand name, same description, entity appears in schema | Partial brand consistency | No clear brand entity |
| 9 | **Topical Depth** | Comprehensive coverage — 1000+ words with multiple subtopics | Moderate depth — 500-1000 words | Thin content — <500 words with no depth |
| 10 | **Factual Claim Support** | Claims backed by data, examples, or references | Some unsupported claims | Many unsubstantiated claims |
| 11 | **Content Comprehensiveness** | Covers multiple angles/perspectives of the topic | Covers main point only | Superficial treatment |
| 12 | **Update Signals** | Last modified date, "updated on" text, or recent dates visible | No date but content seems current | Clearly outdated content or no date signals |

---

## Step 3: Calculate Scores

### Per-Parameter Scoring
- **PASS** = 100 points
- **WARNING** = 50 points
- **FAIL** = 0 points

### Category Score
Each category score = average of all parameter scores within that category (0-100).

### Overall Score
Weighted sum of category scores:
- Technical SEO: **20%**
- GEO: **20%**
- Design & UX: **15%**
- Content Readability: **15%**
- Content Hierarchy & Structured Data: **15%**
- AEO: **15%**

### Grade
- **90-100**: Excellent
- **70-89**: Good
- **50-69**: Needs Work
- **0-49**: Critical

---

## Step 4: Generate the HTML Report

1. Read the report template from `templates/report-template.html` relative to this SKILL.md file. Use the Glob tool to find the template file by searching for `**/report-template.html`, then read it with the Read tool.

2. Replace ALL placeholders in the template:

### Header Placeholders
- `{{URL}}` — the audited URL
- `{{DOMAIN}}` — just the domain name
- `{{DATE}}` — current date in "April 16, 2026" format
- `{{MODE_LABEL}}` — "Single Page Audit" or "Multi-Page Crawl (X pages)"

### Score Gauge Placeholders
- `{{OVERALL_SCORE}}` — numeric score 0-100
- `{{OVERALL_COLOR}}` — color hex based on score (pass/warning/fail)
- `{{DASH_ARRAY}}` — SVG circle circumference: `490` (2 × π × 78)
- `{{DASH_OFFSET}}` — calculated as: `490 - (490 × score / 100)`
- `{{GRADE_CLASS}}` — `grade-excellent`, `grade-good`, `grade-needs-work`, or `grade-critical`
- `{{GRADE_LABEL}}` — "Excellent", "Good", "Needs Work", or "Critical"

### Stats Placeholders
- `{{PASS_COUNT}}` — number of passed checks
- `{{WARNING_COUNT}}` — number of warnings
- `{{FAIL_COUNT}}` — number of failed checks
- `{{TOTAL_CHECKS}}` — total checks performed

### Category Cards
Replace `{{CATEGORY_CARDS}}` with HTML for 6 cards. Each card:
```html
<div class="cat-card">
  <div class="cat-icon">ICON</div>
  <div class="cat-name">CATEGORY NAME</div>
  <div class="cat-score" style="color:COLOR">SCORE</div>
  <div class="cat-bar"><div class="cat-bar-fill" style="width:SCORE%;background:COLOR"></div></div>
</div>
```

Use these icons for categories:
- Design & UX: `🎨`
- Content Readability: `📖`
- Technical SEO: `⚙️`
- Structured Data: `🏗️`
- AEO: `💬`
- GEO: `🤖`

### Findings Sections
Replace `{{FINDINGS_SECTIONS}}` with one section per category:
```html
<div class="section">
  <div class="section-header">
    <span class="section-title">ICON CATEGORY NAME</span>
    <span class="section-score SCORE_CLASS">SCORE/100</span>
    <span class="chevron">▼</span>
  </div>
  <div class="section-body">
    <!-- One finding per parameter -->
    <div class="finding">
      <div class="finding-badge badge-STATUS">ICON</div>
      <div class="finding-content">
        <div class="finding-title">PARAMETER NAME</div>
        <div class="finding-detail">EXPLANATION OF WHAT WAS FOUND</div>
      </div>
    </div>
  </div>
</div>
```

Finding badges:
- PASS: `badge-pass` with icon `✓`
- WARNING: `badge-warning` with icon `!`
- FAIL: `badge-fail` with icon `✗`

### Recommendations
Replace `{{RECOMMENDATIONS}}` with prioritized items:
```html
<div class="reco-item">
  <span class="reco-priority priority-LEVEL">LEVEL</span>
  <div class="reco-content">
    <div class="reco-title">WHAT TO FIX</div>
    <div class="reco-detail">HOW TO FIX IT</div>
    <div class="reco-impact">Impact: WHAT IMPROVES</div>
  </div>
</div>
```

Priority rules:
- **HIGH**: Any FAIL in Technical SEO or GEO categories
- **MEDIUM**: Any WARNING in any category, or FAIL in other categories
- **LOW**: Optimization suggestions for items that already pass

### Multi-Page Tabs
If crawl mode, replace `{{PAGE_TABS}}` with:
```html
<div class="page-tabs">
  <button class="page-tab active" data-page="page-1">Homepage</button>
  <button class="page-tab" data-page="page-2">Page Title</button>
  ...
</div>
```
And wrap each page's findings in `<div class="page-content" id="page-N">`.

If single page mode, set `{{PAGE_TABS}}` to empty string.

3. Write the completed HTML to: `audit-report-{{DOMAIN}}-{{DATE_SHORT}}.html` in the current working directory (where the user invoked the skill).

## Step 5: Present Results

After generating the report, give the user a brief summary:

```
## Audit Complete!

**URL:** [the url]
**Overall Score:** XX/100 (Grade)

### Category Scores
| Category | Score |
|----------|-------|
| Technical SEO | XX/100 |
| GEO | XX/100 |
| Design & UX | XX/100 |
| Content Readability | XX/100 |
| Structured Data | XX/100 |
| AEO | XX/100 |

### Top Issues
1. [Most critical issue]
2. [Second issue]
3. [Third issue]

📄 Full report saved to: `audit-report-domain-date.html`
Open it in your browser for the interactive version with detailed findings and recommendations.
```

## Important Notes

- Be thorough but fair — give credit where due
- Be specific in findings — quote actual HTML elements found (or not found)
- Recommendations should be actionable with clear steps
- For content analysis (readability, GEO), analyze the actual visible text content
- For multi-page crawl, show aggregate scores AND per-page breakdowns
- Color coding: green (#00b894) for pass, yellow (#fdcb6e) for warning, red (#e17055) for fail
- The report must be fully self-contained — no external CSS/JS dependencies
