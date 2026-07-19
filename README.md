# Compliance Currents — website

Static site for compliancecurrents.com. No build step, no framework, no dependencies.
Plain HTML + one stylesheet, deployed via GitHub Pages.

- **Maintenance guide for agents:** see [AGENTS.md](AGENTS.md) — read it fully before editing.
- **Publishing a brief:** copy `briefs/_TEMPLATE.html`, fill it, add its card to `briefs/index.html`. Harrison reviews before merge to `main`.
- **Deploying:** every push to `main` is the deploy. GitHub Pages serves this repo's root directly (Settings → Pages → Deploy from branch → `main` / root).
- `.nojekyll` must stay — it stops GitHub from excluding `briefs/_TEMPLATE.html`.

Not legal advice. © 2026 Compliance Currents.
