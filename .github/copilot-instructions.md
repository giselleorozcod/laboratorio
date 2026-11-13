# Copilot Instructions

## Project shape

- `index.html` is the complete client-side application: Spanish-language HTML, inline CSS, and plain JavaScript. D3.js v7 is loaded from a CDN; there is no framework, package manifest, or build pipeline.
- Data flows from the two file-upload controls through `setupFileListeners()` and `processRawData()` into the shared `state`, then through `app.initGraph()` and `app.renderGraph()`.
- Graph layout is separated into `layoutNetwork()`, `layoutBipartite()`, and `layoutEgo()`. These operate on the shared graph state; `app` owns rendering and UI interactions.

## Data format

- The Points CSV is positional: column 1 is the node ID, column 2 is its category, and optional column 3 is its numeric weight. Additional columns are retained as custom attributes.
- The Links CSV is positional: columns 1 and 2 are source and target node IDs, column 3 is optional weight, column 4 is optional link type, and column 5 is optional direction. Both endpoints must exist in Points; invalid rows are skipped and reported in the UI.
- CSV parsing accepts comma or semicolon delimiters and trims cell values. Keep the two upload inputs in sync; graph processing starts only after both files are loaded.

## Build, test, and lint

- There are no configured build, test, or lint commands.
- To serve the page locally from the repository root, run `python -m http.server 8000`, then open `http://localhost:8000`.
- For a focused manual smoke test, load `data/Points*.csv` and `data/Links*.csv` using the two upload controls. Confirm the graph renders, switch among Red / Bipartito en Columnas / Red Ego, and exercise category filtering and node selection. Browser DevTools can be used to inspect runtime errors.

## Code conventions

- Keep changes in `index.html` unless the app is intentionally being split up. Its numbered HTML/CSS/JavaScript sections mark the existing organization: configuration and state, CSV parsing, graph model/scales/layouts, D3 rendering, UI, and startup.
- The graph data model is positional and keyed by node `id`; preserve endpoint IDs when changing parsing or joins. D3 selections use keyed data joins for nodes and links.
- UI controls call the global `app` object through inline handlers and element IDs. Keep handler names, IDs, and `state` fields aligned when changing controls.
- The UI text and CSV warnings are in Spanish; follow that language for user-facing additions.
