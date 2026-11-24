# Typesetting maths into slides

Requirement: every variable, symbol and formula, **including those inside figure labels**, is real typeset maths (italic variables, proper sub/superscripts, hats, fractions, calligraphic letters), not plain text pretending to be maths. One symbol, one spelling for the whole deck (decide a notation table in `outline.md`: for example `x` input, `\hat{y}` prediction, `y^{*}` target, `\mathcal{L}` loss).

Do not bind yourself to one tool. Probe what exists, pick the most robust route, and verify visually (see the end).

## Routes and trade-offs

| Route | How | Good | Bad |
|---|---|---|---|
| **A. LaTeX to DVI to dvisvgm glyph paths** | compile a snippet with `latex`, convert with `dvisvgm --no-fonts` | True TeX quality; output is pure paths (no font dependency, survives any rasteriser); tight bbox with baseline info; huge symbol coverage | Needs a TeX install and dvisvgm; one compile per snippet (cache them) |
| **B. LaTeX to PDF then SVG** | `pdflatex`/`xelatex`, then `dvisvgm --pdf`, `pdf2svg` or `inkscape` | Works with fonts, TikZ or packages that need PDF output | Extra tool; fonts may be embedded as text in SVG, so verify |
| **C. MathJax (Node)** | `mathjax-full` `tex2svg` with SVG output and `fontCache: 'none'` | No TeX install; paths; widely available through npm | Needs Node/npm; sizes in `ex` units; slightly less exact spacing than TeX |
| **D. matplotlib mathtext** | `rcParams['svg.fonttype']='path'`, draw text with `$...$` into an SVG figure | Already present in many Python setups; consistent with matplotlib charts | Smaller LaTeX subset (no `\dfrac` alignment tricks, limited packages); different look |
| **E. Browser-rendered KaTeX/MathML, captured as PNG** | render in a headless browser, screenshot with transparent background at 3x, embed as `<image>` | No TeX at all; good looking | Raster (blurry if scaled up); cannot be coloured after the fact; do not embed with `foreignObject`, many converters drop it |
| F. Unicode characters in the text font | type `α`, `β`, `ŷ` as text | Zero effort | **Not acceptable** for a deck's variables: combining hats, sub/superscripts and calligraphic letters break; fonts may lack glyphs. Only tolerable for a lone Greek letter inside running text if the glyph was verified in the final PNG |

Prefer A, then C, then D, then E. If nothing is available, tell the user what is missing and offer to proceed with the best remaining route; do not silently fall back to plain text.

## Route A in detail (tested recipe)

Preamble of the snippet (tight page, math packages):
```
\documentclass[10pt]{article}
\usepackage{amsmath,amssymb,bm}
\usepackage[active,tightpage]{preview}
\setlength\PreviewBorder{0.4pt}
\pagestyle{empty}
\begin{document}
\begin{preview}$ <FORMULA> $\end{preview}
\end{document}
```
Use `$\displaystyle ...$` for large display equations, plain `$...$` for labels.
```
latex -interaction=nonstopmode -halt-on-error m.tex
dvisvgm --no-fonts --bbox=preview -o m.svg m.dvi
```
- `--no-fonts` converts glyphs to `<path>`/`<use>` shapes. The resulting `<svg viewBox='x0 y0 w h'>` is in TeX points. **The baseline is at y = 0**, so the ascent is `-y0` and the descent is `y0 + h`. Strip the outer `<svg>`, keep `<defs>` and the body, and embed them inside `<g transform='translate(X,Y) scale(k)' fill='COLOR'>`, where (X, Y) is the point where you want the baseline-left of the formula.
- Scale: for a surrounding text size of S px, use `k = S/10 * 1.06` (the 10 pt snippet at that scale matches the x-height of a sans text font; display equations can use S of 80-100).
- Several formulas in one SVG: dvisvgm reuses ids (`g0-12`, `page1`). **Prefix every id and `#id` reference per snippet** (for example hash of the formula + occurrence index) or glyphs will be swapped between formulas.
- Colour: set `fill` on the wrapper group (paths have no colour of their own). Different colours for terms need separate snippets.
- Cache results by hash of (formula, display flag) so that rebuilding hundreds of slides is fast.
- Failure is loud: a LaTeX error returns non-zero; show the log and fix the formula (unescaped `_`, `%`, `&`, missing braces).

## Mixing text and maths on one line
Write labels as strings with `$...$` for the maths, e.g. `"target $y^{*}$"`. Split on `$`: even segments are text, odd segments are maths. Measure the width of each text segment with the real font (a browser canvas `measureText`, or font metrics) and the width of each formula (`w * k`), place segments left to right on a common baseline, and compute the total width for centring. Take the line's ascent/descent from the largest piece (use it to keep labels inside boxes). Do **not** use `text-anchor` on the whole line when it contains maths; compute the start x yourself.

## Annotating terms of a formula
To connect parts of an equation to pictures, typeset the formula in **pieces** (for example `\mathcal{L}\;=`, `\dfrac{1}{|M|}\sum_{i\in M}`, `\bigl|`, `\hat{y}_i`, `\;-\;`, `y^{*}_i`, `\bigl|`), lay them out horizontally on one baseline with small gaps, and record each piece's x-range. Draw leader lines from the bottom centre of a piece to the picture it explains, and colour the piece and the picture frame the same. Check that the pieces read as one equation (spacing around `=`, `-`, big bars). `assets/samples/08_math_annotated.png` shows this.

## Notation hygiene
- Declare symbols once in `outline.md` and reuse the identical TeX string everywhere (a constants table in the generator avoids typos).
- Words inside maths use `\mathrm{}`: `\mathcal{L}_{\mathrm{aux}}`, not `\mathcal{L}_{aux}`.
- Vectors/tensors/sets/operators follow one convention (for example bold lowercase for vectors, calligraphic for losses and sets).
- Units go in text, not in italics: `7 m`, not `$7 m$`.
- If a source paper uses different symbols, either keep the paper's symbols on slides that explain the paper (and say so once), or map them to your notation on a dedicated slide. Do not mix silently.

## Verification (do not skip)
Render the PNG and look at it: formulas must show real glyphs (no boxes, no raw `\alpha`, no blank gaps), sub/superscripts at correct sizes, fraction bars aligned, hats centred, baselines of text and maths matching, colours as intended, nothing clipped at the viewBox edges. Zoom into small labels (cards, nodes, axis captions), because maths of 20-26 px is where failures show. See `review.md`.
