# UX/UI & Web Standards Specifications
### Production-Grade Reference — April 2026

> **Scope:** Authoritative, non-redundant specifications for building modern web applications and portals. Every entry is currently valid, actively enforced, or directly supported by a shipping browser as of April 2026. Deprecated patterns, experimental proposals, and theoretical guidelines have been excluded.

---

## 1. HTML — SEMANTICS & STRUCTURE

### 1.1 Document Baseline

```html
<!DOCTYPE html>
<html lang="en" dir="ltr" data-theme="light">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <meta name="color-scheme" content="light dark" />
  <title>Page Title — Site Name</title>
  <link rel="canonical" href="https://domain.com/page/" />
</head>
```

- `lang` on `<html>` is mandatory — required by WCAG 2.2, screen readers, and search engines.
- `color-scheme` meta tag tells the browser to render native UI elements (scrollbars, inputs) in the user's preferred scheme before CSS loads — prevents flash of wrong-theme chrome.
- `data-theme` on `<html>` is the standard hook for JS-driven theme switching.

---

### 1.2 Landmark Regions — Use Every Time

```html
<header>           <!-- Site header — one per page -->
<nav>              <!-- Navigation — label with aria-label when multiple navs exist -->
<main>             <!-- Primary content — one per page, no exceptions -->
<aside>            <!-- Supplementary content -->
<section>          <!-- Thematic grouping — must have an accessible heading -->
<article>          <!-- Self-contained content (post, card, job listing) -->
<footer>           <!-- Page or section footer -->
```

**Rules that are commonly broken:**
- `<section>` without a heading is a code smell — either add an `<h2>` or use `<div>`.
- `<main>` must NOT be nested inside `<article>`, `<aside>`, `<header>`, or `<footer>`.
- `<nav>` must be labelled when more than one exists: `<nav aria-label="Breadcrumb">`.
- `<button>` for actions, `<a>` for navigation — never swap these roles.
- `<div>` and `<span>` carry no semantic meaning — use them for layout and styling only.

---

### 1.3 Native HTML Elements to Use (Not Build Custom)

These elements are browser-native, accessible out of the box, and keyboard-functional without JavaScript:

| Element | Use For | Common Mistake |
|---|---|---|
| `<dialog>` | Modals, drawers, confirmation dialogs | Building a `<div>` modal with ARIA roles |
| `<details>` / `<summary>` | Accordions, disclosure widgets | Custom JS accordion for simple collapse |
| `<select>` | Dropdowns where UX allows native styling | Replacing with custom JS dropdowns |
| `<input type="date">` | Date pickers on forms | Building custom date pickers from scratch |
| `<progress>` | Progress bars | Custom `<div>` with ARIA role=progressbar |
| `<meter>` | Scalar value within a range (score, usage) | Custom visualisations |
| Popover API | Tooltips, dropdowns, floating menus | Custom JS-positioned floating layers |

**Popover API (Baseline 2024 — Universally Supported):**
```html
<button popovertarget="menu">Open Menu</button>
<div id="menu" popover>Menu content here</div>
```
Zero JavaScript required. Browser handles focus trapping, Escape to close, and accessibility.

---

### 1.4 Form Best Practices

```html
<!-- Every input needs an explicit label — never use placeholder as a substitute -->
<div class="field">
  <label for="email">Email address <span aria-hidden="true">*</span></label>
  <input
    type="email"
    id="email"
    name="email"
    autocomplete="email"
    aria-required="true"
    aria-describedby="email-hint email-error"
    inputmode="email"
  />
  <p id="email-hint" class="hint">We will never share your email.</p>
  <p id="email-error" role="alert" hidden>Enter a valid email address.</p>
</div>
```

- `autocomplete` attributes reduce user effort and are required for WCAG 1.3.5 (Identify Input Purpose).
- `inputmode` optimises the mobile keyboard without changing input semantics.
- `aria-describedby` links hints and errors programmatically — screen readers announce them with the field.
- Group related fields with `<fieldset>` and `<legend>` — essential for radio groups and checkboxes.
- Use `<button type="submit">` not `<input type="submit">` — the former accepts HTML children for icon buttons.

---

## 2. CSS — MODERN STANDARDS

### 2.1 Architecture: CSS Custom Properties + Cascade Layers

