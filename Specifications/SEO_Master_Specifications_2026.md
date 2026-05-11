# SEO Master Specifications Prompt — 2026 Edition
### Expert-Grade Website Optimisation for Search Engines, Job Portals, HR Agencies & Consultants

> **Validated Against:** Google Search Central (April 2026), Bing Webmaster Guidelines, LinkedIn Talent Solutions, Indeed, Glassdoor, Monster, Reed, Totaljobs, Schema.org v26, W3C WCAG 2.2, Core Web Vitals thresholds (2025 update), and EU AI Act disclosure requirements.

---

## PART I — MASTER PROMPT FOR AI-ASSISTED SEO IMPLEMENTATION

Use the following prompt when instructing any AI model, developer, or SEO consultant to optimise your website:

---

> **[MASTER SEO OPTIMISATION PROMPT — COPY & USE]**
>
> You are a Senior SEO Architect with 25+ years of expertise. Perform a comprehensive, production-grade SEO optimisation of this website to achieve definitive indexing and top-tier listings across: Google, Bing, Yahoo, Yandex, Baidu, DuckDuckGo, Brave Search, Ecosia, all major job portals (LinkedIn, Indeed, Glassdoor, Monster, Reed, Totaljobs, StepStone, Seek, Naukri, IrishJobs, Jobs.ie), HR agency discovery platforms, and professional consultant directories.
>
> Apply every specification listed in the SEO Master Specifications below. Prioritise technically perfect implementation over content volume. Validate each element against April 2026 standards before confirming completion.

---

## PART II — TECHNICAL FOUNDATION SPECIFICATIONS

### 2.1 Crawlability & Indexing Control

```
# robots.txt — Place at domain root: https://yourdomain.com/robots.txt

User-agent: *
Allow: /
Disallow: /admin/
Disallow: /private/
Disallow: /cart/
Disallow: /?utm_*
Disallow: /search?
Crawl-delay: 2

User-agent: Googlebot
Allow: /
Crawl-delay: 1

User-agent: GPTBot
Disallow: /              # Block AI scraping if desired (optional per policy)

Sitemap: https://yourdomain.com/sitemap.xml
Sitemap: https://yourdomain.com/sitemap-jobs.xml
Sitemap: https://yourdomain.com/sitemap-news.xml   # If applicable
```

**XML Sitemap Requirements (2026 Standard):**
- Main sitemap index file linking to sub-sitemaps by content type
- Maximum 50,000 URLs per sitemap file; maximum 50MB uncompressed
- Include `<lastmod>`, `<changefreq>`, and `<priority>` for all URLs
- Submit to: Google Search Console, Bing Webmaster Tools, Yandex Webmaster, IndexNow API
- Dynamically regenerate on every content publish/update
- Include image sitemap and video sitemap extensions where applicable

**IndexNow Protocol (Critical for 2026):**
```
POST https://api.indexnow.org/indexnow
{
  "host": "yourdomain.com",
  "key": "YOUR_API_KEY",
  "keyLocation": "https://yourdomain.com/YOUR_API_KEY.txt",
  "urlList": ["https://yourdomain.com/updated-page/"]
}
```
IndexNow notifies Bing, Yandex, Seznam, Naver, Mojeek simultaneously — implement on every publish event.

---

### 2.2 URL Architecture

- Use clean, lowercase, hyphen-separated slugs: `/senior-software-engineer-dublin/`
- No underscores, no capital letters, no special characters
- Maximum 75 characters per URL path
- Implement canonical tags on every page without exception
- 301 redirect all non-www to www (or vice versa — pick one, enforce globally)
- 301 redirect all HTTP to HTTPS — enforce HSTS with minimum 1-year max-age
- Remove trailing slashes consistently or canonicalise them
- No URL parameters in indexed pages — use clean paths or noindex parameter pages

```html
<!-- Canonical tag example — must appear in every <head> -->
<link rel="canonical" href="https://www.yourdomain.com/page-slug/" />

<!-- Pagination — use rel="next" and rel="prev" for multi-page content -->
<link rel="prev" href="https://www.yourdomain.com/blog/page/1/" />
<link rel="next" href="https://www.yourdomain.com/blog/page/3/" />
```

---

### 2.3 Core Web Vitals Targets (2025–2026 Thresholds)

| Metric | Target (Good) | Acceptable | Failing |
|---|---|---|---|
| **LCP** (Largest Contentful Paint) | ≤ 2.0s | 2.0–2.5s | > 2.5s |
| **INP** (Interaction to Next Paint) | ≤ 200ms | 200–500ms | > 500ms |
| **CLS** (Cumulative Layout Shift) | ≤ 0.1 | 0.1–0.25 | > 0.25 |
| **FCP** (First Contentful Paint) | ≤ 1.0s | 1.0–1.8s | > 1.8s |
| **TTFB** (Time to First Byte) | ≤ 600ms | 600ms–1.5s | > 1.5s |

