# Sigma Capital Website – Claude Code Starter Prompt

Copy and paste the prompt below into Claude Code after navigating to your project directory.

---

## Setup Prompt (paste this first)

```
Read the CLAUDE.md file in this directory. Then read the source mockup file I'm about to provide. Your job is to scaffold a Vite static site project that faithfully reproduces this mockup as a production-ready website.

Steps:
1. Initialise the project: package.json, vite.config.js, .gitignore
2. Split the monolithic HTML into the file structure described in CLAUDE.md
3. Extract all CSS into the separate stylesheet files listed in CLAUDE.md
4. Move all inline styles (especially in the footer) into proper CSS classes
5. Replace all onmouseover/onmouseout handlers with CSS :hover
6. Add a mobile hamburger menu for the nav (CSS + minimal JS)
7. Add the portfolio filter JS (filter cards by data-cat, with CSS transition for show/hide)
8. Re-map all portfolio card data-cat attributes to match the category table in CLAUDE.md — that table is the source of truth, not the mockup
9. Update the portfolio disclaimer (.port-note) to: "Includes direct investments and fund investments made through Sigma Capital and surgical.ai vehicles."
10. Build the News/Insights section but hide it with display:none. Remove the "News" link from both the main nav and footer nav.
11. Add TODO comments next to every instance of investments@vsiemens.com noting the pending swap to investments@sigmacap.ai
12. Add smooth scroll for anchor links
13. Add meta tags: description, OG title/description/image, Twitter card
14. Add a minimal favicon (SVG, navy background, gold "Σ" letter)
15. Create vercel.json per the config in CLAUDE.md
16. Initialise git, make an initial commit

Do NOT change the visual design — colours, fonts, spacing, layout must match the mockup exactly. The mockup is the source of truth for design; CLAUDE.md is the source of truth for content/data mapping.

For the two Unsplash images (hero background and edge section), download them locally into public/images/ and reference them from there. Remove the Cloudflare email-decode script.

After scaffolding, run `npx vite` to verify it builds and serves correctly.
```

## Follow-up Prompt (after initial scaffold)

```
Now let's harden the responsive behaviour:

1. Test at 1440px, 1280px, 980px, 640px, and 375px widths
2. The mobile hamburger should: show a full-screen overlay nav on tap, animate smoothly, close on link click or outside tap
3. Verify the portfolio filter pills wrap correctly on mobile
4. The leadership grid should go 4→3→2→1 columns
5. The footer 3-column grid should stack on mobile
6. Add IntersectionObserver fade-in animations for each section (subtle, 0.3s ease, translateY(20px))
7. Add focus-visible styles for keyboard accessibility on all interactive elements

Show me any issues you find.
```

## Git + Vercel Prompt (when ready to deploy)

```
1. Create a new GitHub repository called "sigma-capital-site" (I'll do this manually — just prepare the git remote commands)
2. Give me the exact terminal commands to:
   - Set the remote origin
   - Push to main
3. Then give me the Vercel CLI commands to:
   - Link this directory to a new Vercel project
   - Set up auto-deploy from the GitHub repo
   - Do an initial deploy

If I don't have the Vercel CLI installed, tell me how to install it first.
```

---

## Notes

- Run Claude Code from inside the `sigma-capital-site/` directory
- The CLAUDE.md gives Claude Code full context on the design system, architecture and task list
- The source HTML mockup should be placed in the project root as `source-mockup.html` so Claude Code can reference it
- You can iterate section by section if you prefer — just ask Claude Code to work on one section at a time
