# Mohan Venkatesh Portfolio

Static portfolio website for `https://www.uvwx.me/`, hosted on Vercel.

## Structure

```
MohanPortfolio/
├── index.html                          ← main portfolio homepage
├── about.html                          ← focused About page
├── ux-research.html                    ← focused UX research page
├── agentic-ux.html                     ← focused Agentic UX page
├── ai-ux-designer.html                 ← focused AI UX designer page
├── senior-ux-designer-ireland.html     ← focused Ireland senior UX page
├── case-studies/                       ← public indexable case-study pages
│   ├── diagnox.html
│   ├── banner-time-entry.html
│   └── my-finance-query.html
├── image-rights.html                   ← image ownership and licensing page
├── manifest.json                       ← PWA manifest
├── robots.txt                          ← crawler rules
├── sitemap.xml                         ← sitemap and image sitemap
├── vercel.json                         ← Vercel redirects/rewrites
├── google0da74ca02bdb86c2.html         ← Google Search Console verification
├── assets/
│   ├── favicon.svg                     ← site favicon/logo
│   ├── logo.png / logo.webp            ← header/footer logo
│   ├── mohan-venkatesh-ux-designer.jpg ← hero + OG/schema image
│   ├── seo-pages.css                   ← shared styles for focused pages
│   ├── slides/                         ← public research slide previews
│   └── thumbnails/                     ← public case-study thumbnails
├── source-files/                       ← NOT deployed (excluded by .vercelignore)
│   ├── case-studies/                   ← exported case study HTML, JSON, and gallery images
│   ├── user-research/                  ← NDA usability research decks (PPTX)
│   └── supporting-docs/                ← private CV and recommendations (PDF)
└── tools/                              ← NOT deployed (excluded by .vercelignore)
    └── deploy-wizard.html              ← local deployment helper
```

## Notes

- Root-level files (`manifest.json`, `robots.txt`, `sitemap.xml`, `vercel.json`, `google*.html`) must remain at the web root — do not move them.
- Public SEO pages should stay either at the web root or under `case-studies/` so their URLs remain stable.
- `assets/` contains only files actively served by the live site.
- `source-files/` and `tools/` are excluded from Vercel deployments via `.vercelignore`.
- Source material in `source-files/` is retained for reference and should not be linked from public pages unless deliberately publishing that asset.
