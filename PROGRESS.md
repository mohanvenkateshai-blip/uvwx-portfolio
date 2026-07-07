# PROGRESS.md — uvwx.me Portfolio

> Living progress log per **MAFIP** (Multi-Agent Feature Implementation Protocol).
> Protocol source: `/Users/ganesha/Projects/Specifications/MAFIP_Multi_Agent_Feature_Implementation_Guide.md`
> This file is git-tracked but excluded from Vercel deploys.

---

## Feature: v2 Redesign — "Ruthless Curation" homepage rebuild

**Status:** Phase 7 — Deployment Prep · ~95% complete
**Started:** 2026-07-07 · **Owner:** Moni (HITL) + Claude Code (execution)

### Origin
- Candid assessment (`Gyan/portfolio-assessment-and-master-prompt.html`, Jul 4 2026): v1 site scored D+ on IA / D on 60-second scannability despite A− credentials. Verdict: "an exhaustive archive when it needs to be a persuasive argument."
- Approved design prototype: `Gyan/uvwx-redesign-v2.html` (user: "the v2 design is awesome").

### Phase log

| Phase | Gate | Outcome |
|---|---|---|
| 1. Discovery | ✅ | Assessment + master prompt already written (Jul 4). User approved v2 direction (Jul 7). HITL decisions: qualitative outcomes → upgraded to real CV metrics; CV = `Gyan/Mohan_Venkatesh_Core.docx`; keep EmailJS form. |
| 2. Design | ✅ | v2 prototype accepted as-is; brand tokens retained (#070F1A / #5B9EFF / #C9913A, Jakarta + Instrument Serif + Plex Mono). |
| 3. Implementation | ✅ | New `index.html` (v2 design + SEO head + JSON-LD + EmailJS form + real metrics), `colophon.html`, CV PDF (`assets/mohan-venkatesh-cv.pdf`), sitemap rewrite (fixed malformed XML), vercel.json (clean URLs, `/cv`, `/colophon`, security headers). |
| 4. Verification | ✅ | Playwright render checks: desktop hero/full-page, mobile 390px (nav collision found → fixed), dialog open test. Zero page errors. All local links verified. JSON/XML validated. |
| 5. Review & alignment | ✅ | Reviewed against `Specifications/UX_UI_Web_Standards_2026.md`, `SEO_Master_Specifications_2026.md`, `Web_Design_UI_UX_Guidelines.md`. Fixes: h3-in-button semantics, footer internal links to 5 SEO pages, security headers (nosniff / referrer / permissions policy). |
| 6. Documentation | ✅ | This file; colophon page documents the build publicly. |
| 7. Deploy & close | ⏳ | Commit as v2.0, push to main (auto-deploys via Vercel), verify live at https://www.uvwx.me/. |

### Key decisions
- **Real metrics over placeholders** — pulled from Moni's own CV: 28 UX issues closed pre-GA, 72% heuristic baseline (200 guidelines · 9 categories · 5 experts), 35% baseline task-failure quantified, 100% completion across 15 validation tasks with Telstra/Optus, 12 GA improvements.
- **Dialogs + full pages** — homepage work rows open native `<dialog>` summaries that link to the standalone `case-studies/*.html` pages (SEO + depth preserved).
- **Old site preserved** — tag `v1.1` = complete final v1 state; `v1.0` = Baseline 1.0 benchmark; file copies in `source-files/backup/v1/`. New design tagged `v2.0` on release.
- **No anti-devtools code** in v2 (per assessment P1 fix).

### Blockers
- None.

### Next steps
- [ ] Commit + tag `v2.0` + push + verify production.
- [ ] Post-launch: PageSpeed/Lighthouse audit on live URL; IndexNow ping for updated sitemap.
- [ ] Future (backlog): restyle the 5 SEO pages + case-study pages to match v2 tokens exactly (`#091524` → `#070F1A`); add a real product capture to the flagship section if NDA allows; refresh `about.html` copy to match new positioning.

---

*Convention going forward: every new feature starts with "Apply MAFIP to implement [feature]" and gets logged here with phase gates.*