```css
/* Layer order declared at the top of your main stylesheet */
@layer reset, tokens, base, components, utilities, overrides;

/* Design tokens — single source of truth */
@layer tokens {
  :root {
    /* Colour */
    --colour-brand-500: #2563eb;
    --colour-brand-600: #1d4ed8;
    --colour-surface:   #ffffff;
    --colour-on-surface: #111827;

    /* Typography */
    --font-sans: "Geist", system-ui, sans-serif;
    --font-mono: "Geist Mono", ui-monospace, monospace;
    --text-base: 1rem;
    --line-height-body: 1.6;

    /* Spacing scale (4px base unit) */
    --space-1: 0.25rem;  /* 4px  */
    --space-2: 0.5rem;   /* 8px  */
    --space-3: 0.75rem;  /* 12px */
    --space-4: 1rem;     /* 16px */
    --space-6: 1.5rem;   /* 24px */
    --space-8: 2rem;     /* 32px */
    --space-12: 3rem;    /* 48px */
    --space-16: 4rem;    /* 64px */

    /* Radii */
    --radius-sm: 4px;
    --radius-md: 8px;
    --radius-lg: 16px;
    --radius-full: 9999px;

    /* Motion */
    --duration-fast:   100ms;
    --duration-base:   200ms;
    --duration-slow:   400ms;
    --ease-out:        cubic-bezier(0.16, 1, 0.3, 1);
    --ease-spring:     cubic-bezier(0.34, 1.56, 0.64, 1);
  }

  @media (prefers-color-scheme: dark) {
    :root {
      --colour-surface:    #0f172a;
      --colour-on-surface: #f1f5f9;
    }
  }
}
```

**Why Cascade Layers matter:** They solve specificity wars without `!important`. Third-party overrides, utility classes, and component styles no longer fight each other — the layer order determines priority, not selector weight.

---

### 2.2 Layout: Grid & Flexbox Patterns That Are Production-Standard

```css
/* Intrinsic responsive grid — no media queries, no breakpoints needed */
.auto-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(min(280px, 100%), 1fr));
  gap: var(--space-6);
}

/* Holy grail layout — header, scrollable content, fixed sidebar */
.app-shell {
  display: grid;
  grid-template:
    "header header" auto
    "sidebar main"  1fr
    / 260px 1fr;
  height: 100dvh; /* dvh = dynamic viewport height — correct on mobile browsers */
}

/* Centred container with fluid padding */
.container {
  width: min(var(--max-width, 1280px), 100% - var(--space-8));
  margin-inline: auto;
}

/* Stack — consistent vertical spacing without margin collapsing issues */
.stack > * + * { margin-block-start: var(--space-6); }
```

**Units that are now standard:**
- `dvh`, `dvw`, `dvmin`, `dvmax` — Dynamic viewport units. Use instead of `vh`/`vw` for mobile-correct sizing (accounts for browser chrome showing/hiding).
- `svh` — Small viewport height — always includes mobile browser UI. Safe for above-the-fold constraints.
- `cqw`, `cqi` — Container query units — width relative to a containment context.
- `lh` — Line-height unit. `padding: 1lh` = one line of text worth of space.

---

### 2.3 Container Queries (Ship in Production Now)

```css
/* Declare containment on the parent */
.card-grid {
  container-type: inline-size;
  container-name: card-grid;
}

/* Component responds to its own container, not the viewport */
.card {
  display: grid;
  grid-template-columns: 1fr;
}

@container card-grid (min-width: 480px) {
  .card {
    grid-template-columns: auto 1fr;
  }
}
```

Container queries are Baseline 2023 — all major browsers support them. They replace viewport-based media queries for component-level responsiveness. A sidebar at 300px wide and a main content card at 900px can style their children independently.

---

### 2.4 Selectors Worth Using Now

```css
/* :has() — the "parent selector" — Baseline 2023 */
/* Style a form group that contains an invalid input */
.field:has(input:invalid:not(:focus)) { border-color: var(--colour-error); }

/* Style a card that contains an image */
.card:has(img) { grid-template-columns: 120px 1fr; }

/* :is() — group selectors without specificity stacking */
:is(h1, h2, h3) { font-weight: 500; line-height: 1.2; }

/* :where() — zero specificity grouping */
:where(button, [role="button"]) { cursor: pointer; }

/* CSS Nesting — Baseline 2024 — no preprocessor needed */
.nav {
  display: flex;
  gap: var(--space-4);

  & a {
    color: var(--colour-on-surface);
    text-decoration: none;

    &:hover, &:focus-visible { text-decoration: underline; }
    &[aria-current="page"]   { font-weight: 500; }
  }
}
```

---

### 2.5 Transitions & Animation — Standards

```css
/* Respect user motion preferences — mandatory */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}

/* View Transitions API — for page and element transitions */
/* Baseline 2024 in same-document context; cross-document in Chromium */
@view-transition { navigation: auto; }

.card {
  view-transition-name: card-1; /* Must be unique per visible element */
}

/* Animate only composited properties (GPU-accelerated) */
/* GOOD: transform, opacity, filter */
/* BAD: width, height, top, left, margin, padding (cause layout) */
.panel {
  transition: transform var(--duration-base) var(--ease-out),
              opacity  var(--duration-base) var(--ease-out);
}
```

---

### 2.6 Typography — Current Standards

