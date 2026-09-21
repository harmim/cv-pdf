# CLAUDE.md

## Project overview

This repository holds Dominik Harmim's CV, typeset in LaTeX using the
[AltaCV](https://github.com/liantze/AltaCV) class (`altacv.cls`, vendored
in this repo). The rendered output is committed as `CV.pdf`.

Key files:
- `main.tex` — main document (layout, colors, main content).
- `sidebar1.tex`, `sidebar2.tex` — content for the two sidebar pages.
- `altacv.cls` — the AltaCV document class (vendored, do not treat as
  generated).
- `photo.jpeg` — profile photo used in the header.
- `CV.pdf` — the built, committed PDF output.

## Build instructions

The document requires **XeLaTeX** (not pdfLaTeX) because AltaCV relies on
`fontawesome5`/`academicons` icon fonts and system font loading via
`fontspec`.

Build with a full TeX Live install (e.g. MacTeX or TeX Live) that includes
XeLaTeX and the `academicons`, `fontawesome5`, and `lato` packages:

```bash
xelatex main.tex
xelatex main.tex   # run twice to resolve references/layout
```

This produces `main.pdf`. After a successful build, rename/copy it over the
committed `CV.pdf`:

```bash
cp main.pdf CV.pdf
```

Build artifacts (`*.aux`, `*.log`, `*.out`, etc.) are gitignored — only
`CV.pdf` should be committed.

## Conventions

- Keep `CV.pdf` in sync with `main.tex`/`sidebar*.tex` whenever content
  changes — commit the rebuilt PDF alongside source edits.
- Don't commit LaTeX build artifacts (covered by `.gitignore`).
