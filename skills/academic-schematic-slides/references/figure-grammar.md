# Figure grammar: how to draw schematic figures

Target: the look of Figure 1 in a top journal or conference paper. Most slides are figures like this. The user explicitly dislikes "a card with text, an arrow to the next card". Everything below exists to prevent that.

Calibrate by opening `assets/samples/03_architecture.png`, `09_card_vs_schematic.png` and `elements.png` before drawing.

## 1. The central rule

> **Boxes hold operations. Data is drawn as what it looks like.**

Ask for every thing in the figure: "what would this look like if I could see it?" Draw that. Only the *operation* between two pieces of data may be a small marker, a narrow strip, a labelled stack, or a circle on the connector; if it must be a box, make it a stack of slabs, a node graph or a formula, not a sentence.

### The label-removal test
Cover or delete every text label. Can you still tell: what goes in, what comes out, which part is a network, which part is frozen, which part exists only in training? If not, it is a dull card diagram. Redraw it with data pictures.

### Anti-pattern (forbidden)
- One white rounded card per step, the step name as text inside, arrows chaining them in a row or column.
- Cards of identical size standing for unlike things (an image, a network, a loss, a number).
- A figure that is mostly sentences; bullet lists posing as diagrams.
- Icon-per-card grids (a clip-art icon above a sentence in a rectangle) as the default way to show "five points". If the content is a set of parallel claims, draw one meaningful picture, or a row of simple drawn objects with one-word labels and no enclosing cards.

## 2. Vocabulary: how data looks

| Data | Draw it as |
|---|---|
| Photo / input image | thumbnail (real image if supplied, else a synthetic scene), 1.2 px ink border |
| Depth, saliency, heat, probability, error map | colour-mapped raster, same map as target; colour bar with range **and unit** beside it |
| Segmentation / mask | flat-colour regions or binary mask (dark on light) |
| Feature vector / embedding | a row (or column) of coloured cells, varying tint of one hue |
| Two embeddings in a shared space | two vectors with different hues (blue / orange-red) next to each other |
| Token / feature map | grid of tinted cells (4-6 rows x 6-10 cols) |
| Network backbone, encoder, transformer | stack of slabs with slight perspective (frozen / trained / training-only fill) |
| MLP / small head | node-and-edge graph, 3-4 layers, 3-9 nodes per layer |
| Text / caption | small document icon: rounded rect with 3 bars (shorter last bar); optionally a short quote |
| Scalar, parameter, score | small card with a coloured header strip and the typeset symbol |
| Loss / objective | circle node with the typeset symbol, placed where its two inputs meet |
| Operation on a link (sigmoid, exp, pooling, concat) | small circle on the connector with the symbol, or a narrow labelled strip |
| Time series / signal | polyline on small axes |
| Distribution / histogram | bars or a filled curve on small axes |
| Point cloud / 3D data | dots with depth-based size and colour |
| Embedding space / clusters | small axes with coloured dot clusters |
| Graph / network data | nodes and edges; node size and colour encode a variable |
| Table of records | a few drawn rows of coloured cells, not text |
| Molecule / sequence / cell / spectrum etc. | draw it as the field draws it (ball-stick, base-pair strip, micrograph thumbnail, spectrum with peaks) |
| Unknown / unmeasured | the object drawn with a dashed outline and a purple `to be confirmed` tag |

If the user gives real figures or images, use them (cropped, with the same frame and colour-bar rules) in place of synthetic ones.

## 3. SVG recipes

All snippets use the palette in `visual-system.md`. Mix a tint of hue H at strength t with white as `H*t + white*(1-t)`.

**Thumbnail (raster, embedded)** - generate a small image (e.g. 320x240) with numpy/Pillow or any renderer, encode as PNG data URI:
```
<image x='96' y='546' width='170' height='128' preserveAspectRatio='none' href='data:image/png;base64,...'/>
<rect x='96' y='546' width='170' height='128' fill='none' stroke='#1F2430' stroke-width='1.2'/>
```
Synthetic scene options: ray-cast boxes in a room (floor checker, walls, 3 coloured boxes) gives an RGB image *and* a consistent depth map; or a few polygons for a vector scene. A vector-only heat map can be a grid of 30-50 columns of `<rect>` cells coloured from the colour map (set `shape-rendering='crispEdges'`).

