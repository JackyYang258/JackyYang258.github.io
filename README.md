# jackyyang258.github.io

Personal academic site for Siqi Yang. Plain static HTML/CSS — no framework, no
templating, no build step for the site itself.

## Pages

| File | URL | What it is |
|---|---|---|
| `index.html` | `/` | Homepage: about, publications, experience, education, honors & service, skills, contact |
| `lwail.html` | `/lwail.html` | Paper microsite — *Latent Wasserstein Adversarial Imitation Learning* (ICLR 2026) |
| `website/index.html` | `/website/` | Paper microsite — *What Does a Self-Evolved Harness Encode?* |

## Supporting files

- `assets/` — images used by `lwail.html`
- `website/static/images/` — images used by the harness paper page
- `cv.tex` — LaTeX source for the CV; CI compiles it to `assets/siqi-yang-cv.pdf`
- `_bibliography/papers.bib` — bibliography source of truth (not deployed; the
  publication list is written directly into `index.html`)
- `.nojekyll` — tells GitHub Pages to serve the branch as-is
- `robots.txt`, `sitemap.xml`

## Legacy redirects

The old al-folio site published URLs that are cited in papers and elsewhere.
These directories are one-line redirect stubs that keep them working:

| Old URL | Redirects to |
|---|---|
| `/projects/triage/` | `/website/index.html` |
| `/projects/lwail/` | `/lwail.html` |
| `/cv/` | `/assets/siqi-yang-cv.pdf` |
| `/publications/`, `/projects/` | `/#publications` |
| `/teaching/` | `/#honors` |
| `/news/`, `/repositories/` | `/` |

## Deploy

GitHub Pages serves the `main` branch directly, so **there is no site build
step** — pushing to `main` publishes. Everything in the repo is therefore
web-reachable, including `README.md` and `cv.tex`.

The one workflow, `.github/workflows/build-cv.yml`, recompiles `cv.tex` into
`assets/siqi-yang-cv.pdf` and commits it back. It runs only when `cv.tex`
changes, because the LaTeX packages it needs are awkward to install locally.

## Local preview

```bash
python3 -m http.server
```

Then open <http://localhost:8000>. Opening `index.html` directly via `file://`
also works; only the root-relative links in the redirect stubs need a server.

## History

The site was previously built on the [al-folio](https://github.com/alshedivat/al-folio)
Jekyll theme, whose workflow built with Jekyll and pushed to a `gh-pages`
branch. Pages actually serves `main`, so that branch is now unused and can be
deleted. The full pre-rewrite state is archived on `archive/jekyll-site-2026-08-24`.