```css
body {
  font-family: var(--font-sans);
  font-size: var(--text-base);          /* 16px — never go below for body text */
  line-height: var(--line-height-body); /* 1.5–1.7 for body, 1.1–1.3 for headings */
  font-synthesis: none;                 /* Prevent faux bold/italic */
  text-rendering: optimizeLegibility;
  -webkit-font-smoothing: antialiased;  /* macOS only — optional */
}

/* Fluid type scale using clamp() — no breakpoints for typography */
h1 { font-size: clamp(1.75rem, 4vw + 1rem, 3.5rem); }
h2 { font-size: clamp(1.375rem, 2.5vw + 0.75rem, 2.25rem); }
h3 { font-size: clamp(1.125rem, 1.5vw + 0.5rem, 1.5rem); }

/* Measure (line length) — 60–80 characters for body text */
.prose { max-inline-size: 65ch; }

/* Balanced headlines — prevent awkward one-word last lines */
h1, h2, h3 { text-wrap: balance; }

/* Prevent orphans in body copy */
p { text-wrap: pretty; } /* Supported in Chrome/Edge 117+, Firefox 121+ */
```

---

### 2.7 Colour — Contrast & Modern Colour Spaces

```css
/* Use modern colour spaces for wider gamut displays */
:root {
  --colour-brand: oklch(55% 0.2 264);      /* OKLCH — perceptually uniform */
  --colour-brand-light: oklch(90% 0.08 264);
}

/* Colour contrast — minimum requirements */
/* WCAG 2.2 AA: body text 4.5:1, large text 3:1, UI components 3:1 */
/* WCAG 2.2 AAA: body text 7:1, large text 4.5:1 */

/* Check contrast using CSS relative colour syntax */
.text-muted {
  /* Derived from brand colour — maintains relative relationship */
  color: oklch(from var(--colour-brand) calc(l + 30%) c h);
}

/* Forced colours mode — Windows High Contrast */
@media (forced-colors: active) {
  .custom-checkbox { border: 2px solid ButtonText; }
}
```

**What OKLCH solves:** RGB and HSL produce inconsistent perceived brightness across hues (purple at 50% lightness appears darker than yellow at 50%). OKLCH is perceptually uniform — same `L` value = same perceived brightness, regardless of hue. Baseline 2024.

---

## 3. JAVASCRIPT & COMPONENT STANDARDS

### 3.1 Progressive Enhancement Principle

Build in layers:
1. **HTML** — Works with zero CSS and zero JavaScript (content is accessible, forms submit)
2. **CSS** — Adds visual design and layout
3. **JavaScript** — Enhances interactivity for supported environments

Never build core functionality that requires JavaScript unless the use case genuinely demands it (e.g. real-time collaboration, complex data visualisation). Navigation, forms, and content display must degrade gracefully.

---

### 3.2 Modern JavaScript — Use These Now

```javascript
// Declarative array methods over imperative loops
const activeJobs = jobs.filter(j => j.active).map(j => j.title);

// Nullish coalescing and optional chaining
const city = user?.address?.city ?? "Unknown";

// Structured clone for deep copying (replaces JSON.parse/stringify)
const copy = structuredClone(originalObject);

// Using top-level await in modules
const data = await fetch("/api/jobs").then(r => r.json());

// AbortController for cancellable fetch
const controller = new AbortController();
const response   = await fetch("/api/search", { signal: controller.signal });
// controller.abort() — cancel on component unmount or new request

// Intersection Observer for lazy loading and scroll effects
const observer = new IntersectionObserver(
  entries => entries.forEach(e => e.isIntersecting && loadContent(e.target)),
  { rootMargin: "200px" }
);

// ResizeObserver for component-level dimension tracking
const ro = new ResizeObserver(entries => {
  const { width } = entries[0].contentRect;
  element.dataset.size = width < 400 ? "compact" : "default";
});
```

---

### 3.3 Web Components (When Appropriate)

Use native Web Components when building framework-agnostic design system primitives:

```javascript
class AppToast extends HTMLElement {
  static observedAttributes = ["type", "message"];

  connectedCallback() {
    this.setAttribute("role", "status");
    this.setAttribute("aria-live", "polite");
    this.render();
  }

  attributeChangedCallback() { this.render(); }

  render() {
    this.innerHTML = `<p>${this.getAttribute("message") ?? ""}</p>`;
  }
}

customElements.define("app-toast", AppToast);
```

**When to use Web Components:**
- Design system tokens and primitives shared across multiple frameworks
- Embeddable widgets for third-party consumption
- Long-lived applications where framework lock-in is a risk

**When not to use:** Within a single-framework application — framework components (React, Vue, Svelte) offer better DX, SSR support, and ecosystem integration.

---

### 3.4 State Management Principles

- Co-locate state as close to the component that uses it as possible
- Lift state only when sibling components genuinely need to share it
- Use URL state (query params) for shareable UI state (filters, pagination, tabs)
- Use `localStorage` / `sessionStorage` for user preferences only — not application data
- Do not store sensitive data in client-side storage — ever

---

## 4. DESIGN SYSTEM & COMPONENT STANDARDS

### 4.1 Design Token Hierarchy

```
Global Tokens     → Raw values: #2563eb, 16px, 400ms
  Semantic Tokens → Purposeful aliases: --colour-action, --colour-danger, --space-component-gap
    Component Tokens → Scoped: --button-bg, --button-text, --button-radius
```

Global tokens are never used directly in components. Semantic tokens reference global tokens. Component tokens reference semantic tokens. This three-tier system allows global rebranding (swap one global token) and semantic retheming (dark mode changes semantic tokens, not component tokens).