**Colour bar** (viridis, bright at top):
```
<defs><linearGradient id='cb' x1='0' y1='1' x2='0' y2='0'>
 <stop offset='0' stop-color='#440154'/><stop offset='0.25' stop-color='#31688e'/>
 <stop offset='0.5' stop-color='#1f9e89'/><stop offset='0.75' stop-color='#6ece58'/><stop offset='1' stop-color='#fde725'/>
</linearGradient></defs>
<rect x='1760' y='554' width='12' height='112' fill='url(#cb)' stroke='#1F2430' stroke-width='1'/>
<text x='1780' y='571' font-size='20' fill='#5F6773'>1</text><text x='1780' y='660' font-size='20' fill='#5F6773'>0 m</text>
```
Use all stops of the map in the real thing; a 5-stop version is shown for brevity.

**Feature vector** (n cells, tints vary, never all identical):
```
for i in range(n):  fill = mix(white, HUE, 0.15 + 0.8*pseudo_random(i))
<rect x='{x+i*cw}' y='{y}' width='{cw}' height='{h}' fill='{fill}' stroke='#FFFFFF' stroke-width='0.8'/>
then one outline: <rect x y width height fill='none' stroke='{HUE}' stroke-width='1.3'/>
```
Token grid: same, two nested loops.

**Layered stack with perspective** (front face `w x slab`, depth offset `dx=26, dy=14`; draw slabs from bottom to top so upper ones overlap lower ones; per slab three shapes):
```
top  : <polygon points='x,ys  x+dx,ys-dy  x+w+dx,ys-dy  x+w,ys' fill='{light}' stroke='{S}' stroke-width='1.2'/>
side : <polygon points='x+w,ys  x+w+dx,ys-dy  x+w+dx,ys+slab-dy  x+w,ys+slab' fill='{darker}' stroke='{S}' stroke-width='1.2'/>
front: <rect x='x' y='ys' width='w' height='slab' fill='{F}' stroke='{S}' stroke-width='1.2'/>
ys = y + i*(slab+gap);  slab 26, gap 12, 3-6 slabs; light = F mixed 55 % with white; darker = F mixed 35 % toward S
```
Frozen: F `#E9EDF4`, S `#8491B4` + snowflake (three crossing lines, r 10) at the top-right. Trained: F white, S primary blue. Training-only: F `#E3F4F0`, S teal, dashed `7 5`. Encoder/decoder pairs: stacks of different height.

**MLP node graph**: layers (e.g. 3,5,5,2) laid out in columns, node radius 8-9, nodes of each layer centred vertically with step `(h-2r)/(max_layer-1)`; edges between every node pair of adjacent layers as `<line stroke-width='0.9' opacity='0.45'>` drawn first, nodes on top (`fill white`, 1.6 px outline). For a small head in a pipeline use 3-4 layers; for an enlarged inset use bigger spacing and the same layer sizes.

**Scalar / parameter card**: `<rect rx='6' fill='#fff' stroke='{S}' stroke-width='1.6'/>` plus a 8 px header strip `<rect height='8' rx='3' fill='{S}'/>` and the typeset symbol centred below the strip.

**Loss node**: `<circle r='40' fill='#fff' stroke='{teal}' stroke-width='1.8' stroke-dasharray='7 5'/>` with the symbol centred; arrows from the two compared quantities end at its edge (not at its centre).

**Operation marker on a link**: draw the arrow first, then `<circle r='22' fill='#fff' stroke='{S}' stroke-width='1.6'/>` over it with the symbol (sigma, exp, concat) inside. The line appears to pass through the operation.

**Axes / mini plot**: L-shaped polyline (2 px ink), dots r 8, curve as polyline 2.6 px. For a point cloud compute pseudo-3D positions and map depth to radius and colour-map value.

**Caption icon**: rounded rect (1.6 px outline) and three horizontal bars, widths 70/70/45 % at opacity 0.55.

**Zoom-in connector**: dashed rectangle (`4 4`, 1.4 px, muted) around the small source object; two dashed lines from its two bottom (or right) corners to the two matching corners of the enlarged panel; the enlarged panel carries the next panel letter and shows the same component enlarged *in the same visual language*, with the interior detail that is hidden at slide scale (layer sizes, input vector, outputs).

## 4. Spatial language