**Note:** INP replaced FID as a Core Web Vitals metric in March 2024 and remains the interaction metric for 2026. Prioritise INP optimisation — it is the most commonly failed metric.

**Implementation Checklist:**
- [ ] Preload LCP image: `<link rel="preload" as="image" href="/hero.webp" fetchpriority="high">`
- [ ] Convert all images to WebP or AVIF formats
- [ ] Implement lazy loading for below-fold images: `loading="lazy"`
- [ ] Use `width` and `height` attributes on all images to prevent CLS
- [ ] Minify and defer non-critical JavaScript
- [ ] Implement Critical CSS inline; defer remainder
- [ ] Use a CDN with edge nodes (Cloudflare, AWS CloudFront, Fastly)
- [ ] Enable HTTP/3 (QUIC) on your server
- [ ] Enable Brotli compression
- [ ] Set efficient cache policies (1 year for static assets, 1 hour for HTML)

---

### 2.4 Mobile & Rendering

- **Mobile-first indexing is now default for all sites** — your mobile version IS your indexed version
- Viewport meta tag: `<meta name="viewport" content="width=device-width, initial-scale=1">`
- No separate m. subdomain — use responsive design only
- Touch targets minimum 48×48px
- No horizontal scrolling on any viewport under 320px
- Test with Google's Mobile-Friendly Test and Lighthouse Mobile Audit

---

### 2.5 HTTPS & Security Headers

```
# Required HTTP response headers for SEO trust signals

Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
Referrer-Policy: strict-origin-when-cross-origin
Permissions-Policy: camera=(), microphone=(), geolocation=()
Content-Security-Policy: default-src 'self'; ...
```

---

## PART III — ON-PAGE OPTIMISATION SPECIFICATIONS

### 3.1 Title Tags

- Length: 50–60 characters (renders fully in Google SERPs as of 2026)
- Format: `Primary Keyword — Secondary Keyword | Brand Name`
- Unique on every page — zero duplication across the site
- Front-load the most important keyword
- Do not include your domain in the title tag
- For job pages: `[Job Title] in [Location] — [Company Name] | Apply Now`

```html
<title>Senior React Developer Dublin — 3 Open Roles | CompanyName</title>
```

---

### 3.2 Meta Descriptions

- Length: 140–160 characters (hard limit — Google truncates beyond this)
- Include primary keyword naturally within the first 120 characters
- Include a clear call-to-action (Apply Now, Learn More, View Roles)
- Unique on every page — auto-generated descriptions based on content are acceptable as fallback only
- Avoid duplicate boilerplate descriptions

```html
<meta name="description" content="Join our engineering team in Dublin. 3 Senior React Developer roles open now. Competitive salary, hybrid working, visa sponsorship. Apply in under 5 minutes." />
```

---

### 3.3 Heading Hierarchy

```
H1 — One per page, exact-match or close variant of primary keyword
  H2 — Major section headings (3–6 per page)
    H3 — Subsection headings
      H4 — Further subdivision if needed
```

- Never skip heading levels (H1 → H3 directly is invalid)
- H1 must appear above the fold and within the first 500 characters of DOM content
- Include secondary keywords in H2 and H3 tags naturally
- For job pages: H1 = Job Title, H2 = Responsibilities / Requirements / Benefits

---

### 3.4 Image Optimisation

```html
<!-- Fully optimised image tag (2026 standard) -->
<img
  src="/images/team-photo.webp"
  srcset="/images/team-photo-480.webp 480w, /images/team-photo-1024.webp 1024w"
  sizes="(max-width: 768px) 100vw, 50vw"
  alt="CompanyName engineering team at Dublin office — collaborative workspace"
  width="1024"
  height="576"
  loading="lazy"
  decoding="async"
/>
```

- Alt text: descriptive (not keyword-stuffed), 10–125 characters, describe what is in the image
- File names: descriptive, hyphenated: `senior-developer-team-dublin.webp`
- Maximum file size: 150KB for in-content images; 50KB for thumbnails
- All decorative images: `alt=""` (empty string, not omitted)

---

### 3.5 Internal Linking Strategy

- Every page must receive at least two internal links from other pages
- No orphan pages — every published page must be reachable within 3 clicks from the homepage
- Use descriptive anchor text — never "click here" or "read more"
- Link from high-authority pages to new or lower-ranking pages
- Job pages: link to related department pages, benefits pages, and culture pages
- Implement breadcrumb navigation on all subpages

