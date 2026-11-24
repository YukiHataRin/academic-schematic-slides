---
name: academic-schematic-slides
description: "Use this skill whenever the user wants a presentation, slide deck, slides, talk, lab meeting, journal club, paper reading, literature review, thesis or research-proposal pages, or any single figure to be drawn: diagram, architecture figure, model figure, flowchart, pipeline, workflow, method figure, mechanism figure, concept figure, schematic, illustration, Figure 1. Applies even when no style or format is named. Produces academic, journal-style (Nature-like) slides in which most figures are schematic illustrations that draw the data itself (images, maps, feature vectors, layer stacks, node graphs, curves) instead of text cards joined by arrows; one SVG per slide, rendered to 1920x1080 PNG, with real typeset math and a mandatory visual self-check. Chinese triggers: 簡報, 投影片, 流程圖, 架構圖, 示意圖, 方法圖, 組會, 文獻研討, 論文導讀."
---

# Academic schematic slides (SVG to PNG)

Journal-style slides whose figures look like Figure 1 of a strong paper: **schematic illustrations first**. The reader should see what the data looks like and where it goes, not read sentences inside boxes.

## Language
- Talk to the user in the user's language. Slide text is **English by default** unless the user asks otherwise.
- Keep slide text minimal: a short statement title, tiny labels, no paragraphs.

## Core idea: schematic first (most important rule)
Most slides should be a **schematic illustration**: model architectures, pipelines, mechanisms, before/after comparisons, training vs. inference, data flow.
1. **Boxes are only for operations. Data is drawn as what it looks like**: image thumbnails, colour-mapped depth/heat maps, rows of coloured cells for vectors, token grids, stacked slabs for networks, node-and-edge graphs for MLPs, curves, point clouds, small axes with clusters. Operations are small markers, narrow strips or labelled stacks, never a white card with a sentence in it.
2. **Never** draw "one white card per step, text inside, arrows chaining the cards in a row or column". Equal-sized boxes must not stand for unlike things. No slide-sized walls of text.
3. **Label-removal test**: remove every text label mentally. If the structure and the data are no longer visible, it is a dull card diagram. Redraw it.
4. Inherently sequential content (pipelines, protocols, timelines) is also drawn schematically: nodes are pictures of the data or of the object, the connectors carry small operation marks, zones group the stages.
5. Use space as grammar: tinted zones, fixed semantic colours/line styles, zoom-in insets, panel letters, formulas written on the arrows or tensors they describe, complex figures built up over several slides.

Details, vocabulary and SVG snippets: `references/figure-grammar.md`. See `assets/samples/09_card_vs_schematic.png` for the same pipeline drawn both ways.

## Hard deliverable spec
- One SVG per slide, converted to a PNG with the same base name; **1920x1080 (16:9)**; files `NN_slug.svg` / `NN_slug.png`. Number of slides is unlimited; 40-70 is normal for a literature review.
- **One point per slide.** Title = short statement sentence. Information already in the figure is not repeated in text.
- **Cover slide: the title only** (no date, author, affiliation) unless the user asks.
- **All variables and formulas are truly typeset maths**, including labels inside figures; one symbol, one notation, whole deck. Never fake maths with plain text or an unchecked font. See `references/math.md`.
- Figures over tables or bullet text. A symbol matrix is acceptable only to compare many items on many attributes.
- Numbers or claims of uncertain origin carry a small tag (`unverified`, `estimate`, `to be confirmed`, `illustrative`). Untagged means "checked against the source by you".
- Colour maps need a colour bar, units and range. Charts: short axes and captions, colour-blind-safe colours, error bars when the data has variance. Name the source bottom-right when numbers come from one.
- Fixed page number position, small section label bottom-left, one visual language across the deck.

