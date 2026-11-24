# Visual system

Goal: any model that follows this file produces the same look: white page, thin lines, low-saturation journal palette, one fixed visual language. Values below are for a 1920x1080 canvas (1 SVG unit = 1 px).

## Principles
- White background, generous whitespace. Thin strokes (1.2-2.4 px), small corner radii.
- **No** drop shadows, **no** decorative gradients, **no** heavy frames, **no** emoji, **no** clip-art icons with multiple colours. A gradient is allowed only when it *is data* (colour maps, colour bars).
- Colour carries meaning (table below). Orange-red is rare and only for emphasis.
- Everything aligns: shared baselines, equal gaps, panels on the same grid.

## Palette (Nature-inspired, low saturation)
| Role | Hex | Use |
|---|---|---|
| Ink | `#1F2430` | titles, body text, axes |
| Muted | `#5F6773` | captions, labels, footers |
| Primary blue | `#3C5488` | trained modules, inference path, main data series |
| Primary tint | `#E8ECF5` / zone `#EEF1F8` (border `#B5C0DA`) | light fills of the main path |
| Accent orange-red | `#E64B35` (dark `#B8321F`, tint `#FBE7E3`) | emphasis, bottleneck, risk, "ours" in charts. At most one or two per slide |
| Teal | `#00A087` (text `#00806C`, tint `#E3F4F0`, zone border `#8FD1C3`) | **training-only** parts and losses |
| Purple | `#7B5EA7` (tint `#F0EBF7`) | honesty tags only |
| Frozen blue-grey | fill `#E9EDF4`, stroke `#8491B4` | frozen / pretrained modules |
| Grey | fill `#F0F0EE`, stroke `#A3A5A8`, text `#777A80` | removed, disabled, baselines, background elements |
| Neutral zone | fill `#F6F7F9`, border `#D9DDE4` | plain panels |
| Neutral link | `#7A8291` | arrows with no semantic (anti-pattern demos, generic links) |

Chart series: grey `#A3A5A8`, blue-grey `#8491B4`, deep blue `#3C5488`, orange-red `#E64B35` for the highlighted method. Never rely on colour alone: also use direct labels, hatching, markers or position. Add a darker variant (`#2B3D66`) only if a dark fill needs contrast.

### Colour maps for data (colour-blind friendly, perceptually ordered)
- viridis (10 stops, dark to bright): `#440154 #482878 #3e4989 #31688e #26828e #1f9e89 #35b779 #6ece58 #b5de2b #fde725`
- cividis (11 stops): `#00204d #00306f #31446b #4d5a6c #666970 #7f7c75 #958f78 #aea374 #cbba69 #e6d25a #ffea46`
- For a single-hue magnitude (error, density) use white to `#E64B35` or white to `#3C5488`.
- Never use rainbow/jet. The same quantity uses the same map on every slide; a prediction and its target share one map and range.
- Interpolate linearly between stops for rasters; for a vertical colour bar use an SVG `linearGradient` with these stops.

## Typography
Font stack (put it in every SVG; do not depend on one platform font):
```
font-family: Inter, 'Helvetica Neue', Helvetica, Arial, 'Liberation Sans', 'DejaVu Sans', sans-serif
```
Before rendering, confirm which family actually resolves on the machine (font listing or measure in a browser) and that glyphs such as digits, Greek letters used in plain text, `±`, `·`, `→` exist. If a glyph is missing, draw it as maths or as a path.
Set on the root: `font-kerning:none; font-variant-ligatures:none; text-rendering:geometricPrecision` so measured widths match rendered widths.

| Element | Size (px) | Weight | Colour |
|---|---|---|---|
| Slide title | 46 (shrink to 40 if it would exceed one line) | 700 | ink |
| Cover title | 84-90 (subtitle 52, regular) | 700 | ink |
| Section title | 100-112 | 700 | ink |
| Panel title | 26 | 700 | muted; teal text for training-only, primary for inference |
| Panel letter | 34 | 800 | ink |
| Module/figure label | 22-24 | 500 | muted |
| Text inside a box or pill | 26-30 | 600 | ink |
| Body/caption sentence | 26-34 | 500 | ink or muted |
| Chart tick labels | 24 | 500 | muted |
| Value labels on bars | 22-26 | 600 | ink |
| Source / footnote | 20 | 500 | muted |
| Section label (bottom-left) | 22 | 500 | muted |
| Page number | 26 | 600 | muted |