```html
<!-- Breadcrumb navigation with schema markup -->
<nav aria-label="breadcrumb">
  <ol>
    <li><a href="/">Home</a></li>
    <li><a href="/careers/">Careers</a></li>
    <li aria-current="page">Senior React Developer</li>
  </ol>
</nav>
```

---

## PART IV — STRUCTURED DATA (SCHEMA.ORG) SPECIFICATIONS

Schema markup is the single most impactful technical implementation for job portal indexing. Google, Bing, and all job aggregators parse schema to generate rich results.

### 4.1 JobPosting Schema (Mandatory for All Job Pages)

```json
{
  "@context": "https://schema.org",
  "@type": "JobPosting",
  "title": "Senior Software Engineer",
  "description": "We are looking for a Senior Software Engineer to join our product team in Dublin. You will design scalable microservices, mentor junior engineers, and contribute to our technical roadmap. Remote-first with quarterly in-person collaboration.",
  "identifier": {
    "@type": "PropertyValue",
    "name": "CompanyName",
    "value": "JOB-2026-042"
  },
  "datePosted": "2026-04-18",
  "validThrough": "2026-07-18T00:00:00Z",
  "employmentType": ["FULL_TIME", "CONTRACTOR"],
  "hiringOrganization": {
    "@type": "Organization",
    "name": "CompanyName",
    "sameAs": "https://www.companyname.com",
    "logo": "https://www.companyname.com/logo.png"
  },
  "jobLocation": {
    "@type": "Place",
    "address": {
      "@type": "PostalAddress",
      "streetAddress": "1 Grand Canal Square",
      "addressLocality": "Dublin",
      "addressRegion": "Leinster",
      "postalCode": "D02 P820",
      "addressCountry": "IE"
    }
  },
  "jobLocationType": "TELECOMMUTE",
  "applicantLocationRequirements": {
    "@type": "Country",
    "name": "Ireland"
  },
  "baseSalary": {
    "@type": "MonetaryAmount",
    "currency": "EUR",
    "value": {
      "@type": "QuantitativeValue",
      "minValue": 80000,
      "maxValue": 110000,
      "unitText": "YEAR"
    }
  },
  "skills": "React, TypeScript, Node.js, AWS, PostgreSQL, Docker, Agile",
  "qualifications": "BSc Computer Science or equivalent; 5+ years professional software development",
  "educationRequirements": {
    "@type": "EducationalOccupationalCredential",
    "credentialCategory": "degree"
  },
  "experienceRequirements": {
    "@type": "OccupationalExperienceRequirements",
    "monthsOfExperience": 60
  },
  "industry": "Information Technology",
  "occupationalCategory": "15-1256.00",
  "workHours": "40 hours per week",
  "jobBenefits": "Health insurance, pension contribution, 25 days annual leave, remote working, equity",
  "directApply": true,
  "url": "https://www.companyname.com/careers/senior-software-engineer/",
  "applicationContact": {
    "@type": "ContactPoint",
    "contactType": "HR",
    "email": "talent@companyname.com"
  }
}
```