## Workflow
1. **Outline first** (`outline.md`): slide order, the single point of each slide, and for each slide the figure type (schematic, mechanism, chart, maths...). Content comes from the user's sources (paper, notes, data). If a detail is missing, read the source or ask; never invent. Mark gaps with tags.
2. **Choose tools** from what the environment offers (see below). Decide the maths route, the SVG-to-PNG route, and fonts.
3. **Build shared components once** (page chrome, panel, label, arrow, stack, MLP, vector, thumbnail, colour map, maths helper) and **generate slides with a script**. Do not hand-write dozens of SVGs.
4. **After each slide: render to PNG and look at it** (zoom into dense regions), fix and re-render until clean. Checklist and what bad looks like: `references/review.md`. Keep a short `review_log.md` with one line per slide.
5. **Finish with a whole-deck pass**: order, consistency of colours, line styles, notation, tags, panel letters, positions of recurring elements.
6. Report: output path, slide count, outline, tools used, tagged slides, items needing user confirmation, anything unresolved.

Suggested layout of the deliverable: `<deck>/svg/`, `<deck>/png/`, `<deck>/assets/`, `<deck>/build/` (generator and helpers), `outline.md`, `review_log.md`, `README.md` (how to rebuild).

## Which reference to read
| Need | File |
|---|---|
| Colours, type sizes, line widths, margins, panel letters, semantic colour/line language, colour maps | `references/visual-system.md` |
| How to draw figures: data vocabulary, zones, insets, build-ups, do/don't, card-vs-schematic examples | `references/figure-grammar.md` |
| Layout of each slide type (cover, section, concept, mechanism, architecture, build-up, comparison, results, maths, summary) | `references/slide-patterns.md` |
| Typesetting maths into SVG (several routes and trade-offs) | `references/math.md` |
| Self-check after each slide and at the end | `references/review.md` |
| Copy-able examples (SVG + PNG) | `assets/samples/` (`elements.svg` is a one-page component sheet) |

Read `figure-grammar.md` and `visual-system.md` before drawing anything; open the matching sample PNGs to calibrate the look.

## Environment and tool choice
Let the user's environment decide; do not hard-wire a tool. Probe first (which tools exist, which fonts exist) and write down the choice.
- **Maths**: LaTeX plus dvisvgm glyph outlines is the most robust when available; alternatives (MathJax, matplotlib mathtext, pre-rendered formulas) are in `references/math.md`.
- **SVG to PNG**: a headless browser (also handy for measuring text), or rsvg-convert / cairosvg / inkscape. Whichever is used, check fonts and maths survive the conversion.
- **Fonts**: use a font stack with fallbacks (see `visual-system.md`); confirm the font really resolves on the rendering machine and no glyph is missing.
- Follow the user's own language/tooling conventions (e.g. their Python environment manager). Do not install system packages without asking; report what is missing and offer options.

## Illustrative imagery (optional)
For scene illustrations (a camera, a room, a lab bench) either (a) use an image-generation tool if the environment has one: prompts must ask for **no text, no labels, no logos** (all text stays in SVG), record prompt, model and seed in `assets/README.md`, send no personal information in prompts; or (b) draw procedural or vector scenes (ray-cast boxes, simple polygons, noise fields). State which was used. Real data images are better than synthetic ones when the user provides them; label synthetic scenes ("synthetic example").

## Honest labelling
Never present a guessed number, an unread claim or a made-up example as fact. Tag it, or leave it out, or ask. Charts built from invented numbers say so (`illustrative data`). Source of numbers goes bottom-right in small type.

## Relation to other skills and output types
- If the user wants a **PNG/SVG-page deck or figures**, use this skill. If they explicitly want an editable `.pptx`/Google Slides/PDF-from-a-deck-tool file, use the matching tool, but still apply this skill's figure grammar for the pictures inside it (draw the figures as SVG/PNG and place them).
- For a stand-alone chart, follow the chart rules here (palette, error bars, colour-blind safety); if a dedicated charting skill is present it may be used for the plotting itself.
- Manuscript figures for papers can reuse the same figure grammar; keep the same palette and semantic language.
