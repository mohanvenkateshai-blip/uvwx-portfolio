# AGENTS.md

## Cursor Cloud specific instructions

This repository is a **static HTML/CSS/JS portfolio website** (deployed on Vercel). There is
**no build step, no bundler, no package manager, and no automated test suite**. The "application"
is the set of `.html` files served as-is from the repo root (see `README.md` for the file map).

### Running the site locally

There are no dependencies to install. Serve the repo root with any static file server, e.g.:

```
python3 -m http.server 8000
```

Then open `http://localhost:8000/`. Python 3 is preinstalled, so this needs no setup.

Notes / gotchas:
- A plain static server does **not** apply the `vercel.json` `redirects`/`rewrites`/`headers`.
  Extension-less clean URLs (e.g. `/about`, `/case-studies/diagnox`) and the `/jobs` and `/doctor`
  rewrites only work in Vercel's environment. Locally, request the real files with their
  `.html` extension (e.g. `/about.html`, `/case-studies/diagnox.html`).
- Highest-fidelity local preview is `vercel dev`, but the Vercel CLI is not installed and it
  requires interactive login/linking, so it is not usable in the autonomous cloud environment.
  Use the static server above instead.

### Core functionality to smoke-test

- Homepage `index.html`: work-row buttons open native `<dialog>` case-study modals (JS-driven);
  there is a scroll-progress bar and a contact form.
- The contact form uses **EmailJS** loaded from a CDN. Submitting sends a real email via a live
  service, so do not actually submit during testing — filling the fields is enough to verify.

### Lint / test / build

- **Build:** none (static files are deployed directly).
- **Tests:** none in the repo.
- **Lint:** no linter is configured.

### Not deployed

`source-files/`, `tools/`, `Gyan/`, and `PROGRESS.md` are excluded from deploys via `.vercelignore`.
