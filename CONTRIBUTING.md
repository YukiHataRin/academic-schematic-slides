# Contributing

Thanks for helping. Small, focused pull requests are easiest to review.

## Good contributions

- New sample slides (timeline, qualitative results, summary, other scientific domains).
- Component vocabularies for other fields, added to `references/figure-grammar.md`.
- Verified rendering routes (MathJax, matplotlib mathtext, `rsvg-convert`, `cairosvg`, `inkscape`) with notes on what worked and on which platform.
- Translations of the README.
- Bug reports and clarity fixes for the reference files.

## Guidelines

- The skill lives in `skills/academic-schematic-slides/`. Keep `SKILL.md` short; detail belongs in `references/`.
- Samples must be self-contained SVG (embedded rasters, maths as paths, no external files) plus a 1920x1080 PNG with the same base name, named `NN_slug`.
- Samples follow the skill's own rules: schematic-first, one point per slide, typeset maths, fixed semantic colours and line styles.
- Invented numbers or scenes must carry an honesty tag (`illustrative`, `synthetic example scene`, ...).
- Look at the rendered PNG before opening a pull request, as `references/review.md` describes.
- Do not include personal data, private paper titles, or local file paths in samples.

## Reporting a problem

Open an issue with the prompt you used, the agent and version, and the rendered PNG if the problem is visual.
