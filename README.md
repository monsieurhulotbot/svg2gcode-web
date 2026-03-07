# SVG → G‑Code (Y/Z) Web Tool

Single‑file web tool that converts an SVG containing a **single path** into G‑Code suitable for CNC/plotter applications where the X‑axis is fixed and Y/Z are used for movement.

## Features

- **Strict validation**: SVG must contain exactly one `<path>` element (no other shapes).
- **Curve segmentation**: Circles, arcs, Bézier curves are sampled into linear segments.
- **Axis mapping**:
  - SVG‑X → G‑Code‑Y
  - SVG‑Y → G‑Code‑Z
  - G‑Code‑X is fixed at `X0` (unused axis)
- **Preview**: Visual path preview with coordinate display.
- **Configurable**:
  - Sampling resolution (points along the path)
  - Feed rate
  - Decimal places
  - G‑Code header/footer (customizable)
  - Output file extension (`.gcode` or `.nc`)
- **Zero dependencies**: Pure HTML/CSS/JavaScript – runs in any modern browser.

## Usage

1. Open `index.html` in a browser.
2. Upload an SVG file that contains **only one path**.
3. Adjust settings (sampling points, feed rate, etc.).
4. Preview the path.
5. Click **Generate G‑Code** and download the resulting file.

## Why Y/Z mapping?

Some CNC setups (e.g., vertical‑axis engravers, pen plotters with fixed X) treat the horizontal motion as Y and vertical motion as Z. This tool remaps accordingly.

## Technical notes

- Uses the browser’s built‑in `DOMParser` and SVG `getPointAtLength()`.
- No external libraries – the entire tool is contained in one HTML file.
- G‑Code output follows common `G0`/`G1` commands with configured feed rate.

## Example SVG

Create a simple SVG with a single `<path>`:

```svg
<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
  <path d="M10,10 L90,10 L90,90 L10,90 Z" fill="none" stroke="black"/>
</svg>
```

## License

MIT – use freely for personal or commercial projects.