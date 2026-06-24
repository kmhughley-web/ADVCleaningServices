# CLAUDE.md

Guidance for AI assistants (Claude Code and others) working in this repository.

## What this is

A **single-page marketing website** for **ADVanced Cleaning Services**, a residential
and commercial cleaning company in Tulsa, OK (owner: Andrea Del Valle). The site is
built and maintained by Hovera Group LLC.

- **Live site:** https://advcleaningservices.com
- **Repo:** `kmhughley-web/ADVCleaningServices`
- **Tagline:** "Where Every Detail Is the Standard."

## Tech stack — read this first

This is **pure static HTML/CSS/JavaScript with no build step, no frameworks, no
dependencies, and no package manager.** There is no `package.json`, no `node_modules`,
no bundler, no transpiler. Do **not** introduce any of these unless explicitly asked.

- **HTML5** — hand-written, semantic.
- **CSS3** — all styling lives **inline** in a single `<style>` block inside each HTML
  file (custom properties, CSS grid, flexbox, keyframe animations). There are **no
  separate `.css` files.**
- **JavaScript** — vanilla, **inline** in `<script>` blocks at the bottom of each HTML
  file. No external JS libraries (the only third-party script is the GoHighLevel form
  embed loaded from `api.growthhub365.com`). There are **no separate `.js` files.**
- **Fonts** — Google Fonts: Cormorant Garamond (serif, headings) + Manrope (sans, body).

Because everything is inline in one file, "editing the site" means editing `index.html`
directly. Keep CSS in the `<style>` block and JS in the `<script>` block — match the
existing inline pattern rather than splitting files out.

## Repository layout

```
ADVCleaningServices/
├── index.html          # The entire main site — markup + inline CSS + inline JS
├── feedback.html       # Standalone internal client-review/feedback page (not linked from index)
├── vercel.json         # Vercel deploy config + security headers + caching rules
├── _headers            # Headers file for Netlify/Cloudflare-Pages-style static hosts
├── CNAME               # Custom domain for GitHub Pages (advcleaningservices.com)
├── README.md           # Business-facing overview + deployment instructions
├── favicon.ico, favicon-16/32/192/512.png, apple-touch-icon.png
└── images/
    ├── andrea.png            # Founder portrait (hero + story sections)
    ├── og-image.png          # Open Graph / social share image
    ├── oven-before/after.jpg        # Before/after gallery
    ├── commercial-before/after.jpg  # Before/after gallery
    ├── movein-before/after.jpg      # Before/after gallery
    └── adv-work.mp4          # Work footage (large ~3MB asset)
```

## Anatomy of `index.html`

A single document. Top to bottom:

1. **`<head>`** — SEO meta (description, keywords), Open Graph + Twitter Card tags,
   mobile/theme meta, favicons, `<link rel="canonical">`, and a **schema.org
   `LocalBusiness` JSON-LD block** listing services and `areaServed`. Keep the JSON-LD,
   the visible copy, and the meta tags **consistent** with each other when editing
   business facts (services, phone, areas).
2. **Inline `<style>`** — design system + all component styles + responsive
   breakpoints (`@media(max-width:900px)` and `@media(max-width:600px)`).
3. **`<body>`** sections, in order:
   `nav` → `hero` → `marquee` → `philosophy` → `services` (6 cards) → `results`
   (before/after grid) → `story` → `reviews` → `area-banner` → `about-blurb` →
   `contact` (GHL form iframe) → service **modals** → cookie banner → privacy modal → footer.
4. **Inline `<script>`** at the very bottom — scroll/nav behavior, `IntersectionObserver`
   reveal animations, modal open/close, cookie consent, image protection, phone obfuscation.

### Design system (CSS custom properties)

All colors are defined once in `:root` at the top of the `<style>` block — edit there,
never hard-code hex values inline. The palette: sage greens (`--sage`, `--sage-deep`,
`--sage-muted`), creams (`--cream`, `--cream-deep`, `--paper`, `--white`), gold accents
(`--gold`, `--gold-soft`), and text (`--dark`, `--text`, `--text-light`).

### Services + modals pattern

There are **6 services**, each rendered as a `.service-card` in `.services-track` and
each backed by a `.modal-overlay` with id `modal-<key>`:

| # | Title             | Modal key         |
|---|-------------------|-------------------|
| 01 | Deep Cleaning     | `deep`            |
| 02 | Basic Cleaning    | `basic`           |
| 03 | Commercial        | `commercial`      |
| 04 | Post-Construction | `postconstruction`|
| 05 | Move-In / Out     | `moveinout`       |
| 06 | Restaurant Cleaning | `restaurant`    |

Cards are fully clickable (`onclick="openModal('<key>')"`, plus `role="button"`,
`tabindex`, and an `aria-label`). `openModal(id)` / `closeModal(id)` toggle the
`.open` class, lock body scroll, and manage focus; Escape closes any open modal.
**To add a service:** add a `.service-card` AND a matching `modal-<key>` overlay AND
an entry in the schema.org `hasOfferCatalog` AND (optionally) the marquee — keep all
four in sync.