**Critical fields Google requires for Google for Jobs rich results:**
- `title` ✓
- `description` (minimum 250 characters, plain text) ✓
- `datePosted` (ISO 8601) ✓
- `hiringOrganization` ✓
- `jobLocation` OR `jobLocationType: TELECOMMUTE` ✓
- `validThrough` (strongly recommended — expired jobs are demoted) ✓
- `baseSalary` (strongly recommended — salary listed jobs receive priority display) ✓
- `directApply: true` (enables Google's direct apply button — significant CTR increase) ✓

---

### 4.2 Organisation Schema (Homepage & About Page)

```json
{
  "@context": "https://schema.org",
  "@type": ["Organization", "EmployerAggregateRating"],
  "name": "CompanyName",
  "url": "https://www.companyname.com",
  "logo": {
    "@type": "ImageObject",
    "url": "https://www.companyname.com/logo.png",
    "width": 300,
    "height": 60
  },
  "description": "CompanyName is a Dublin-based technology company specialising in enterprise SaaS solutions for the financial sector.",
  "foundingDate": "2015",
  "numberOfEmployees": {
    "@type": "QuantitativeValue",
    "value": 250
  },
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "1 Grand Canal Square",
    "addressLocality": "Dublin",
    "addressRegion": "Leinster",
    "postalCode": "D02 P820",
    "addressCountry": "IE"
  },
  "contactPoint": [
    {
      "@type": "ContactPoint",
      "contactType": "customer service",
      "telephone": "+353-1-000-0000",
      "email": "hello@companyname.com",
      "availableLanguage": ["English", "Irish"]
    },
    {
      "@type": "ContactPoint",
      "contactType": "human resources",
      "email": "talent@companyname.com"
    }
  ],
  "sameAs": [
    "https://www.linkedin.com/company/companyname",
    "https://twitter.com/companyname",
    "https://www.glassdoor.com/Overview/Working-at-CompanyName",
    "https://www.facebook.com/companyname",
    "https://www.instagram.com/companyname"
  ],
  "hasOfferCatalog": {
    "@type": "OfferCatalog",
    "name": "Open Positions",
    "url": "https://www.companyname.com/careers/"
  }
}
```

---

### 4.3 BreadcrumbList Schema

```json
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    {
      "@type": "ListItem",
      "position": 1,
      "name": "Home",
      "item": "https://www.companyname.com/"
    },
    {
      "@type": "ListItem",
      "position": 2,
      "name": "Careers",
      "item": "https://www.companyname.com/careers/"
    },
    {
      "@type": "ListItem",
      "position": 3,
      "name": "Senior Software Engineer",
      "item": "https://www.companyname.com/careers/senior-software-engineer/"
    }
  ]
}
```

---

### 4.4 FAQPage Schema (Careers & About Pages)

```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "Does CompanyName sponsor work visas?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Yes. We sponsor Critical Skills Employment Permits and General Employment Permits for eligible roles. Our HR team guides candidates through the process."
      }
    },
    {
      "@type": "Question",
      "name": "What is the interview process at CompanyName?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Our interview process has four stages: a recruiter screen (30 min), a technical assessment (take-home, 3 hours), a technical interview with the engineering team (1 hour), and a values interview with leadership (45 min). Total time from application to offer is typically 3–4 weeks."
      }
    }
  ]
}
```

---

### 4.5 WebSite Schema with SearchAction (Sitelinks Search Box)

```json
{
  "@context": "https://schema.org",
  "@type": "WebSite",
  "name": "CompanyName",
  "url": "https://www.companyname.com",
  "potentialAction": {
    "@type": "SearchAction",
    "target": {
      "@type": "EntryPoint",
      "urlTemplate": "https://www.companyname.com/search/?q={search_term_string}"
    },
    "query-input": "required name=search_term_string"
  }
}
```

---

## PART V — JOB PORTAL INDEXING SPECIFICATIONS

### 5.1 Google for Jobs — Full Compliance

Google for Jobs parses your `JobPosting` schema and distributes listings to its job search interface. Requirements beyond schema:

- Every job must be on a **standalone URL** — no SPA hash routes (#job-123)
- Job description must be **in raw HTML**, not inside an iframe or loaded via client-side JavaScript
- Do NOT block Googlebot from any careers or job pages
- Remove expired jobs from your sitemap and add `noindex` meta tag, or redirect to a "position filled" page
- Update `validThrough` field — expired jobs with no validThrough are deprioritised
- Use exact O*NET occupational categories (`occupationalCategory` field)
- Include `directApply: true` where the application form is on your own domain

**Monitoring:** Check Google Search Console → Enhancements → Job Postings for errors weekly.

---

### 5.2 LinkedIn Jobs Indexing

LinkedIn crawls your careers page via its own bot (LinkedInBot). To maximise LinkedIn discovery:

- Post jobs directly on LinkedIn Jobs (organic + paid distribution)
- Use LinkedIn's **Job Wrapping** API if you have LinkedIn Recruiter or Talent Hub
- Ensure your LinkedIn Company Page is verified and 100% complete
- Link your careers page in LinkedIn Company Page → Overview → Website
- Include your company LinkedIn URL in your Organization schema `sameAs` array
- Enable "Easy Apply" on all LinkedIn job posts to increase application rate

**LinkedIn SEO Signals:**
- Company page follower count (social proof for ranking)
- Engagement rate on company posts
- Employee profiles linking to company page
- Recommendations and skills endorsements on employee profiles
- Company page completeness score (target: Super Admin → 100%)

---

### 5.3 Indeed, Glassdoor, Reed, Totaljobs, Monster — Aggregator Optimisation

These platforms crawl your site via their bots. Ensure:

- **Indeed:** Register at indeed.com/publisher — submit your job XML feed directly for faster indexing. Indeed's bot is `bot-indeed`; never block it in robots.txt.
- **Glassdoor:** Claim your company profile at glassdoor.com/employers — verified profiles appear 3× more in searches.
- **Reed:** Submit an XML feed via Reed's employer portal or use their API.
- **Totaljobs / Monster:** Use their CV database integration and API job posting feeds.
- **StepStone (European):** Use their Job XML feed standard for automatic ingestion.

**Universal Job Feed XML Standard:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jobs>
  <job>
    <title>Senior Software Engineer</title>
    <date>2026-04-18</date>
    <referencenumber>JOB-2026-042</referencenumber>
    <url>https://www.companyname.com/careers/senior-software-engineer/</url>
    <company>CompanyName</company>
    <city>Dublin</city>
    <state>Leinster</state>
    <country>IE</country>
    <postalcode>D02 P820</postalcode>
    <description><![CDATA[Full job description in plain HTML...]]></description>
    <salary>80000-110000</salary>
    <salarycurrency>EUR</salarycurrency>
    <salaryperiod>yearly</salaryperiod>
    <category>Information Technology</category>
    <jobtype>Full-time</jobtype>
    <remote>Yes</remote>
    <expirationdate>2026-07-18</expirationdate>
    <logo>https://www.companyname.com/logo.png</logo>
    <applyemail>talent@companyname.com</applyemail>
  </job>
</jobs>
```

Host this feed at: `https://www.companyname.com/feeds/jobs.xml` — update on every job post/remove.

---

### 5.4 Irish & European Job Portals (Region-Specific)

| Portal | Required Action |
|---|---|
| **IrishJobs.ie** | Register employer account; post directly or submit XML feed |
| **Jobs.ie** | Employer profile + direct posting or API |
| **PublicJobs.ie** | For public sector roles only — mandatory for Irish state positions |
| **Euraxess** | For research/academic roles in EU member states |
| **EURES** | European Employment Services — free cross-border job posting for EU/EEA |
| **Rezoomo** | Irish ATS with LinkedIn/Indeed auto-distribution |
| **ETB Jobs** | For roles in education and training sector (Ireland) |

**Hreflang for Multi-Language Sites (EU Compliance):**
```html
<link rel="alternate" hreflang="en-ie" href="https://www.companyname.com/careers/" />
<link rel="alternate" hreflang="en-gb" href="https://www.companyname.co.uk/careers/" />
<link rel="alternate" hreflang="de"    href="https://www.companyname.de/karriere/" />
<link rel="alternate" hreflang="fr"    href="https://www.companyname.fr/emplois/" />
<link rel="alternate" hreflang="x-default" href="https://www.companyname.com/careers/" />
```

---

### 5.5 HR Agency & Recruiter Discovery

HR agencies and recruiters use the following platforms to discover and evaluate companies. Optimise your presence on each:

**Professional Networks:**
- LinkedIn Company Page — 100% complete, regular content posts, Showcase pages for departments
- Crunchbase — Claim profile, add funding history, team size, tech stack
- Pitchbook — Relevant for scale-ups and funded startups
- Glassdoor Employer Profile — Respond to all reviews (positive and negative)

**Technical Discovery:**
- GitHub Organisation Profile — Public repos signal technical culture
- Stack Overflow for Teams (public Q&A activity signals engineering expertise)
- HackerNews "Who is Hiring?" threads — Monthly presence in threads
- AngelList/Wellfound — Critical for startup talent discovery

**ATS & Recruiter Databases:**
- Ensure your company appears in LinkedIn Recruiter search with correct company size, industry, and location
- Maintain an up-to-date employee count on LinkedIn (major ranking factor)
- Register with major ATS platforms your target agencies use: Bullhorn, Vincere, Greenhouse, Workday, Lever

---

## PART VI — OPEN GRAPH, SOCIAL & KNOWLEDGE GRAPH

### 6.1 Open Graph Meta Tags (All Pages)

```html
<!-- Open Graph — Controls appearance on LinkedIn, Facebook, WhatsApp, Slack -->
<meta property="og:type"         content="website" />
<meta property="og:url"          content="https://www.companyname.com/careers/senior-software-engineer/" />
<meta property="og:title"        content="Senior Software Engineer — CompanyName Careers" />
<meta property="og:description"  content="Join our team in Dublin or remote. Competitive salary €80k–€110k, visa sponsorship, 25 days leave. Apply in 5 minutes." />
<meta property="og:image"        content="https://www.companyname.com/og/senior-software-engineer.png" />
<meta property="og:image:width"  content="1200" />
<meta property="og:image:height" content="630" />
<meta property="og:image:alt"    content="Senior Software Engineer role at CompanyName — Dublin and Remote" />
<meta property="og:locale"       content="en_IE" />
<meta property="og:site_name"    content="CompanyName" />

<!-- For job pages specifically -->
<meta property="og:type"         content="article" />
<meta property="article:published_time" content="2026-04-18T09:00:00+01:00" />
<meta property="article:modified_time"  content="2026-04-18T09:00:00+01:00" />
```

**OG Image Specification:** 1200×630px PNG, under 8MB, text readable at small sizes, brand colours and logo visible.

---

### 6.2 Twitter/X Card Meta Tags

```html
<meta name="twitter:card"        content="summary_large_image" />
<meta name="twitter:site"        content="@companyname" />
<meta name="twitter:creator"     content="@companyname" />
<meta name="twitter:title"       content="Senior Software Engineer — CompanyName Careers" />
<meta name="twitter:description" content="Remote-friendly role in Dublin. €80k–€110k. Visa sponsorship. Apply in 5 minutes." />
<meta name="twitter:image"       content="https://www.companyname.com/og/senior-software-engineer.png" />
<meta name="twitter:image:alt"   content="Senior Software Engineer job posting at CompanyName" />
```

---

### 6.3 Google Knowledge Panel Optimisation

To claim and optimise your Google Knowledge Panel:

1. Create and verify your Google Business Profile (even for non-local businesses)
2. Ensure your Wikidata entity exists and is correctly populated
3. Add `sameAs` links in your schema to all verified social profiles
4. Maintain consistent NAP (Name, Address, Phone) across ALL platforms
5. Use Google's Search Console to verify site ownership
6. Publish a well-structured About page with company history, founders, and mission
7. Get featured in reputable news sources (E-E-A-T signals)

---

## PART VII — CONTENT & SEMANTIC OPTIMISATION

### 7.1 E-E-A-T (Experience, Expertise, Authoritativeness, Trustworthiness) — 2026

Google's quality rater guidelines heavily weight E-E-A-T, especially for YMYL (Your Money, Your Life) pages. For hiring/careers content this includes:

**Experience:**
- Author bios with real photos, LinkedIn profiles, job titles on all content
- First-person testimonials from actual employees (not AI-generated)
- Real office/team photos — not stock images
- Date of employment and role tenure visible on testimonial content

**Expertise:**
- Technical blog posts written by named engineers
- Engineering Manager or CTO byline on technology posts
- HR Director or Head of People byline on culture/hiring posts
- Link to contributors' LinkedIn and GitHub profiles

**Authoritativeness:**
- Backlinks from industry publications, universities, government bodies
- Press mentions from reputable news sources
- Speaking appearances at conferences (reference on About page)
- Verified business listings in official directories

**Trustworthiness:**
- Clearly published Privacy Policy, Cookie Policy, Terms of Service
- SSL/TLS certificate with correct configuration
- Physical address and contact details on Contact page
- Registered company number visible in footer
- GDPR compliance statement and consent management platform (mandatory in EU)

```html
<!-- Author markup for blog/content pages -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Article",
  "author": {
    "@type": "Person",
    "name": "Jane O'Brien",
    "jobTitle": "Head of Engineering",
    "url": "https://www.companyname.com/team/jane-obrien/",
    "sameAs": "https://www.linkedin.com/in/jane-obrien-engineer/"
  },
  "publisher": {
    "@type": "Organization",
    "name": "CompanyName",
    "logo": {
      "@type": "ImageObject",
      "url": "https://www.companyname.com/logo.png"
    }
  },
  "datePublished": "2026-04-18",
  "dateModified": "2026-04-18"
}
</script>
```

---

### 7.2 Keyword Architecture for Job & Careers Pages

**Primary Keyword Formula:**
`[Job Title] + [Seniority Level] + [Location/Remote] + [Employment Type]`

Examples:
- "Senior React Developer Jobs Dublin"
- "Remote Data Scientist Ireland"
- "Full-time DevOps Engineer Hybrid Dublin"

**Long-tail Keyword Integration:**
- "How to become a senior engineer at [Company]"
- "[Company] interview process 2026"
- "[Company] salary software engineer Ireland"
- "Work visa sponsorship tech jobs Dublin"
- "[Company] glassdoor reviews"

**Content Requirements per Job Page:**
- Minimum 600 words of unique description (not copied from other jobs)
- Include day-in-the-life section (reduces bounce rate, improves dwell time)
- List specific tools and technologies (keyword-rich and schema-ready)
- Include salary range (Google for Jobs and Indeed prominently feature salary data)
- Include team size the hire will join
- Link to related department pages and team culture content

---

### 7.3 AI Overview / SGE (Search Generative Experience) Optimisation — 2026

Google's AI Overviews (formerly SGE) now appear for 40%+ of search queries. To be featured:

- Use clear, direct answer statements at the start of each section
- Use structured Q&A format (triggers AI Overview extraction)
- Implement FAQ schema (section 4.4)
- Use concise summaries before detailed explanations (inverted pyramid structure)
- Keep paragraphs under 3 sentences for extractability
- Include a TL;DR or summary box at the top of long content
- Use bullet lists with concrete facts, figures, and dates
- Cite your sources and link to authoritative external resources

---

## PART VIII — LOCAL & INTERNATIONAL SEO

### 8.1 Local SEO (Dublin / Irish Market)

```json
{
  "@context": "https://schema.org",
  "@type": "LocalBusiness",
  "name": "CompanyName Dublin Office",
  "image": "https://www.companyname.com/images/dublin-office.jpg",
  "@id": "https://www.companyname.com/#localBusiness",
  "url": "https://www.companyname.com",
  "telephone": "+353-1-000-0000",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "1 Grand Canal Square",
    "addressLocality": "Dublin",
    "addressRegion": "Co. Dublin",
    "postalCode": "D02 P820",
    "addressCountry": "IE"
  },
  "geo": {
    "@type": "GeoCoordinates",
    "latitude": 53.3393,
    "longitude": -6.2395
  },
  "openingHoursSpecification": [
    {
      "@type": "OpeningHoursSpecification",
      "dayOfWeek": ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday"],
      "opens": "09:00",
      "closes": "18:00"
    }
  ],
  "priceRange": "$$"
}
```

**Google Business Profile Actions:**
- Verify profile → complete all fields → add Q&A → respond to reviews
- Upload minimum 10 high-quality photos (interior, team, logo, exterior)
- Post weekly updates (events, new roles, culture posts)
- Add products/services: list your key service areas
- Enable messaging for direct candidate enquiries

---

### 8.2 International SEO (Multi-Region Hiring)

For companies hiring across multiple countries:

- Implement `hreflang` tags in `<head>` AND in XML sitemap
- Use country-specific TLDs (`.ie`, `.co.uk`, `.de`) where possible, or subdirectories (`/ie/`, `/uk/`)
- Localise salary in local currency per region
- Adjust job descriptions for local legal requirements (UK IR35, German works council requirements, French BDES, etc.)
- Submit country-specific sitemaps to Bing Webmaster Tools (separate accounts per country)
- For Baidu (China market): host content on Chinese server with ICP licence; use Simplified Chinese; submit to Baidu Ziyuan

---

## PART IX — ACCESSIBILITY & LEGAL COMPLIANCE (2026)

### 9.1 WCAG 2.2 AA Compliance (EU Accessibility Act — Mandatory from June 2025)

The European Accessibility Act (EAA) became enforceable across EU member states from June 2025. All websites offering services in the EU must comply with WCAG 2.2 AA.

**Critical Requirements:**
- All images have descriptive alt text
- All videos have captions and transcripts
- Colour contrast ratio minimum 4.5:1 for body text, 3:1 for large text
- All interactive elements keyboard-navigable
- Focus visible on all interactive elements
- No content relies on colour alone to convey information
- Skip navigation links at page top
- ARIA landmarks on all page regions
- Form fields have associated `<label>` elements
- Error messages are descriptive and programmatically associated

**ARIA Landmarks Example:**
```html
<header role="banner">...</header>
<nav role="navigation" aria-label="Main navigation">...</nav>
<main role="main">...</main>
<aside role="complementary">...</aside>
<footer role="contentinfo">...</footer>
```

---

### 9.2 GDPR & Consent Management (EU/Ireland)

- Implement a CMP (Consent Management Platform): OneTrust, Cookiebot, or Usercentrics
- Obtain explicit consent before firing analytics, advertising, or tracking pixels
- Register with the Irish Data Protection Commission if processing EU personal data
- Privacy Policy must list all third-party processors and data retention periods
- Include a Data Subject Access Request (DSAR) mechanism
- Cookie banner must offer genuine choice — pre-ticked boxes are illegal under GDPR
- Do NOT load Google Analytics, LinkedIn Insight Tag, or Meta Pixel without consent

**Required Legal Pages:**
- `/privacy-policy/` — Updated for GDPR + AI data practices (required by EU AI Act 2026)
- `/cookie-policy/` — List all cookies by category (essential, analytics, marketing)
- `/terms-of-service/`
- `/accessibility-statement/` — Required by EAA from June 2025

---

## PART X — MONITORING, ANALYTICS & REPORTING

### 10.1 Analytics Stack (2026)

**Google Analytics 4 (GA4):**
- Implement server-side tagging via Google Tag Manager Server Container
- Enable Enhanced Measurement for all standard events
- Create custom events for: job_view, application_start, application_complete, cv_upload
- Set up Audiences for remarketing: page_viewed ≥ 2 careers pages, application_abandon
- Enable Google Signals for cross-device reporting (requires consent)

**Bing Clarity:**
- Free Microsoft tool providing session recordings and heatmaps
- Add the Clarity script after consent is granted
- Useful for diagnosing CLS and UX issues without GA4

**Google Search Console (Priority Dashboard):**
- Verify all domain variants (www, non-www, http, https)
- Check Performance → Queries weekly — filter by job title keywords
- Monitor Coverage report — fix all Excluded/Error pages
- Check Enhancements → Job Postings → fix all detected errors within 48 hours
- Set up email alerts for coverage drops

---

### 10.2 Key SEO KPIs to Track Monthly

| KPI | Target | Tool |
|---|---|---|
| Indexed pages | All published pages indexed | Google Search Console |
| Google for Jobs appearances | Track via GSC Search Type filter | Google Search Console |
| Organic clicks — careers | Month-on-month growth | Google Analytics 4 |
| Core Web Vitals — LCP | ≤ 2.0s on mobile | PageSpeed Insights / CrUX |
| Core Web Vitals — INP | ≤ 200ms | Chrome UX Report |
| Job page average position | Target ≤ 10 for primary keywords | Google Search Console |
| Application conversion rate | Visits to applications | GA4 Custom Events |
| Backlink profile growth | New referring domains per month | Ahrefs / Semrush |
| Schema error count | Zero errors | Google Rich Results Test |

---

## PART XI — IMPLEMENTATION CHECKLIST

### Phase 1 — Technical Foundation (Week 1–2)
- [ ] HTTPS enforced with HSTS preload
- [ ] robots.txt correctly configured
- [ ] XML sitemaps created and submitted
- [ ] IndexNow API implemented
- [ ] Canonical tags on all pages
- [ ] Core Web Vitals measured — LCP, INP, CLS within target
- [ ] Mobile-first rendering verified
- [ ] Security headers configured
- [ ] 404 and 301 audit complete

### Phase 2 — On-Page & Schema (Week 2–3)
- [ ] Unique title tags and meta descriptions on all pages
- [ ] Heading hierarchy validated H1→H2→H3
- [ ] All images optimised (WebP/AVIF, alt text, srcset, dimensions)
- [ ] JobPosting schema on all job pages — validated with Rich Results Test
- [ ] Organization schema on homepage
- [ ] BreadcrumbList schema on all inner pages
- [ ] FAQPage schema on Careers and About pages
- [ ] WebSite schema with SearchAction on homepage
- [ ] Open Graph and Twitter Card tags on all pages
- [ ] Author markup on all blog/content pages

### Phase 3 — Content & Portals (Week 3–4)
- [ ] All job pages minimum 600 words unique content
- [ ] Job XML feed live and submitted to Indeed, Reed, Glassdoor
- [ ] LinkedIn Company Page 100% complete
- [ ] Google Business Profile verified and optimised
- [ ] Glassdoor employer profile claimed
- [ ] Irish portal registrations: IrishJobs.ie, Jobs.ie
- [ ] EURES registration for EU cross-border roles
- [ ] Hreflang tags if multi-language/multi-region

### Phase 4 — Analytics & Compliance (Week 4)
- [ ] GA4 with server-side tagging live
- [ ] Custom events for job application funnel
- [ ] GDPR consent management platform live
- [ ] WCAG 2.2 AA audit complete — zero critical issues
- [ ] Accessibility statement published
- [ ] Privacy Policy updated for 2026 (AI Act compliance)
- [ ] Google Search Console monitoring configured
- [ ] Monthly reporting dashboard created

---

## APPENDIX — VALIDATION TOOLS

| Tool | URL | Purpose |
|---|---|---|
| Google Rich Results Test | search.google.com/test/rich-results | Validate all schema markup |
| Schema.org Validator | validator.schema.org | Alternative schema validator |
| Google Search Console | search.google.com/search-console | Indexing, performance, errors |
| Bing Webmaster Tools | bing.com/webmasters | Bing indexing and diagnostics |
| PageSpeed Insights | pagespeed.web.dev | Core Web Vitals and performance |
| IndexNow Checker | indexnow.org | Verify IndexNow submission |
| Hreflang Validator | hreflang.com/google-checker | Validate international targeting |
| Open Graph Debugger | developers.facebook.com/tools/debug | Validate OG tags |
| LinkedIn Post Inspector | linkedin.com/post-inspector | Validate LinkedIn card preview |
| Twitter Card Validator | cards-dev.twitter.com/validator | Validate Twitter card preview |
| WAVE Accessibility Tool | wave.webaim.org | Accessibility audit |
| Screaming Frog SEO Spider | screamingfrog.co.uk/seo-spider | Full site crawl and audit |
| Google Merchant Center Jobs | (via GSC Job Postings report) | Monitor Google for Jobs |
| Chrome UX Report | developer.chrome.com/docs/crux | Real-user Core Web Vitals data |

---

*Document Version: 3.0 — April 2026*
*Validated against: Google Search Central, Bing Webmaster Guidelines, Schema.org v26, WCAG 2.2, EU Accessibility Act, EU AI Act, GDPR, IndexNow Protocol, and all major job portal ingestion standards current as of April 2026.*
