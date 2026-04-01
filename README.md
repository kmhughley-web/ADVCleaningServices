# ADVanced Cleaning Services — Website

**Live site:** [advcleaningservices.com](https://advcleaningservices.com)

> "Where Every Detail Is the Standard."

A professional, fully responsive single-page website for **ADVanced Cleaning Services** — a residential and commercial cleaning company based in Tulsa, OK, founded by Andrea Del Valle.

---

## Project Overview

| Property | Detail |
|---|---|
| Business | ADVanced Cleaning Services |
| Owner | Andrea Del Valle |
| Location | Tulsa, OK (serving Tulsa, Bixby, Broken Arrow, Jenks, Owasso, Sand Springs) |
| Phone | (918) 948-0071 |
| Email | andrea@advcleaningservices.com |
| Built by | [Hovera Group LLC](https://hoveragroup.com) |

---

## Tech Stack

This is a **pure static HTML/CSS/JavaScript** site — no build tools or frameworks required. It is designed for maximum performance and simplicity.

| Layer | Technology |
|---|---|
| Markup | HTML5 |
| Styling | CSS3 (custom properties, grid, flexbox) |
| Fonts | Google Fonts (Cormorant Garamond, Manrope) |
| Animations | CSS keyframes + Intersection Observer API |
| Hosting | Vercel (primary) / GitHub Pages (fallback) |

---

## Repository Structure

```
ADVCleaningServices/
├── index.html          # Main single-page site
├── images/
│   ├── andrea.png      # Founder portrait (hero + story sections)
│   └── og-image.png    # Open Graph social share image
├── vercel.json         # Vercel deployment configuration
├── .gitignore
└── README.md
```

---

## Deployment

### Vercel (Recommended)

1. Log in to [vercel.com](https://vercel.com) and click **Add New Project**.
2. Import the `kmhughley-web/ADVCleaningServices` GitHub repository.
3. Vercel auto-detects the static site — no build command or output directory needed.
4. Click **Deploy**. The site will be live in under a minute.
5. In **Project Settings → Domains**, add your custom domain (e.g., `advcleaningservices.com`).

> The `vercel.json` file in this repo pre-configures security headers and image caching automatically.

### GoHighLevel / Growth Hub 365 (White Label)

To embed or redirect this site within GoHighLevel:

**Option A — Custom Domain Redirect (Recommended)**
1. In GHL, go to **Sites → Funnels** or **Websites**.
2. Create a new site and set the custom domain to `advcleaningservices.com`.
3. Use a **redirect** or **iframe embed** pointing to the Vercel-hosted URL.

**Option B — Host HTML in GHL Website Builder**
1. In GHL, go to **Sites → Websites → New Website**.
2. Use the **Custom Code** block to paste the full `index.html` content.
3. Upload images via GHL's media library and update `src` paths accordingly.

**Option C — Subdomain Setup**
1. Host the main site on Vercel at `www.advcleaningservices.com`.
2. Use GHL for a booking/funnel subdomain such as `book.advcleaningservices.com`.
3. Link CTAs on the main site to the GHL booking funnel URL.

---

## Local Development

No build step required. Simply open `index.html` in any browser:

```bash
# Using Python's built-in server (recommended for local testing)
python3 -m http.server 8080

# Then open: http://localhost:8080
```

---

## Customization Notes

- **Brand colors** are defined as CSS custom properties in `:root` at the top of `index.html` — easy to update.
- **Phone number** appears in two places: the hero button and the contact section.
- **Email** is in the contact section.
- **Services list** is in the `#services` section.
- **Reviews** are in the `#reviews` section — add or update as needed.
- **Service areas** are in the `.area-banner` section.

---

## License & Credits

&copy; 2026 ADVanced Cleaning Services. All rights reserved.  
Website designed and developed by [Hovera Group LLC](https://hoveragroup.com).