### Lead capture — GoHighLevel (GHL)

The quote form in the `#contact` section is a **GoHighLevel native iframe embed**
(`api.growthhub365.com/widget/form/DjXXH9U2oW4owq6DoniA`), with `form_embed.js` loaded
right after it. Bot protection, rate limiting, and lead routing are handled by GHL.
**Never put webhook URLs, API keys, or form credentials in client-side code** — a prior
commit (`7f32517`) specifically removed an exposed webhook, and CSP is scoped to the GHL
domains. If you change the form, update the GHL domains in **both** `vercel.json` and
`_headers` CSP if needed.

### Client-side behaviors in the inline `<script>`

- **Reveal animations:** elements with class `.reveal` (and stagger classes `.d1`–`.d6`)
  are observed by an `IntersectionObserver` that adds `.vis`.
- **Cookie consent:** banner shown unless `localStorage['adv-cookie-consent']` is set;
  `acceptCookies()` / `declineCookies()`. Site uses only essential cookies (no analytics).
- **Image protection:** right-click/context-menu and drag are disabled on `<img>`.
- **Phone obfuscation:** the number is split into parts in JS and assembled into any
  `[data-phone]` element at runtime, so scrapers can't read it from the HTML source.
- **Honeypot:** `.hp-field` is a hidden anti-spam field.

## `feedback.html` (separate internal tool)

A standalone, **client-facing review/approval page** used during site development — it
is **not** linked from `index.html`. It presents the site's copy section-by-section and
collects edits, then assembles everything into a **`mailto:` link** (the form does not
POST anywhere). It shares the same design tokens as `index.html` but is otherwise
independent. Treat it as an internal/temporary artifact, not part of the public site UX.

## Hosting, deployment & headers

- **Primary host: Vercel.** `vercel.json` configures `@vercel/static`, security headers,
  and caching (`images/*` cached 1 year immutable; `*.html` no-cache). No build command.
- **Fallback host: GitHub Pages** via the `CNAME` file (`advcleaningservices.com`).
- **`_headers`** provides the equivalent security headers for static hosts that read that
  format (Netlify / Cloudflare Pages).
- **Deployment is push-to-deploy** on the connected host — there is no manual build/release
  step in this repo. Just commit and push.

### Security headers (keep these two in sync)

Both `vercel.json` and `_headers` define: `X-Content-Type-Options`, `X-Frame-Options`,
`Referrer-Policy`, `Permissions-Policy`, `X-XSS-Protection`, `Strict-Transport-Security`,
and a **Content-Security-Policy**. The CSP allow-lists Google Fonts and the GHL/LeadConnector
domains. If you add a third-party script, font, image source, or iframe, you must update the
CSP in **both** files or it will be blocked in production.

## Local development

No build step. Serve the folder over HTTP (needed for the GHL iframe and relative paths):

```bash
python3 -m http.server 8080   # then open http://localhost:8080
```

Opening `index.html` directly via `file://` mostly works but the embedded form and some
CSP-governed behavior won't behave like production.

## Conventions for making changes

- **Edit `index.html` in place.** Keep styles in the inline `<style>` block and scripts in
  the inline `<script>` block — do not extract separate `.css`/`.js` files or add a build
  system unless explicitly requested.
- **Single source of truth for business facts.** Phone `(918) 948-0071`, email
  `andrea@advcleaningservices.com`, owner Andrea Del Valle, and the service-area list
  (Tulsa, Bixby, Broken Arrow, Jenks, Owasso, Sand Springs) appear in multiple places
  (meta, JSON-LD, visible copy, footer). Update them everywhere together.
- **Preserve accessibility.** The site has a skip link, ARIA labels on nav/cards/modals,
  `focus-visible` outlines, `prefers-contrast: high` overrides, and `.sr-only` utilities.
  Don't regress these.
- **Preserve security posture.** Don't reintroduce inline secrets/webhooks; keep CSP and
  the image/phone protections intact.
- **English only.** A prior PT/EN language toggle was intentionally removed (commit
  `6a3b253`) in favor of browser translation. Don't re-add a custom translation toggle.
- **Branding:** "ADVanced Cleaning Services" in copy/UI; "LLC" appears only where legally
  appropriate (e.g., footer, privacy) — several commits deliberately trimmed it elsewhere.

### Commit message style

History uses short, conventional-ish prefixes when applicable and plain descriptive
subjects otherwise — e.g. `fix:`, `feat:`, `security:`, `remove:`. Match that tone. Keep
messages specific about what changed (the existing log is a good model).

> **History note:** commit `89c65e7` ("Update Restaurant Daily Maintenance description")
> accidentally truncated `index.html` from ~1053 lines to 26 (deleting `<style>`,
> `<body>`, and `<script>`). It was restored from the last good commit `99870c6` via
> `git checkout 99870c6 -- index.html`. If you ever see `index.html` as a tiny partial
> `<head>` stub again, that's the same regression — restore from the last full version.
