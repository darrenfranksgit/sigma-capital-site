# Sigma Capital – Website Build

## Project Overview
Convert a single-file HTML mockup (v5) into a production-ready, responsive, Vercel-deployed static website. The mockup is a marketing site for Sigma Capital, a surgical AI investment firm founded by Bjoern von Siemens.

## Stack Decision
**Vite + vanilla HTML/CSS/JS** (static site). This is a single-page marketing site with no dynamic content, no CMS, no auth. Vite gives us: dev server with HMR, asset optimisation/hashing, trivial Vercel deployment. No framework overhead needed.

If the client later needs multi-page routing, blog, or CMS integration, migrate to Astro or Next.js at that point.

## Source Mockup
The original mockup is a single 674-line HTML file with all CSS inlined in `<style>` tags and JS at the bottom. It was produced as a design mockup, not a production build.

## Architecture

```
sigma-capital/
├── index.html              # Main page (clean semantic HTML only)
├── styles/
│   ├── variables.css        # CSS custom properties (colours, spacing, type scale)
│   ├── base.css             # Reset, body, typography defaults
│   ├── nav.css              # Navigation (sticky header + mobile hamburger)
│   ├── hero.css             # Hero section
│   ├── philosophy.css       # Philosophy section
│   ├── edge.css             # "What Sets Us Apart" section
│   ├── leadership.css       # Leadership cards
│   ├── portfolio.css        # Portfolio grid + filter pills
│   ├── insights.css         # News/insights cards
│   ├── ecosystem.css        # Ecosystem partners + governance cards
│   ├── footer.css           # Footer
│   └── responsive.css       # All media queries consolidated
├── scripts/
│   └── main.js              # Portfolio filter logic, smooth scroll, mobile nav toggle
├── public/
│   ├── images/              # Optimised local copies of any images (replace Unsplash hotlinks)
│   ├── favicon.svg          # Sigma Capital favicon
│   └── og-image.jpg         # Open Graph image for social sharing
├── CLAUDE.md                # This file
├── package.json
├── vite.config.js
├── vercel.json              # Vercel config (rewrites, headers, caching)
└── .gitignore
```

## Design System (extracted from mockup)

### Colours
```css
--navy: #04102a;
--gold: #8a6a1a;
--gold-light: #B8974A;
--cream: #faf7f2;
--cream-alt: #f4f1eb;
--muted: #5a5a57;
--rule: #d4d1cc;
```

### Typography
- **Headlines**: Newsreader (Google Fonts), serif, weight 300 (light)
- **Body/UI**: Inter (Google Fonts), sans-serif, weights 300–700
- **Icons**: Material Symbols Outlined (variable font)

### Key Sizes (desktop)
- Max content width: 1440px
- Wrap padding: 48px (→ 32px at ≤1280px → 20px at ≤640px)
- Section padding: 120px top/bottom (→ 72px at ≤640px)
- Hero: min-height 840px, 88vh, max 960px

## Sections (in order)
1. **Nav** – Sticky, logo left, links centre, CTA button right. Needs mobile hamburger menu (not in mockup; must be added). **v1: omit the "News" link from navigation.**
2. **Hero** – Full-bleed background image with dark overlay + gradient. Headline, sub, sup text, two CTAs.
3. **Philosophy** – 2-column grid. One plain card, one shaded card.
4. **What Sets Us Apart** – 2-column (image left, content right). 3 pillars in a sub-grid.
5. **Leadership** – 4-column card grid (→ 3 → 2 → 1 col responsive). Silhouette placeholders, badges, LinkedIn links.
6. **Portfolio** – Filter pills + 4-column card grid. JS filtering by data-cat attribute.
7. **Insights (News)** – Featured article left, 3 side cards right. **v1: build the section but hide it with `display: none`. Omit the "News" link from both the main nav and footer nav. This section will be made visible in a later update once news content is ported from an existing external site or new content is seeded.**
8. **Ecosystem** – Intro text + governance cards (4-col → 1-col).
9. **Footer** – 3-column grid: brand, contact (email + LinkedIn links + locations), nav links. Bottom strip with copyright. **v1: omit the "News" link from footer navigation.**

## Portfolio Categories & Company Assignments

Filter pills map to `data-cat` attributes as follows:

