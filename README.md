# Interactive Cake Studio

A single-file, browser-based 3D cake-cutting studio. An assorted eight-wedge mousse cake on a white plate in a lavender studio: pick a knife, press it into the cake, open a real layered cross-section, then lift the slice onto a serving plate.

Everything (HTML, CSS, JavaScript, GLSL shaders, configuration and procedural assets) lives in `index.html`. Three.js r160 loads from jsDelivr.

## Run it

- Open `index.html` in a current desktop Chrome, Edge, Firefox or Safari. An internet connection is needed on first load for Three.js (and the Google Fonts, which fall back to system fonts).
- If your browser blocks module scripts on `file://`, serve the folder instead: `npx serve .` or `python3 -m http.server`, then open the printed address.

## Controls

| Action | Mouse / touch | Keyboard |
| --- | --- | --- |
| Cut | Drag across the cake, release to slice | Arrow keys move the keyboard path, Shift+↑/↓ tilts, Enter cuts |
| Tools | Tool grid in the panel | 1–6 |
| Orbit / zoom | Right-drag, wheel, two-finger pinch | V resets the view |
| Serve | Cake server: click a piece, click the plate | N selects, Enter lifts/places, R rotates |
| Fork | Press to dent, drag to nudge | |
| History | Panel buttons | Ctrl+Z, Ctrl+Shift+Z |
| Pause / photo | Panel buttons | Space, P, Esc |

## How the main systems work

- **Cutting** is exact geometry. Each piece is a connected set of convex cells. A cut is a (tiltable) plane limited to the blade's length and the chosen depth. Cells are split by those limits without a gap and by the blade with a 4.5 mm kerf; each new face is capped, and cells that still touch stay one piece. A cut that stops short of the base or the edge leaves a real slit with the piece still attached.
- **Layers** are coloured in the fragment shader from each wedge's layer table, so every new face shows the correct mousse, jelly, cream, ganache, sponge or crumb.
- **Soft-body response** uses per-piece damped springs (shear jiggle, squash, knife-pressure dent, fork press) that drive a shared vertex displacement in the cake, sprinkle and shadow shaders, mirrored on the CPU so toppings ride the surface.
- **Serving** moves pieces kinematically with separating-axis collision on each cell's footprint, so pieces never overlap or sink into a plate.

## Approximations and limits

- Deformation is a spring-damper vertex displacement, not a volumetric simulation. Picking and cutting use the undeformed shape.
- Cuts are straight planes (tilt up to ±30°). Curved cuts are not supported.
- Whole toppings are never split: in the blade's path they are pushed aside or fall off. Sprinkles on the blade line are knocked loose as crumbs.
- Pieces are placed rather than simulated as rigid bodies, so they cannot be stacked or knocked over. Crumbs and fallen toppings use simple gravity with ground contact.
- Complexity is capped at 64 pieces and 320 cells; undo keeps the last 30 steps.
- Sound is synthesised with Web Audio and starts after the first click.
- In sandboxed viewers the PNG download may be blocked; the export dialog also shows the image for saving by hand.