Minimum text size anywhere: 20 px. Titles: short declarative sentences, sentence case, no final full stop ("Training uses text; inference does not").

## Page grid (chrome)
- Margins: left/right **96 px** for text; wide panels may reach 72-1848.
- Title baseline at **y = 104**, left x = 96.
- Content area: **y 150 to 940**.
- Legend row: baseline y = 978 (swatch height 28, label 22 px muted), x = 96.
- Source note: right-aligned at x = 1824, y = 972, 20 px muted.
- Footer: section label at (96, 1038); page number right-aligned at (1824, 1038). No page number on the cover.
- Cover: vertical accent bar (x 120, width 14, rounded, orange-red, about 290-400 tall) left of the title; title at x = 178.

## Strokes, radii, spacing
| Item | Value |
|---|---|
| Box / module outline | 1.6-1.8 px, radius 8 |
| Zone (panel) outline | 1.5 px, radius 12 |
| Pill / tag | 1.4-1.6 px, radius = half height |
| Layer slab edges | 1.2 px |
| MLP edges | 0.9 px at opacity 0.45; node outline 1.6 px |
| Arrow | 2.4-2.8 px (primary) / 2.0 px (neutral); arrowhead length about 12 + 2.2 x stroke width |
| Zoom connector | 1.2-1.4 px dashed `4 4` in muted |
| Axis lines | 2.4 px ink; ticks 2 px, length 10 |
| Chart lines | 3.2-3.6 px; markers: r 6, white fill, 3 px coloured outline |
| Dash styles | training-only `7 5` (boxes/zones) and `8 6` (arrows); removed `7 5` / `7 6` |
| Gap between panels | 24-30 px; inner padding of panels 24-28 px |
| Gap between flow elements | 50-90 px so arrowheads (about 17 px) are visible |

## Fixed semantic language (never change within a deck)
| Meaning | Look |
|---|---|
| Frozen / pretrained | fill `#E9EDF4`, stroke `#8491B4`, small snowflake mark at the top-right |
| Trained | white fill, primary-blue outline |
| Inference path | solid primary-blue arrows, zone `#EEF1F8` |
| Training only | teal dashed outline, teal tint zone `#E3F4F0`, dashed teal arrows |
| Removed at inference | grey dashed outline/fill `#F0F0EE`, dashed grey arrows (optional red-brown cross `#9A3B3B`) |
| Emphasis / bottleneck | orange-red pill or outline with tint `#FBE7E3` |
| Honesty tag | purple pill, 700 weight, text `unverified` / `estimate` / `to be confirmed` / `illustrative` |
| Loss / objective | circle node (white, 1.8 px outline, teal dashed if training-only) placed where the signals meet |
| Operation on a link | small white circle (r about 22) with the symbol, sitting on the connector |

Every slide that uses these shows a **legend** (only the entries it uses).

## Panels and labels
- Multi-panel figures: bold lowercase **a, b, c** (34 px, weight 800) at the top-left inside each panel, followed by the panel title (26 px bold) if any. Letters run left-to-right, top-to-bottom. Single-panel slides may omit the letter, but be consistent within the deck.
- Panel title colour follows the semantic language (teal training-only, blue inference, muted neutral, grey removed).
- Zoom-in insets are normal panels with the next free letter, linked by dashed lines (see figure-grammar).
- Training branches go **above** the inference path; data flows **left to right**; the input is at the left.

## Chart conventions
- White background, axis lines only (no grid, no box). Y ticks every round value; unit in the axis caption or panel title.
- Bars: no outlines, width 88-100, gap 12-14 inside a group; value labels above bars (above the error bar if present).
- Always say what error bars are (s.d., s.e., 95 % CI) and n. Say "lower/higher is better" in muted text.
- Highlight "ours" with orange-red or primary blue, baselines grey. Direct-label lines instead of using a legend where possible.
- Source note bottom-right; tag invented numbers `illustrative data`.

## SVG boilerplate
```
<svg xmlns='http://www.w3.org/2000/svg' width='1920' height='1080' viewBox='0 0 1920 1080'>
<style>text{font-family:Inter,'Helvetica Neue',Helvetica,Arial,'Liberation Sans','DejaVu Sans',sans-serif;
font-kerning:none;font-variant-ligatures:none;text-rendering:geometricPrecision}</style>
<rect width='1920' height='1080' fill='#FFFFFF'/>
<!-- content -->
</svg>
```
Self-contained: embed rasters as `data:image/png;base64,...`, embed maths as paths, no external files, no `@import`.