| Filter label | `data-cat` value | Companies |
|---|---|---|
| AI & Data | `data` | Caresyntax, Applied AI Corp |
| Medtech & Robotics | `medtech` | Siemens Healthineers, Intuitive Surgical, Auris Health, Endoquest Robotics, Capstan Medical, DasLab, FidoCure, Neteera, roclub |
| Care Infrastructure | `care` | Huma, Ada Health, LillianCare, XO Life, hi.health |
| Investment Platforms | `acq` | Puma VC |

Ensure every portfolio card's `data-cat` matches this mapping. The mockup may have some cards assigned incorrectly; this table is the source of truth.

**Portfolio disclaimer** (`.port-note` element at the bottom of the portfolio section) should read:
> "Includes direct investments and fund investments made through Sigma Capital and surgical.ai vehicles."

## Contact Email

The mockup uses `investments@vsiemens.com`. The intended production email is `investments@sigmacap.ai`, pending Google Workspace setup. **For v1: use a CSS class or data attribute to make the email address easy to swap.** Add a `<!-- TODO: update contact email to investments@sigmacap.ai when Workspace is confirmed -->` comment in the HTML next to every instance of the email address.

## Critical Build Tasks

### Must do
- [ ] Extract all inline styles from the footer into proper CSS classes
- [ ] Replace `onmouseover`/`onmouseout` inline handlers with CSS `:hover` states
- [ ] Add mobile hamburger navigation (the mockup only wraps links; needs a proper toggle menu)
- [ ] Download and self-host the two Unsplash hero/edge images (or replace with client-provided assets). For now, use placeholder local files with a TODO comment.
- [ ] Add proper `<meta>` tags: description, OG tags, Twitter card
- [ ] Add smooth scroll behaviour for anchor links
- [ ] Ensure all images have proper `alt` text
- [ ] Add `loading="lazy"` to below-fold images
- [ ] Portfolio filter JS: animate card show/hide (CSS transitions, not jQuery)
- [ ] Verify all external links open in new tabs with `rel="noopener noreferrer"`
- [ ] Add favicon
- [ ] Performance: preload critical fonts, inline critical CSS if needed
- [ ] Hide the News/Insights section (`display: none`) and remove "News" from nav and footer nav links
- [ ] Re-map all portfolio card `data-cat` attributes to match the category table in this file
- [ ] Update the portfolio disclaimer to: "Includes direct investments and fund investments made through Sigma Capital and surgical.ai vehicles."
- [ ] Add TODO comments next to all instances of `investments@vsiemens.com` for the pending email swap to `investments@sigmacap.ai`

### Should do
- [ ] Add subtle scroll-triggered fade-in animations for sections (IntersectionObserver)
- [ ] Add keyboard accessibility for portfolio filters and mobile nav
- [ ] Test and refine responsive behaviour at 1280, 980, 640, and 375px breakpoints
- [ ] Add print stylesheet or at minimum `@media print` rules to hide nav/footer

### Nice to have
- [ ] Add a skip-to-content link for accessibility
- [ ] Structured data (JSON-LD) for the organisation
- [ ] Sitemap.xml and robots.txt

## Vercel Config
```json
{
  "buildCommand": "npx vite build",
  "outputDirectory": "dist",
  "framework": null,
  "headers": [
    {
      "source": "/assets/(.*)",
      "headers": [
        { "key": "Cache-Control", "value": "public, max-age=31536000, immutable" }
      ]
    }
  ]
}
```

## Git Workflow
- `main` branch auto-deploys to production on Vercel
- Use conventional commits: `feat:`, `fix:`, `style:`, `chore:`

## Notes for Claude Code
- Preserve the exact visual design from the mockup; this is a client-approved layout
- Do not swap fonts, colours, or spacing unless explicitly asked
- The mockup has base64-encoded logos for some portfolio companies; keep these as-is for now
- The mockup references a Cloudflare email decode script at the bottom; remove this in the production build
- The portfolio category-to-company mapping in this file overrides whatever the mockup has; re-assign `data-cat` values accordingly
- The "Contact" section at the bottom of the page is integrated into the footer, not a separate section
- The News/Insights section must be built but hidden for v1 (see Sections list above)
- The contact email will change from `investments@vsiemens.com` to `investments@sigmacap.ai`; keep this easy to swap (see Contact Email section above)
