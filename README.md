# jackyyang258.github.io

Personal academic site for Siqi Yang. Plain static HTML/CSS — no framework, no
templating, no build step for the site itself.

## Pages

| File | URL | What it is |
|---|---|---|
| `index.html` | `/` | Homepage: about, publications, experience, education, honors & service, skills, contact |
| `lwail.html` | `/lwail.html` | Paper microsite — *Latent Wasserstein Adversarial Imitation Learning* (ICLR 2026) |
| `website/index.html` | `/website/` | Paper microsite — *What Does a Self-Evolved Harness Encode?* (submitted to AAAI 2027) |

## Supporting files

- `assets/` — images used by `lwail.html`
- `website/static/images/` — images used by the harness paper page
- `cv.tex` — LaTeX source for the CV; CI compiles it to `/cv.pdf`
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
| `/cv/` | `/cv.pdf` |
| `/publications/`, `/projects/` | `/#publications` |
| `/teaching/` | `/#honors` |
| `/news/`, `/repositories/` | `/` |

## Deploy

`.github/workflows/deploy.yml` runs on every push to `main`:

1. compiles `cv.tex` to `cv.pdf` (`continue-on-error` — a LaTeX failure can
   never block the site going out)
2. rsyncs the site into `dist/`, excluding sources and repo metadata
3. pushes `dist/` to the `gh-pages` branch

Adding a new page needs no workflow change — rsync picks it up automatically.

## Local preview

```bash
python3 -m http.server
```

Then open <http://localhost:8000>. Opening `index.html` directly via `file://`
also works; only the root-relative links in the redirect stubs need a server.

## History

The site was previously built on the [al-folio](https://github.com/alshedivat/al-folio)
Jekyll theme. That full state is archived on the branch
`archive/jekyll-site-2026-08-24`.
