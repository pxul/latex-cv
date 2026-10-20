# latex-cv

A LaTeX style for a plain, single-column CV, with a matching cover letter. It is a modified copy of [Cies Breijs's `resume.sty`](https://github.com/cies/resume): the modifications are an engine-aware font selector, contact-line icons from Tabler Icons, text extraction that applicant tracking systems can read cleanly, and a set of spacing and page-break fixes so that an entry is never split across pages.

The examples are a CV and cover letter for Lieutenant Commander Data, who has been advised that keeping a current CV is a human practice. The rendered output is in [`examples/cv.pdf`](examples/cv.pdf) and [`examples/cover-letter.pdf`](examples/cover-letter.pdf).

## Requirements

- A TeX distribution with the TeX Gyre fonts, `fontspec`, `microtype`, `mdwlist`, `relsize`, `ulem`, `hyperref` and `xcolor`. TeX Live's full install has all of them.
- LuaLaTeX or XeLaTeX for the OpenType fonts and the contact-line icons. The style also builds under pdfLaTeX, using the equivalent Type 1 font package; the icon macros then expand to nothing and the contact details list as plain text.
- Optionally `accsupp`, from the oberdiek bundle. With it, the contact icons and the date stamp are hidden from text extraction, so an applicant tracking system does not read the icons as stray characters or run the date into the name. Without it the style still builds.
- The cover letter uses KOMA-Script's `scrlttr2` and `ragged2e`.

## Building the examples

From the repository root:

```sh
latexmk examples/cv.tex
latexmk examples/cover-letter.tex
```

The `.latexmkrc` selects LuaLaTeX and writes output to `out/`. Building from the root is what lets `\usepackage{resume}` and the `fonts/` path resolve; if you build from elsewhere, put `resume.sty` and `fonts/` next to your document or on `TEXINPUTS`.

Add `-pvc` to rebuild on every save, or `-pdf` to build with pdfLaTeX instead (no icons, see Requirements):

```sh
latexmk -pvc examples/cv.tex
latexmk -pdf examples/cv.tex
```

## Using it for your own CV

Copy `resume.sty` and `fonts/` next to your `.tex` file, then start from `examples/cv.tex`. The document is built from a small set of macros:

| Macro | Purpose |
|---|---|
| `\maintitle{name}{subtitle}{date}` | Name in large bold, an italic subtitle, and a small grey date top right. |
| `\roottitle{title}` | A top-level section: Experience, Skills, Education. |
| `\headedsection{left}{right}{body}` | An employer or institution: name on the left, location or dates on the right, then the body. Consecutive sections get extra space between them and are the preferred page-break points. |
| `\headedsubsection{title}{dates}{body}` | A position within an employer: bold title on the left, italic dates on the right. |
| `\bodytext{text}` | An indented paragraph, or an `itemize` list, under a section or subsection. Never split across pages. |
| `\cvicon{\tiLocation}`, `\tiPhone`, `\tiMail`, `\tiLinkedin` | Contact-line icons, coloured to match links. |
| `\sbull` | A spaced centred bullet for separating contact details. |
| `\acr{text}` | Slightly smaller text for acronyms. |
| `\nobreakvspace{len}`, `\breakvspace{len}` | Vertical space that forbids or allows a page break. |

### Settings

All in `resume.sty`, each with a comment beside it:

- `\cvfont`: the document font. One of `pagella`, `latinmodern`, `schola` (the default), `bonum`, `termes`. Each maps to a TeX Gyre or Latin Modern family for fontspec and to the matching package for pdfLaTeX.
- `cv-link`: the colour used for links and icons. Black by default; links are underlined so they survive a monochrome printer.
- `\cvcompanysep`: extra space between one employer block and the next.
- `\cvsectionpenalty`: how strongly a page break is drawn to the gap between employer blocks.
- `\cviconraise`: vertical nudge for the icons.

### Page breaks

An entry (`\headedsection`, `\headedsubsection` or `\bodytext`) has no legal break point inside it, so a page can only break between entries, and it prefers to break between employers. Breaks between the items of a nested `itemize` remain legal, as the escape for a block too tall for one page. Pages are `\raggedbottom`, so a page that breaks early ends short rather than stretching its contents to the bottom margin.

## Licence

MIT. The original style is copyright Cies Breijs; the modifications are copyright Paul Jefferson. See `LICENSE`.

`fonts/tabler-icons.ttf` is [Tabler Icons](https://github.com/tabler/tabler-icons) v3.31.0, MIT licensed; see `fonts/README.md`.

The examples are a parody. Star Trek and its characters belong to Paramount.
