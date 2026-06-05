# Mohan Venkatesh Portfolio

Static portfolio website for `https://www.uvwx.me/`, hosted on Vercel.

## Structure

```
MohanPortfolio/
├── index.html                          ← live portfolio (single-page)
├── manifest.json                       ← PWA manifest
├── robots.txt                          ← crawler rules
├── sitemap.xml                         ← sitemap and image sitemap
├── vercel.json                         ← Vercel redirects/rewrites
├── google0da74ca02bdb86c2.html         ← Google Search Console verification
├── assets/
│   ├── favicon.svg                     ← site favicon/logo
│   └── images/profile/
│       └── mohan-venkatesh-ux-designer.jpg  ← hero + OG/schema image
├── source-files/                       ← NOT deployed (excluded by .vercelignore)
│   ├── case-studies/                   ← exported case study HTML, JSON, and gallery images
│   │   └── gallery/                    ← local copies of case study images (live site uses CDN)
│   ├── user-research/                  ← NDA usability research decks (PPTX)
│   └── supporting-docs/                ← private CV and recommendations (PDF)
└── tools/                              ← NOT deployed (excluded by .vercelignore)
    └── deploy-wizard.html              ← local deployment helper
```

## Notes

- Root-level files (`manifest.json`, `robots.txt`, `sitemap.xml`, `vercel.json`, `google*.html`) must remain at the web root — do not move them.
- `assets/` contains only files actively served by the live site.
- `source-files/` and `tools/` are excluded from Vercel deployments via `.vercelignore`.
- Case study media on the live site is served from `cdn.myportfolio.com`; `source-files/case-studies/gallery/` are local reference copies only.
