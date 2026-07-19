# Compliance Currents — site maintenance guide

This guide is for the AI agents (Hermes / briefcase profile) that maintain this site.
Read it fully before editing anything. When this guide and a task instruction conflict,
this guide wins unless Harrison explicitly overrides it.

## What this site is

A fully static site. No build step, no framework, no JavaScript, no dependencies.
Every page is plain HTML that links `assets/styles.css`. Editing a page = editing
its HTML. Deploying = uploading the folder. That simplicity is deliberate — keep it.

## File map

```
site/
  index.html                    Homepage
  about.html                    About page
  briefs/
    index.html                  Brief archive (list of cards)
    _TEMPLATE.html              Template for new briefs — copy, never edit
    2025-05-co-supervision-scope.html   Sample brief
  assets/
    styles.css                  All styling. Design tokens at the top.
    cc_mark.svg                 Five-bar mark (standalone)
    cc_favicon.svg              Favicon tile (three-bar cut)
  AGENTS.md                     This file
```

The header/footer are duplicated in each page by design (no includes system).
If you change the nav or footer, change it in EVERY page. Grep before you claim done:
`grep -rl "site-header" .`

## How to publish a new brief

1. Copy `briefs/_TEMPLATE.html` to `briefs/YYYY-MM-short-slug.html`
   (example: `2026-08-co-telehealth-consent.html`).
2. Fill every `{{PLACEHOLDER}}`. The structure is non-negotiable:
   title + subtitle + receipt line + What changed + When it changed +
   Why it matters + Sources + ONE next action + not-legal-advice line.
3. Delete the agent-instructions comment block at the top of the article.
4. Add a matching `brief-card` to `briefs/index.html` (newest first).
5. If the brief is recent, add/replace a card in the "Recent work" section
   of `index.html`. Keep at most 3 cards on the homepage.
6. STOP. Do not deploy. Harrison reviews every brief before it goes live.

## Content rules (hard rules — never violate)

- VERIFY EVERYTHING. Every claim checked against the primary source. Every URL
  opened and confirmed before it's cited. If a claim can't be verified, it does
  not run. Never assert a rule "just changed" without proving what specifically
  changed and when it took effect.
- BANNED PHRASES: never use "plain-English" or "plain-language" anywhere on the
  site, in any copy, ever. Describe readability by demonstrating it, not naming it.
- "Not legal advice" appears on every page footer and at the end of every brief.
  Never remove it.
- NO PRICING anywhere on the site. No numbers, no "starting at," no tiers.
  Agency interest routes to the LinkedIn contact link.
- No clickbait, no humor in public copy, no manufactured urgency, no emoji.
- Sentence-style headlines; factual titles. The brief title states what happened.
- Effective dates are sacred: format `eff. MM·DD·YY`, always inside
  `<span class="eff">` so they render amber.
- Dates in receipts use middle dots (·), not slashes.

## Design rules

- All colors come from the CSS variables at the top of `assets/styles.css`.
  Never hardcode a new hex in a page. The palette is: navy #16324F,
  teal #1B7F8E, amber #B7791F (dates/deadlines ONLY), cream #FAF8F4.
- Amber is reserved exclusively for effective dates and deadlines. If amber is
  appearing anywhere else, that's a bug.
- Fonts are loaded from Google Fonts in each page head: Fraunces (display),
  Public Sans (body), IBM Plex Mono (receipts/eyebrows). Note: Fraunces is a
  placeholder standing in for a licensed serif (Canela / Freight Display).
  If Harrison licenses one, swap the font link + `--display` variable in one pass.
- The five-bar mark is inline SVG in each header. Its geometry is final.
  Don't redraw it, recolor it, or add bars.

## Things that need updating (known TODOs)

- [ ] Substack URL: currently `brieflycompliant.substack.com` everywhere.
      When the Substack is renamed for Compliance Currents, find-and-replace
      across all pages: `grep -rl "brieflycompliant" .`
- [ ] Contact: currently routes to Harrison's LinkedIn. If a
      hello@compliancecurrents.com address is set up, replace the LinkedIn
      "Get in touch" links (keep LinkedIn in the footer Follow list).
- [ ] The sample brief carries a "Sample brief" tag. When the first
      client-grade public brief ships, the sample can be retired or kept —
      Harrison's call.

## Deploying — GitHub Pages

This site deploys from a GitHub repository via GitHub Pages, "deploy from
branch" mode. There is no build step. **A merge to `main` IS the deploy** —
which means nothing merges to `main` without Harrison's approval.

Workflow for agents:
1. Make changes on a branch (e.g. `brief/2026-08-co-telehealth-consent`).
2. Run the pre-deploy checklist below.
3. Open a pull request and stop. Harrison merges; the merge publishes.
   Never push directly to `main`.

Repo facts:
- Pages is configured under Settings → Pages → "Deploy from a branch,"
  branch `main`, folder `/ (root)`.
- `.nojekyll` at the repo root is REQUIRED. Without it, GitHub's Jekyll
  processing silently drops `briefs/_TEMPLATE.html` (underscore-prefixed
  files are excluded). Never delete it.
- All internal links are relative, so the site works both at
  `https://<user>.github.io/<repo>/` and at the custom domain. Keep new
  links relative — never absolute paths starting with `/`.
- After a deploy, changes can take a minute or two to appear; check the
  repo's Actions tab for the "pages build and deployment" run.

Custom domain (once compliancecurrents.com is registered):
1. Settings → Pages → Custom domain → enter `compliancecurrents.com`.
   GitHub writes a `CNAME` file into the repo — commit it, keep it.
2. At the DNS provider: apex A records pointing to GitHub Pages' published
   IP addresses, plus a `www` CNAME record pointing to `<user>.github.io`.
   Confirm the current IPs against GitHub's own Pages documentation at
   setup time rather than trusting any cached list.
3. Turn on "Enforce HTTPS" once the certificate is issued (can take an hour).

## Pre-deploy checklist (run every time)

- [ ] Every `{{PLACEHOLDER}}` is gone: `grep -rn "{{" . --include="*.html"`
      (the only allowed hits are inside `_TEMPLATE.html`)
- [ ] Every cited URL opens and shows what the citation claims
- [ ] Banned-phrase check: `grep -rin "plain-english\|plain-language" . --include="*.html"` returns nothing
- [ ] "Not legal advice" present in every page footer
- [ ] New brief has its card in `briefs/index.html`
- [ ] Harrison has approved the content. No approval, no deploy.
