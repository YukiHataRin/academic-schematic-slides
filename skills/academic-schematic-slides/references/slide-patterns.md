# Slide patterns

Layout recipe for each slide type. All coordinates are for 1920x1080 and follow `visual-system.md`. Each slide has one point, a short statement title at (96, 104), the footer chrome, and a figure that carries the content. Sample PNGs in `assets/samples/` show most of these.

## Cover (`01_cover`)
- Only the title (as the user gave it). No date, author, affiliation, logo, unless requested.
- Orange-red vertical bar at x = 120 (width 14, rounded, 290-400 tall), vertically centred around y = 540.
- Title x = 178, 84-90 px bold. A title with a colon may be split: first part bold 84 px, second part regular 52 px on the next line.
- No footer, no page number.

## Section divider (`02_section`)
- Small "Section B" (30 px, muted), short orange-red rule (96 x 8, rounded), then the section name 100-112 px bold at x about 176. Everything left-aligned, vertical centre about 540. Page number bottom-right.
- Minimal: no pictures. Footer section label may be omitted.

## Concept / intuition slide
- Goal: make the idea visible in a few seconds with a drawing of the situation, not a definition.
- One scene or one comparison: a drawn physical setup (camera, ray, objects), two distributions, before/after panels, a space with clusters.
- Use panels a/b for "situation" vs "consequence". One pill (orange-red or neutral) can state the take-away in at most six words.
- Add a single caption line only if the drawing cannot say it.

## Mechanism / method slide
- A schematic of how one thing works: the data in, the operation, the data out, drawn with the vocabulary of `figure-grammar.md`.
- Horizontal flow centred at y of about 600 inside a zone; labels under objects; symbols on tensors and arrows.
- If a detail matters, add an enlarged inset below or to the side (panel c) linked by dashed lines.
- Add a tag if the mechanism's source is uncertain.

## Architecture overview (`03_architecture`)
- Three zones: **a** training-only (top, teal dashed), **b** inference path (below, blue tinted, contains input -> frozen encoder -> features -> head -> output), **c** enlarged detail (inset linked by dashed lines).
- Input thumbnail left, output colour map right with colour bar; losses as circles where prediction meets target (training-only zone); legend at y = 978.
- Complex architectures: use the build-up pattern instead.

## Step-by-step build-up (`04_build_step2`, `05_build_step3`)
- Identical base layout on every step; each step adds one element group; reserved empty zones show a dashed outline with a muted title "added in step N".
- Titles "Step k: <what is added>". Last step: complete figure, then an inference view with removed parts greyed and crossed.

## Two-way comparison / before-after (`06_concept_two_approaches`)
- Two equal panels (72-948 and 972-1848, y 170-810), letters a and b. Same components drawn in both; differences carried by fill, marks, and a small quantitative bar or pill at the bottom of each panel.
- Title states the conclusion ("Training only a small head leaves the backbone untouched").
- If numbers are invented, tag `illustrative`.

## Results chart (`07_results_chart`)
- Panel a: grouped bars (3 groups x 2-4 series) with error bars, value labels, legend row at the top, "lower/higher is better" at the right. Panel b (optional): lines with s.d. bands and direct labels.
- Axis lines only; ticks at round values; unit in the caption; source or "mean +- s.d., n = ..." bottom-left; honesty tag if synthetic.
- Title states the finding with the direction ("The compact head lowers error on all three datasets").
- Never more than 4 series per chart. Extra series go to another slide.

## Qualitative results (images)
- Rows = examples, columns = input / methods / ground truth, each cell a thumbnail with the same colour map; short column headers in muted 22 px; a shared colour bar once at the right. Mark the winner subtly (outline), not with text.

## Maths slide (`08_math_annotated`)
- One display equation at the top centre (size about 90 px), typeset from separate pieces so that each term has a known position.
- Under it, a small picture for each term (mask, prediction, target, curve...) in the colour that the term is typeset in, connected by thin elbow lines from the term to the picture; operators between pictures where they help.
- One plain-language gloss line at the bottom (optional, 26 px muted).
- Definitions appear on the pictures or beside terms, never as a separate legend list.
- Multi-line derivations: one step per slide, same position of the equation, the changed part in the accent colour.

## Table-like comparison (rare)
- Only when many methods are compared on many yes/no attributes: rows = methods, columns = attributes, filled dot (teal) = yes, hollow grey dot = no, purple `?` = to be confirmed, highlighted row for "ours" (teal tint), legend underneath. Column headers are two-line, 22-24 px bold.

## Parallel claims / "five gaps"
- Do not make five identical text cards. Draw one object per claim (a chip, a document, a lock, a few dots with a question mark) in the palette with a one- or two-word label beneath; or use a single drawn landscape with numbered markers. Keep colours meaningful.

## Timeline / plan
- Drawn axis with ticks, bars for periods in primary blue (orange-red only for the critical item), small pictograms or marks for milestones, labels of 1-3 words. Tag guesses `estimate`.

## Summary / take-away
- A reduced version of the main figure with the contributions marked by numbered pills, or three small drawings in a row with one-word captions. At most one sentence.
- Next steps / open questions: a short list is acceptable here (3-6 lines, 30 px), because lists are the content.

## Common additions
- Legend row (y = 978) whenever fixed semantics are used.
- Source note (bottom-right, 20 px) when numbers or figures come from one source; "synthetic example scene" when thumbnails are synthetic.
- Honesty tag at the element it concerns, not in the footer.