---

### 4.2 Component API Conventions

Every production component must define:

| Property | Rule |
|---|---|
| **size** | Use t-shirt sizing: `xs sm md lg xl` — not pixel values |
| **variant** | Describe intent: `primary secondary ghost danger` — not visual descriptors like `blue outlined` |
| **state** | Boolean props: `disabled loading error` |
| **accessible name** | Every interactive component must accept `aria-label` or `aria-labelledby` |
| **polymorphic rendering** | Components that look like links must optionally render as `<a>` — use `as` prop pattern |

---

### 4.3 Button Component — Complete Accessible Implementation

```html
<!-- Default button -->
<button type="button" class="btn btn--primary">Save changes</button>

<!-- Loading state — aria-live region announces completion -->
<button type="button" class="btn btn--primary" aria-disabled="true" aria-describedby="save-status">
  <svg aria-hidden="true" class="spinner" ...></svg>
  Saving…
</button>
<p id="save-status" role="status" aria-live="polite" class="sr-only"></p>

<!-- Icon-only button — visible label for screen readers -->
<button type="button" class="btn btn--icon" aria-label="Delete item">
  <svg aria-hidden="true" focusable="false">...</svg>
</button>
```

**Rules:**
- Use `aria-disabled="true"` (not `disabled`) when the button must remain focusable (e.g. to show a tooltip explaining why it's disabled).
- Use `disabled` (native) when the field is genuinely unavailable and no explanation is needed.
- Never put a `<div>` or `<a>` with `onclick` where a `<button>` belongs.

---

### 4.4 Focus Management

```css
/* Remove default and replace with a custom focus ring — never just remove it */
:focus-visible {
  outline: 2px solid var(--colour-brand-500);
  outline-offset: 3px;
  border-radius: var(--radius-sm);
}

/* Remove focus ring for mouse/touch users (it only shows for keyboard) */
:focus:not(:focus-visible) { outline: none; }
```

**Focus management rules:**
- After opening a modal/dialog: move focus to the first focusable element inside it.
- After closing a modal: return focus to the trigger element.
- After routing to a new page (SPA): move focus to `<main>` or the new page's `<h1>`.
- Never use `tabindex` values greater than 0 — use 0 (focusable) or -1 (programmatically focusable only).
- Skip link (`<a href="#main">Skip to main content</a>`) must be the first focusable element on every page.

---

### 4.5 Responsive Design: Breakpoint System

```css
/*
  Breakpoints (content-based, not device-based)
  Use as container query or viewport query depending on context
*/
:root {
  --bp-sm: 480px;   /* Small devices */
  --bp-md: 768px;   /* Medium — typically where sidebar patterns emerge */
  --bp-lg: 1024px;  /* Large — desktop-first layouts */
  --bp-xl: 1280px;  /* Wide — data tables, dashboards */
  --bp-2xl: 1536px; /* Ultra-wide — max-width containers */
}

/*
  Mobile-first: write base styles for mobile, override upward.
  Never write desktop-first CSS and override down — it creates
  specificity debt and forces overriding already-applied properties.
*/
.nav-links {
  display: none; /* Hidden on mobile */
}
@media (min-width: 768px) {
  .nav-links { display: flex; } /* Visible on tablet+ */
}
```

---

## 5. ACCESSIBILITY (WCAG 2.2 AA — Current Legal Standard)

> **WCAG 2.2 is the current enforceable standard** (published October 2023). The EU Accessibility Act mandates WCAG 2.2 AA compliance across EU member states from June 2025, including Ireland. WCAG 3.0 remains a Working Draft — do not build to it yet.

### 5.1 New in WCAG 2.2 (vs 2.1) — These Are Now Required

| Criterion | Requirement | Level |
|---|---|---|
| **2.4.11 Focus Not Obscured (Minimum)** | Focused component must not be entirely hidden by sticky headers/footers | AA |
| **2.4.12 Focus Not Obscured (Enhanced)** | Focused component must be fully visible | AAA |
| **2.4.13 Focus Appearance** | Focus indicator must have 2px minimum thickness and 3:1 contrast ratio against adjacent colours | AA |
| **2.5.3 Target Size (Minimum)** | Touch targets minimum 24×24px CSS; if smaller, must have 24px spacing from other targets | AA |
| **2.5.7 Dragging Movements** | All drag actions must have a single-pointer alternative (e.g. drag-to-reorder + up/down buttons) | AA |
| **2.5.8 Target Size (Enhanced)** | Touch targets minimum 44×44px CSS (strongly recommended) | AAA |
| **3.2.6 Consistent Help** | Help mechanisms in same location across pages | A |
| **3.3.7 Redundant Entry** | Don't ask users to re-enter information they already provided in the same session | A |
| **3.3.8 Accessible Authentication (Minimum)** | No cognitive function test (CAPTCHA) without an alternative | AA |

**Note:** 4.1.1 (Parsing) was removed in WCAG 2.2 as browsers handle malformed HTML better and validators became less relevant to AT.

---

### 5.2 ARIA — Rules of Use

**Rule 1:** Use native HTML before ARIA. `<button>` is better than `<div role="button">`.

**Rule 2:** Never change native semantics unless you have no other option.

**Rule 3:** All interactive ARIA widgets must be keyboard operable.

**Rule 4:** Don't use `role="presentation"` or `aria-hidden="true"` on focusable elements.

**Rule 5:** Interactive elements must have accessible names.

```html
<!-- Live regions for dynamic content -->
<p role="status" aria-live="polite">3 results found.</p>   <!-- Non-urgent -->
<p role="alert"  aria-live="assertive">Error: Session expired.</p> <!-- Urgent -->

<!-- Table with complex headers -->
<table>
  <caption>Q1 2026 Applications by Region</caption>
  <thead>
    <tr>
      <th scope="col">Region</th>
      <th scope="col">Applications</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Ireland</th>
      <td>1,482</td>
    </tr>
  </tbody>
</table>
```

---

### 5.3 Screen Reader Testing Matrix

Test all critical user journeys on:

| Platform | Screen Reader | Browser |
|---|---|---|
| Windows | NVDA (free) | Firefox |
| Windows | JAWS | Chrome |
| macOS / iOS | VoiceOver | Safari |
| Android | TalkBack | Chrome |

Test these flows minimum:
- Page navigation using headings only (H key in NVDA/JAWS)
- Form completion without a mouse
- Modal open/close cycle with keyboard
- Error message announcement on form submission
- Notification/toast announcement

---

### 5.4 Media & Content Accessibility

- All `<video>` must have captions (WCAG 1.2.2). Auto-generated captions (YouTube etc.) do not meet the standard — they must be reviewed and corrected.
- All `<video>` with dialogue must have transcripts (WCAG 1.2.3 / 1.2.8).
- Audio-only content must have a text transcript.
- `<img>` alt text: descriptive for informational images, empty string (`alt=""`) for decorative images.
- Complex images (charts, diagrams) must have a long description — `aria-describedby` pointing to a full textual description.
- Never convey information by colour alone — always pair with a secondary cue (icon, label, pattern).
- Animation that auto-plays and lasts > 5 seconds must have a pause mechanism (WCAG 2.2.2).

---

## 6. PERFORMANCE — CORE WEB VITALS 2026

### 6.1 Current Thresholds (Google CrUX — 75th Percentile Basis)

| Metric | Good | Needs Improvement | Poor |
|---|---|---|---|
| **LCP** (Largest Contentful Paint) | ≤ 2.5s | 2.5–4.0s | > 4.0s |
| **INP** (Interaction to Next Paint) | ≤ 200ms | 200–500ms | > 500ms |
| **CLS** (Cumulative Layout Shift) | ≤ 0.1 | 0.1–0.25 | > 0.25 |

**INP replaced FID (First Input Delay) in March 2024** and remains the active interaction metric. INP measures the worst interaction latency across the full page lifecycle — it is significantly harder to optimise than FID.

---

### 6.2 LCP Optimisation Checklist

- [ ] Identify the LCP element using Chrome DevTools Performance panel or PageSpeed Insights
- [ ] Preload the LCP resource: `<link rel="preload" as="image" href="/hero.avif" fetchpriority="high">`
- [ ] Set `fetchpriority="high"` on the LCP `<img>` element itself
- [ ] Never lazy-load the LCP image (`loading="lazy"` kills LCP)
- [ ] Use AVIF (preferred) or WebP — not JPEG/PNG for hero images
- [ ] Serve from same origin or a CDN with low TTFB (< 600ms TTFB)
- [ ] Inline the LCP image's CSS — do not load it from an external stylesheet

---

### 6.3 INP Optimisation Checklist

INP = Input Delay + Processing Time + Presentation Delay. All three must be minimised.

- [ ] Break up long tasks (> 50ms) using `scheduler.yield()`:

```javascript
// Before: one long synchronous block
function processItems(items) {
  items.forEach(item => expensiveOperation(item));
}

// After: yield between items to keep the main thread responsive
async function processItems(items) {
  for (const item of items) {
    expensiveOperation(item);
    await scheduler.yield(); // Yields to browser between iterations
  }
}
```

- [ ] Debounce input handlers: search boxes, filters, resize callbacks
- [ ] Move heavy computation to Web Workers
- [ ] Avoid layout thrash: batch DOM reads and writes separately
- [ ] Use `content-visibility: auto` on off-screen sections to skip rendering

---

### 6.4 CLS Optimisation Checklist

- [ ] Always set `width` and `height` on `<img>` and `<video>` elements
- [ ] Reserve space for ads and embeds with `min-height` or aspect-ratio containers
- [ ] Never inject content above existing content (except on user interaction)
- [ ] Use `font-display: optional` or `font-display: swap` + size-adjust for web fonts:

```css
@font-face {
  font-family: "Geist";
  src: url("/fonts/Geist.woff2") format("woff2");
  font-display: swap;
  ascent-override: 90%;    /* Match fallback font metrics to prevent shift */
  descent-override: 20%;
  line-gap-override: 0%;
}
```

- [ ] Animations must only use `transform` and `opacity` — never animate layout properties

---

### 6.5 Resource Loading Order

```html
<head>
  <!-- 1. Critical meta — charset and viewport first, always -->
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />

  <!-- 2. Preconnect to critical third-party origins -->
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />

  <!-- 3. Preload LCP image and critical font -->
  <link rel="preload" as="image" href="/hero.avif" fetchpriority="high" />
  <link rel="preload" as="font" href="/fonts/Geist.woff2" type="font/woff2" crossorigin />

  <!-- 4. Critical CSS — inline or linked with high priority -->
  <style>/* Inline critical CSS — above-the-fold styles only */</style>
  <link rel="stylesheet" href="/css/main.css" />

  <!-- 5. Non-critical scripts — defer always -->
  <script src="/js/app.js" defer></script>

  <!-- 6. Prefetch next likely navigation -->
  <link rel="prefetch" href="/dashboard/" as="document" />
</head>
```

---

### 6.6 Image Standards

```html
<!-- Responsive image with modern format and fallback -->
<picture>
  <source type="image/avif" srcset="/img/hero-480.avif 480w, /img/hero-1024.avif 1024w" />
  <source type="image/webp" srcset="/img/hero-480.webp 480w, /img/hero-1024.webp 1024w" />
  <img
    src="/img/hero-1024.jpg"
    alt="Descriptive alt text"
    width="1024" height="576"
    fetchpriority="high"
    sizes="(max-width: 640px) 100vw, 50vw"
  />
</picture>
```

- Use AVIF for photographs (40–50% smaller than WebP at equivalent quality)
- Use WebP as fallback
- Use SVG for icons, logos, illustrations — never rasterise vector content
- Use `loading="lazy"` on all images below the fold
- Maximum file size: 200KB for full-width hero images, 100KB for content images, 20KB for thumbnails

---

## 7. INTERNATIONALISATION (i18n) & LOCALISATION (l10n)

### 7.1 HTML Foundation

```html
<!-- Always specify language on the root element -->
<html lang="en">          <!-- English -->
<html lang="en-GB">       <!-- British English -->
<html lang="ar" dir="rtl"> <!-- Arabic — right-to-left -->
<html lang="zh-Hant">     <!-- Traditional Chinese -->

<!-- Inline language change within content -->
<p>The French word <span lang="fr">bonjour</span> means "hello".</p>
```

**`lang` attribute values follow BCP 47** — use ISO 639-1 language codes and ISO 3166-1 country subtags. Wrong: `lang="english"`. Correct: `lang="en"` or `lang="en-IE"`.

---

### 7.2 RTL (Right-to-Left) Support

```css
/* Use logical properties — they flip automatically in RTL */
/* WRONG: Physical properties */
.card { margin-left: 16px; padding-right: 24px; text-align: left; border-left: 3px solid; }

/* CORRECT: Logical properties */
.card { margin-inline-start: 16px; padding-inline-end: 24px; text-align: start; border-inline-start: 3px solid; }

/* RTL flip icons using CSS */
[dir="rtl"] .icon-arrow { transform: scaleX(-1); }

/* Bidirectional text isolation */
.user-content { unicode-bidi: plaintext; }
```

**Logical property mapping:**
| Physical | Logical |
|---|---|
| `margin-left` | `margin-inline-start` |
| `margin-right` | `margin-inline-end` |
| `padding-top` | `padding-block-start` |
| `padding-bottom` | `padding-block-end` |
| `border-left` | `border-inline-start` |
| `text-align: left` | `text-align: start` |
| `float: right` | `float: inline-end` |

---

### 7.3 Text Externalisation — What Must Be Translated

Every user-visible string must be externalised to a translation file — never hardcode strings in components:

```json
// en.json
{
  "nav.home": "Home",
  "nav.careers": "Careers",
  "job.apply": "Apply now",
  "job.salary": "€{min}–€{max} per year",
  "search.results": "{count, plural, =0 {No results} one {# result} other {# results}}"
}
```

**Use ICU Message Format** for plurals, gender, and select patterns — it handles plural rules for all languages correctly (Russian has 4 plural forms; English has 2; Japanese has 1). Libraries: `formatjs` (React), `i18next`, `@fluent/bundle`.

---

### 7.4 Numbers, Dates, Currencies — Use the Intl API

```javascript
// Dates — never format manually
const date = new Date("2026-04-18");

// English (Ireland)
new Intl.DateTimeFormat("en-IE").format(date);        // "18/04/2026"
// French
new Intl.DateTimeFormat("fr-FR").format(date);        // "18/04/2026" (same layout, different locale context)
// Arabic
new Intl.DateTimeFormat("ar-SA").format(date);        // "١٨‏/٤‏/٢٠٢٦" (Eastern Arabic numerals)

// Currencies — always use locale-aware formatting
new Intl.NumberFormat("en-IE", { style: "currency", currency: "EUR" }).format(85000);
// → "€85,000.00"

new Intl.NumberFormat("de-DE", { style: "currency", currency: "EUR" }).format(85000);
// → "85.000,00 €"

// Relative time
new Intl.RelativeTimeFormat("en", { numeric: "auto" }).format(-3, "day");
// → "3 days ago"

// List formatting
new Intl.ListFormat("en").format(["React", "TypeScript", "Node.js"]);
// → "React, TypeScript, and Node.js"
```

**Rules:**
- Never construct date strings manually — timezone bugs and locale mismatches are guaranteed
- Always store dates in UTC; convert to local time at the point of display
- Currency amounts must be stored as integers (cents/minor units) — never as floating point
- Localise number formatting — not all locales use comma as the thousands separator

---

### 7.5 Locale Detection & Persistence

```javascript
// Preferred detection order
function detectLocale() {
  return (
    localStorage.getItem("preferred-locale")   // 1. Explicit user choice (persisted)
    ?? navigator.languages[0]                   // 2. Browser language preference
    ?? navigator.language                       // 3. Browser language (single)
    ?? "en"                                     // 4. Fallback
  );
}
```

**Never use IP geolocation alone to determine language** — a French speaker in London should not be defaulted to English. IP geo is useful for currency and legal jurisdiction detection only.

Set `Accept-Language` header on API requests so the server can return localised content.

---

### 7.6 Content Expansion — Layout Allowances

Translated content expands in length compared to English source strings. Design layouts to accommodate:

| Language | Approximate expansion vs English |
|---|---|
| German | +30–35% |
| Finnish | +30% |
| French | +15–20% |
| Arabic | +20–25% (RTL) |
| Japanese / Korean / Chinese | −30–40% (more compact) |

**Design rules:**
- Never use fixed-width buttons that clip at translated lengths — use `min-width` with padding
- Never truncate UI labels with CSS `text-overflow: ellipsis` without providing the full string via tooltip or `title` attribute
- Test all UI strings with German translations — if the layout survives German, it survives most languages

---

### 7.7 Font Considerations for Multi-Script Support

- **Latin/Cyrillic/Greek:** Geist, Inter, Noto Sans
- **Arabic:** Noto Naskh Arabic, IBM Plex Arabic
- **CJK (Chinese, Japanese, Korean):** Noto Sans CJK — be aware of large file sizes; use `unicode-range` subsetting
- **Devanagari (Hindi, Marathi):** Noto Sans Devanagari
- **Hebrew (RTL):** Noto Serif Hebrew

```css
/* Load CJK script font only for pages that need it */
@font-face {
  font-family: "Noto Sans JP";
  src: url("/fonts/NotoSansJP.woff2") format("woff2");
  unicode-range: U+3000-9FFF, U+FF00-FFEF;
}
```

---

## 8. SEO — FROM A UX/UI TECHNICAL PERSPECTIVE

> This section covers SEO elements that live in the frontend implementation layer. For full SEO strategy and schema specifications, refer to the SEO Master Specifications 2026 document.

### 8.1 HTML Signals That Directly Impact Rankings

```html
<!-- Every page -->
<title>Primary Keyword — Secondary | Brand</title>       <!-- 50–60 chars -->
<meta name="description" content="..." />                  <!-- 140–160 chars -->
<link rel="canonical" href="https://domain.com/page/" />

<!-- SPA / JavaScript-heavy apps -->
<!-- Ensure all content is in the initial HTML response OR use SSR/SSG -->
<!-- Google crawls JS but does so on a delayed secondary wave -->
<!-- Critical content must be in the HTML — not rendered client-side only -->

<!-- Structured data — in <head> or before </body> -->
<script type="application/ld+json">{ "@context": "https://schema.org", ... }</script>
```

---

### 8.2 Heading Hierarchy as UX and SEO Signal

```
H1 — One per page. Above the fold. Primary keyword. Maximum 70 characters.
  H2 — Section headings. Secondary keywords. Use 3–6 per standard page.
    H3 — Subsections under each H2.
```

Screen readers use headings as a document outline for navigation. Search engines use them as content signals. They serve both audiences simultaneously — treat them as semantic structure, not visual style.

---

### 8.3 Performance as SEO Factor (Page Experience Signal)

Google's Page Experience ranking signal combines:
- All three Core Web Vitals passing (LCP, INP, CLS) — see Section 6
- HTTPS — mandatory
- No intrusive interstitials on mobile (no full-screen popups blocking content)
- Mobile-friendly — verified via Google Search Console

---

### 8.4 JavaScript Rendering — What the Crawler Sees

```javascript
// Problem: content only in JS — crawler may not see it
document.getElementById("jobs").innerHTML = fetchedJobsHTML;

// Solution A: Server-Side Rendering (SSR) — HTML in initial response
// Solution B: Static Site Generation (SSG) — HTML built at deploy time
// Solution C: Dynamic Rendering — detect Googlebot and serve pre-rendered HTML

// Check: View Page Source (not DevTools) — if content is absent, crawler may miss it
```

---

### 8.5 URL & Navigation Structure

- Every page must be reachable by a standard `<a href>` link — JavaScript-only navigation breaks crawling
- Do not use `#` hash routes for crawlable content — use History API (`pushState`) or SSR routes
- SPA page transitions: update `<title>` and send a pageview event on every route change
- `rel="nofollow"` on untrusted user-generated links (comments, bios)
- `rel="sponsored"` on paid/affiliate links
- `rel="ugc"` on user-generated content links

---

## 9. SECURITY — FRONTEND RESPONSIBILITIES

### 9.1 Content Security Policy

```http
Content-Security-Policy:
  default-src 'self';
  script-src 'self' 'nonce-{random}';
  style-src 'self' 'unsafe-inline';
  img-src 'self' data: https://cdn.domain.com;
  font-src 'self';
  connect-src 'self' https://api.domain.com;
  frame-ancestors 'none';
  base-uri 'self';
  form-action 'self';
```

- Use nonce-based CSP for scripts — avoids the security hole of `'unsafe-inline'`
- `frame-ancestors 'none'` prevents clickjacking
- Never use `'unsafe-eval'` — it defeats the entire purpose of CSP

---

### 9.2 Input & Output Rules

- Sanitise all user-generated content before rendering as HTML — use DOMPurify
- Never use `innerHTML` with untrusted data — use `textContent` or `createElement`
- Validate on the client for UX; validate on the server for security — never rely only on client validation
- Use `rel="noopener noreferrer"` on all external links opened in new tabs: `<a href="..." target="_blank" rel="noopener noreferrer">`

---

### 9.3 Authentication & Storage

- Never store auth tokens in `localStorage` — XSS-vulnerable. Use `httpOnly` cookies.
- Never store PII (Personally Identifiable Information) in `localStorage` or `sessionStorage`
- Implement CSRF protection on all state-changing form submissions
- Enforce logout on session expiry with proper client-side state cleanup

---

## 10. QUALITY ASSURANCE & TESTING

### 10.1 Automated Testing Baseline

Every production codebase must have:

| Layer | Tool | Coverage Target |
|---|---|---|
| Unit tests | Vitest / Jest | Core logic functions |
| Component tests | Testing Library | All interactive components |
| E2E tests | Playwright | Critical user journeys |
| Accessibility | axe-core / axe-playwright | All page routes |
| Performance | Lighthouse CI | All routes — fail build if score drops |
| Visual regression | Chromatic / Percy | Design system components |

---

### 10.2 Accessibility Automated Coverage

Automated tools catch ~30–40% of accessibility issues. They are necessary but not sufficient. Always supplement with:

- Manual keyboard navigation testing
- Screen reader testing (NVDA, VoiceOver)
- Zoom testing at 200% and 400% (WCAG requires content usable at 400% for WCAG 1.4.10)
- High contrast mode testing (Windows High Contrast / forced-colors)

```javascript
// Playwright + axe-core integration
import { checkA11y } from "axe-playwright";

test("careers page has no accessibility violations", async ({ page }) => {
  await page.goto("/careers/");
  await checkA11y(page, null, { detailedReport: true });
});
```

---

### 10.3 Browser & Device Testing Matrix (2026)

**Target Baseline (Baseline 2024):** All features used must be in Baseline 2024 or earlier to ensure cross-browser compatibility.

| Browser | Engine | Priority |
|---|---|---|
| Chrome 120+ | Blink | Critical |
| Safari 17+ | WebKit | Critical |
| Firefox 121+ | Gecko | High |
| Edge 120+ | Blink | High |
| Samsung Internet 23+ | Blink | Medium |
| Chrome Android | Blink | Critical (mobile) |
| Safari iOS 17+ | WebKit | Critical (mobile) |

**Device testing:**
- Physical device testing on iOS Safari is non-negotiable — iOS Safari has unique rendering quirks that emulators miss, particularly around `dvh`, `position: fixed`, and scroll behaviour.
- Test at 320px viewport width — the minimum required by WCAG 1.4.10 (Reflow).

---

## APPENDIX — AUTHORITATIVE REFERENCES

| Standard | Authority | URL |
|---|---|---|
| WCAG 2.2 | W3C | w3.org/TR/WCAG22 |
| ARIA 1.2 | W3C | w3.org/TR/wai-aria-1.2 |
| HTML Living Standard | WHATWG | html.spec.whatwg.org |
| CSS Cascade & Layers | W3C | w3.org/TR/css-cascade-5 |
| Web Content Baseline | WebDX CG | webstatus.dev |
| Core Web Vitals | Google | web.dev/articles/vitals |
| i18n Best Practices | W3C | w3.org/International |
| ICU Message Format | Unicode | unicode-org.github.io/icu |
| EU Accessibility Act | EU | ec.europa.eu/social/accessibility |
| EU AI Act (UI disclosure) | EU | artificial-intelligence-act.com |
| GDPR (EU) | EU | gdpr-info.eu |

---

*Version 1.0 — April 2026*
*Validated against: WCAG 2.2 (Oct 2023), W3C HTML Living Standard, CSS Cascade Level 5, Baseline 2024 browser compatibility data, Core Web Vitals thresholds (INP active March 2024), EU Accessibility Act (enforced June 2025), and all referenced specifications as of April 2026.*
