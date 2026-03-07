# SVG → G‑Code Web Converter

Single‑file web tool that converts an SVG shape into G‑Code for CNC applications — originally built for a **styrofoam hot‑wire cutter** made from a repurposed 3D printer.

**Live:** <https://monsieurhulotbot.github.io/svg2gcode-web/>

## Features

- **Any SVG shape**: Supports `<path>`, `<rect>`, `<circle>`, `<ellipse>`, `<line>`, `<polyline>`, `<polygon>` — including transformed (rotated, scaled, etc.) elements.
- **Adaptive sampling**: Curves get dense points where needed; straight segments stay lean. Configurable tolerance parameter instead of fixed point count.
- **Smart start point**: For closed paths, the starting point is rotated to the one nearest the CNC origin — minimizing the initial rapid move (important when the tool can't be retracted, e.g. a hot wire).
- **Generic axis mapping**: Freely map SVG X/Y to any CNC axis (X/Y/Z) with per‑axis invert option.
- **Visual preview**: SVG shape centered in viewport with CNC coordinate arrows showing axis directions at the CNC zero point.
- **Configurable**: Feed rate, decimal precision, G‑Code header/footer, output extension (`.gcode`/`.nc`).
- **Convenience**: Copy to clipboard, direct link to NCViewer for 3D preview.
- **Settings persistence**: Axis mapping stored in cookies.
- **Zero dependencies**: Pure HTML/CSS/JavaScript — runs in any modern browser, no build step.

## Usage

1. Open `index.html` in a browser (or use the [live version](https://monsieurhulotbot.github.io/svg2gcode-web/)).
2. Upload an SVG file containing **exactly one shape element**.
3. Adjust axis mapping, tolerance, feed rate, header/footer as needed.
4. Click **Generate G‑Code**, then download or copy.

## How it works

1. **Normalize**: Any SVG shape type is converted to a `<path>`, baking in any `transform` attributes.
2. **Anchor extraction**: Path command endpoints (M, L, C, Q, A, Z, …) are parsed from the `d` attribute.
3. **Arc‑length mapping**: Anchors are located on the path's total length via binary search on `getPointAtLength()`.
4. **Recursive subdivision**: Between each pair of anchors, the midpoint is checked against the straight‑line approximation. If deviation exceeds the tolerance, the interval is split. This continues up to 12 levels deep.
5. **Start‑point optimization**: For closed paths, the point array is rotated so the point nearest CNC (0,0,0) comes first. For open paths, the direction may be reversed.
6. **Axis mapping + G‑Code output**: Each point is mapped through the configured axis mapping (with optional invert/offset), then emitted as `G0`/`G1` commands.

## License

MIT — use freely for personal or commercial projects.
