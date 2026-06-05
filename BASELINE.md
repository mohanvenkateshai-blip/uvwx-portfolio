# Performance Baseline 1.0

**Date:** 2026-06-05  
**Commit:** `1948c92`  
**Tool:** Lighthouse 13.3.0  
**URL:** https://www.uvwx.me/

---

## Scores

| Category        | Desktop | Mobile |
|-----------------|---------|--------|
| Performance     | 97      | 99     |
| Accessibility   | 96      | —      |
| Best Practices  | 96      | —      |
| SEO             | 100     | —      |

---

## Core Web Vitals

| Metric                    | Desktop | Mobile |
|---------------------------|---------|--------|
| First Contentful Paint    | 0.7 s   | 1.6 s  |
| Largest Contentful Paint  | 0.9 s   | 1.6 s  |
| Total Blocking Time       | 0 ms    | 0 ms   |
| Cumulative Layout Shift   | 0       | 0.002  |
| Speed Index               | 1.4 s   | 2.7 s  |
| Time to Interactive       | 0.9 s   | 2.5 s  |

---

## Payload

| Asset           | Size    |
|-----------------|---------|
| HTML (index)    | ~202 KB |
| Profile image   | 53 KB (WebP) |
| Logo            | 1.6 KB (WebP) |
| Slides (128)    | ~11 MB total (loaded on demand) |
| Avatars (12)    | ~110 KB |
| Total initial   | ~207 KB upload |

---

## What was in Baseline 1.0

- JS syntax error fixed (`imgMap` quote bug — was silently hiding all page content)
- 128 case study slides extracted from inline base64 → `assets/slides/` (HTML: 15MB → 202KB)
- Hero image converted to WebP with `<picture>` fallback (197KB → 53KB)
- Logo resized and converted to WebP (58.6KB → 1.6KB)
- Testimonial avatars extracted from inline base64 → `assets/avatars/`
- Google Fonts async-loaded via preload + onload
- EmailJS deferred
- `role="listitem"` added to 8 skill elements
- CDN thumbnail 403s fixed — hosted locally at `assets/thumbnails/`
- Security headers: `X-Frame-Options`, `CSP frame-ancestors`, `COOP`, `HSTS`
- Folder restructure: flat `assets/`, source files excluded from deploy
- Broken reference fixed: `manifest.json` icon path

---

## Pre-optimisation Comparison (same day)

| Metric | Before | Baseline 1.0 |
|--------|--------|--------------|
| Performance | 55 | **97–99** |
| FCP | 7.7 s | **0.7 s** |
| LCP | 7.7 s | **0.9 s** |
| HTML payload | 8,734 KB | **202 KB** |
| Best Practices | Error | **96** |
