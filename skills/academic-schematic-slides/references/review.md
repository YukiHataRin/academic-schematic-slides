# Reviewing each slide with your own eyes

This skill ships no checker. You are a capable visual model: use that. A slide is **not done** until you have looked at the rendered PNG and found it clean. How you obtain the pixels, and how you measure anything, is your choice (a headless browser, a rasteriser, cropping and enlarging with an image library, measuring text boxes in a browser, listing fonts, or anything else at hand). Nothing here prescribes a method; it describes what to look for.

## Procedure per slide
1. Convert the SVG to a PNG at the full 1920x1080 and open the PNG as an image (not just the SVG source).
2. Look at the whole slide first: Is the point of the slide obvious in three seconds? Is it the schematic style (data drawn, not text cards)? Is there too much text?
3. Then inspect regions: **enlarge** any dense area (labels in a figure, small maths, legend, colour bar, corners, the zone borders, the footer). Details that vanish at slide scale show up at 2-3x.
4. Fix every defect, re-render, look again. Repeat until clean.
5. Add one line to `review_log.md`: slide number, what you checked, what you fixed ("overlap of label and arrow at head, moved label"; "clean").

## What to look for, and what wrong looks like

**Text out of bounds**
- Letters cut off at the canvas edge, or text running closer than about 28 px to the border.
- Text sticking out of its box, pill, node, card or panel; labels wider than the object they name. Text pressed against a border with less than about 5 px of padding.
- Titles running past the right margin or wrapping into the figure.

**Overlap and covering**
- Text over text (even slightly: doubled letters, a smudge).
- Text under an arrow, line, shape, thumbnail or zone border; a line running through a label; an arrowhead landing on a letter.
- Shapes hiding each other unintentionally (a stack top covering a vector, a node covering a connector end).
- Dashed connector lines passing through unrelated objects. Arrows should stop at object edges, not at centres.

**Maths**
- A formula shown as raw source (`\alpha`, `$x$`), as boxes, as question marks, or blank.
- Wrong or fallback glyphs, missing hats or subscripts, subscripts as large as the base, fractions with a misplaced bar, clipped formula edges, baseline mismatch between text and maths on the same line.
- Different notation for the same quantity on different slides; maths in a figure label rendered as plain italics.

**Fonts and characters**
- Missing-glyph rectangles, `?`, or strangely different widths (a fallback font silently replaced the intended one); asymmetric spacing; misaligned right-aligned text.
- Bold/regular weights not as intended.

**Colour maps, charts, units**
- A colour map without colour bar, range or unit; a bar whose end labels are missing or touch the image.
- A prediction and its target using different maps or ranges.
- Chart without axis caption/unit, without a statement of what the error bars are, with legend entries that do not match colours, bars with value labels colliding with error bars, direct labels sitting on lines.
- Series that are hard to distinguish (similar greys), colours that fail for colour-blind readers, rainbow maps.
- Honesty: invented numbers without a tag; a source missing; claims you did not verify without a tag.

**Alignment and rhythm**
- Elements that should share a centre line or baseline but are a few pixels off; uneven gaps between equal elements; panels not on a common grid; objects of the same kind drawn at different sizes.
- Panel letters missing, out of order, at inconsistent positions.
- Legend entries that do not match what the slide actually uses, or missing legend.

**Consistency with the deck**
- Colours, line styles, dash patterns and the semantic language (frozen, trained, training-only, inference, removed) the same as on neighbouring slides.
- Recurring elements (title, footer, page number, legend, source note, panel positions in build-up sequences) in the same place; in build-up sequences the base drawing has not moved.

**Density**
- Too crowded (touching elements, small text under 20 px, more than about 6-8 objects in a figure, paragraphs).
- Too empty (large dead areas inside panels, a tiny figure floating on the page, a panel much larger than its content). Fix by resizing the panel or the figure, not by adding text.

**Schematic-style check**
- Remove the labels mentally (see `figure-grammar.md`). Cards with sentences, equal boxes for unequal things, text-only flows: redraw as a schematic.

## Whole-deck pass (after all slides)
- Page through all slides (a contact sheet or a series of reduced-size views helps) looking for: inconsistent colours or line styles, notation drift, repeated or contradictory titles, uneven density, sections without dividers, missing legends, tags that should be on a slide but are not.
- Check the order tells a story: each title, read in sequence, should form an argument.
- Confirm cover has only the title; page numbers are continuous; footer section labels match the outline.
- Look again at slides you fixed early, since later changes in shared components may have shifted them.

## Report
Summarise in the final message: slides reviewed, defects found and fixed, anything left (with the slide number), and the items the user must confirm (tagged numbers, ambiguous sources).
