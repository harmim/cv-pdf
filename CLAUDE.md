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
- `fonts/FontAwesome.otf` — patched FontAwesome v4 font (see quirk below),
  loaded explicitly by `main.tex`.
- `CV.pdf` — the built, committed PDF output.

## Build instructions

The document requires **XeLaTeX** (not pdfLaTeX) because AltaCV relies on
the legacy `fontawesome` (v4) and `academicons` icon fonts, loaded via
`fontspec`'s system font lookup.

Build with a full TeX Live install (e.g. MacTeX or TeX Live) that includes
XeLaTeX and the `academicons`, `fontawesome`, and `lato` packages:

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

### macOS FontAwesome quirk

On macOS, XeTeX/fontspec resolves font *names* through Core Text. The
`FontAwesome.otf` shipped in TeX Live only has Macintosh-platform `name`
table records (no Microsoft-platform ones), which Core Text silently
rejects — `\faicon` glyphs then render as blank/`nullfont` with no build
error. `academicons` is unaffected (its font's `name` table is fine).

The fix already applied in `main.tex`: a patched copy of `FontAwesome.otf`
(Microsoft-platform name records added via fontTools) is vendored at
`fonts/FontAwesome.otf`, and `main.tex` overrides the `\FA` font-switch
command to load it by explicit path:

```latex
\renewcommand{\FA}{\fontspec[Path=./fonts/,Extension=.otf]{FontAwesome}}
```

You'll still see a handful of harmless `! Package fontspec Error: The font
"FontAwesome" cannot be found` messages early in the log — that's
`altacv.cls`'s own initial (name-based) load attempt, which the override
above replaces before any icon is actually typeset. As long as there are no
`Missing character` / `nullfont` warnings later in the log, the build is
fine. This quirk is macOS-specific; Linux (fontconfig-based) TeX Live
installs are not known to need this workaround, but the override is
harmless there too.

## Conventions

- Keep `CV.pdf` in sync with `main.tex`/`sidebar*.tex` whenever content
  changes — commit the rebuilt PDF alongside source edits.
- Don't commit LaTeX build artifacts (covered by `.gitignore`).
