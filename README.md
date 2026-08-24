# jackyyang258.github.io

Personal academic site for Siqi Yang. Plain static HTML/CSS, no build step, no framework.

## Structure

- `index.html` &mdash; homepage (about, publications, experience, education, honors, contact)
- `lwail.html` &mdash; paper microsite for *Latent Wasserstein Adversarial Imitation Learning* (ICLR 2026)
- `website/index.html` &mdash; paper microsite for *What Does a Self-Evolved Harness Encode?* (submitted to AAAI 2027)
- `assets/` &mdash; images referenced by `lwail.html`
- `website/static/images/` &mdash; images referenced by `website/index.html`
- `projects/triage/`, `projects/lwail/` &mdash; redirect stubs preserving old citation URLs
- `cv.tex` &mdash; LaTeX source for the CV
- `_bibliography/papers.bib` &mdash; bibliography source of truth (not deployed; the publication list on the homepage is written directly into `index.html`)

## Deploy

`.github/workflows/deploy.yml` copies the static files straight to the `gh-pages` branch on every push to `main`. No Jekyll, no build step.

## Local preview

Just open `index.html` in a browser, or run:

```bash
python3 -m http.server
```