1. **Zones**: tinted rounded panels (radius 12, 1.5 px border) group what belongs together: inference path (blue-tinted), training-only (teal tinted, dashed border), removed (grey dashed), neutral (grey). Elements sit inside their zone, not across borders, except connectors.
2. **Fixed semantics** (see `visual-system.md`): frozen, trained, training-only, inference path, removed. Same look on every slide, and a legend.
3. **Layout**: training branches **above**, the inference path **below**, so "inference is only the bottom row" is visible at a glance. Data flows left to right. Connectors are straight or orthogonal; **arrows never run through labels or other elements**, leave at least 12 px clearance and route around.
4. **Symbols on the figure**: put typeset symbols directly on the tensor, vector or arrow they name (the image `x`, the map `\hat{y}`, the scalar `s`), and loss symbols inside loss nodes. Do not create a separate symbol glossary column.
5. **Multi-panel**: a, b, c inside one slide when concept, architecture and enlarged detail belong together. Each panel still holds little.
6. **Labels**: 22-24 px muted, directly under (or beside) the object, centred; one to three words. Never inside the drawing of a network.
7. **Rhythm**: align rows on common centre lines; equal horizontal gaps; objects of the same kind have the same size on every slide.
8. **Compare methods with the same grammar**: "ours" and "prior" use the same components, sizes and colours so the only visible differences are the real differences; mark a bottleneck with an orange-red zone/pill.
9. **Whitespace**: leave empty areas rather than filling them with text; but avoid large dead blocks inside panels (shrink the panel or enlarge the figure).

## 5. Step-by-step build-up

For architectures and mechanisms with more than about 6 elements, split into several slides. **Each slide adds exactly one idea; the base drawing never moves.** Typical order: (1) input and backbone, (2) add the head and output (and an enlarged inset), (3) add the training-only branch, (4) add losses and targets, (5) the inference view (training parts greyed with a cross, or removed). Reserve empty, dashed placeholder panels for parts that come later so the page does not reflow. Titles can be numbered ("Step 2: ..."). See `assets/samples/04_build_step2.png` and `05_build_step3.png` (same base, one more panel).

## 6. When the content is naturally a pipeline or flow

Still schematic: use pictures of the data at the nodes (thumbnails, vectors, grids), stacks or graphs for processing, narrow strips or circles with symbols for small operations, zone backgrounds for stages, and a legend. Time-based content (timeline, protocol) uses a drawn axis with marks and small pictograms of the objects involved, not equal cards. If an operation truly needs a name, put the name under a stack or graph in 22 px muted text.

## 7. Same content, card version vs schematic version

**A. Training/inference pipeline** (see `assets/samples/09_card_vs_schematic.png`)
- Card version: five equal white cards "Input image / Frozen encoder / Feature extraction / Small prediction head / Output map" in a column with arrows, plus two more cards for an auxiliary branch.
- Schematic version: image thumbnail, a frozen slab stack with a snowflake, a token grid, a node-graph head, a colour-mapped output with a bar; the auxiliary branch sits in a teal dashed zone above, linked by a dashed arrow to the head and ending in a circle loss node; a legend names frozen / trained / training-only.

**B. Concept: "same pixels, different sizes"**
- Card version: boxes "Camera -> Image -> Ambiguity".
- Schematic version: a side view with a camera, a ray, and two houses at different distances and sizes that project to the same image segment; a small 'image plane' bar showing equal projected height; one pill states the point.

**C. Comparison of methods**
- Card version: a table with rows of sentences ("Uses text at test time: yes/no").
- Schematic version: one panel per method drawn with the *same* components (input, encoder stacks, head graph, output); the extra component of the competitor is drawn in an orange-red zone with its cost as a small pill; ours has none. For many methods x many attributes only, a matrix of filled/hollow dots with a legend is acceptable.

**D. Mechanism: "two ways of training"**
- Card version: "Fine-tune all layers" and "Train head only" boxes with bullet text.
- Schematic version: two panels with identical stacks; one with an "updated" mark on every slab, the other with snowflakes on the slabs and a mark only on the head; a bar below each showing the trainable-parameter budget (`assets/samples/06_concept_two_approaches.png`).

## 8. Do / Don't summary

| Do | Don't |
|---|---|
| Draw data as pictures (thumbnails, maps, vectors, stacks, graphs, curves) | Put data names in text boxes |
| Use zones, fixed colours and line styles for roles | Use colour randomly or box everything alike |
| Put training above, inference below, flow left to right | Let arrows cross labels or each other |
| Typeset symbols on the figure itself | List symbols in a glossary column |
| Enlarge hidden detail in an inset linked by dashed lines | Cram internal detail into the main drawing |
| Split complex figures across slides, base fixed | Draw 25 elements on one slide |
| One legend with only the used entries | Leave semantics unexplained |
| Colour bar with range and unit on every colour map | Show unlabeled colour maps |
| Same size/look for same kind of object everywhere | Vary sizes arbitrarily |
| Leave whitespace | Fill space with sentences |
