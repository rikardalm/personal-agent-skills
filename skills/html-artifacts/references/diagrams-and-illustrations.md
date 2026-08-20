# Diagrams & Illustrations

Inline SVG gives the agent a real pen. Don't fall back to ASCII or "imagine a flowchart that…" — render it. House style: `../design.md`; token block in `matching-your-style.md`.

## Figure sheet

For "make me the diagrams for this writeup."

One figure per `<figure class="card inset">` with a caption and a **copy SVG** button — the point is that the user pastes them into their real document. Consistent visual language across the set: same line weight, same arrowhead, same type. A scattered set reads as amateurish even when each figure is fine alone.

## Annotated flowchart

For "diagram our deploy pipeline."

Nodes with labels, edges with direction, branching visually distinct from the main path. Click a node to expand a side panel — the chart is navigation, the panel is content; don't cram everything onto the chart. Highlight the happy path with a **heavier ink stroke**, secondary paths **dashed in accent blue**, so the distinction survives greyscale and colourblind viewing. Legend in the corner.

**Avoid** Mermaid auto-layout hairballs (hand-place the nodes; SVG positions are just numbers), and 40-node charts — abstract rare branches into one "error handling" subgraph.

## Craftsmanship

- **`viewBox`, never fixed `width`/`height`.** Lets the figure scale.
- **Use the page tokens for ink** — `var(--ink)`, `var(--mute)`, `var(--hairline)`, `var(--link)` — or `currentColor`. Never a hard-coded hex: the figure then adapts to dark mode and matches the surrounding text for free.
- **Round numbers.** `x="120"`, not `x="119.7843"`.
- **Group with `<g>` and label it.** Someone editing by hand navigates by structure, not coordinates.
- **Type set as `<text>`,** not paths. Selectable, copyable, accessible.
- **No raster fallbacks.** A PNG of a diagram defeats the purpose.

## Sketch

```html
<figure class="card inset">
  <svg class="flow" viewBox="0 0 600 200" role="img" aria-labelledby="title">
    <title id="title">Request lifecycle</title>
    <defs>
      <marker id="arrow" viewBox="0 0 10 10" refX="9" refY="5"
              markerWidth="7" markerHeight="7" orient="auto">
        <path d="M0,0 L10,5 L0,10 z" fill="var(--mute)"/>
      </marker>
      <marker id="arrow-hot" viewBox="0 0 10 10" refX="9" refY="5"
              markerWidth="7" markerHeight="7" orient="auto">
        <path d="M0,0 L10,5 L0,10 z" fill="var(--link)"/>
      </marker>
    </defs>

    <g class="node" data-step="ingress">
      <rect class="n-box" x="20" y="78" width="120" height="44" rx="6"/>
      <text class="n-lab" x="80" y="99" text-anchor="middle">ingress</text>
      <text class="n-sub" x="80" y="113" text-anchor="middle">nginx</text>
    </g>
    <!-- happy path node: heavier ink stroke, no fill change needed -->
    <g class="node" data-step="auth">
      <rect class="n-box hot" x="200" y="78" width="120" height="44" rx="6"/>
      <text class="n-lab" x="260" y="99" text-anchor="middle">auth</text>
      <text class="n-sub" x="260" y="113" text-anchor="middle">jwt · 4ms</text>
    </g>

    <line class="edge" x1="140" y1="100" x2="192" y2="100" marker-end="url(#arrow)"/>
    <!-- secondary / return path: accent blue, dashed -->
    <path class="edge back" d="M320 122 L320 156 L80 156 L80 126"
          marker-end="url(#arrow-hot)"/>
    <text class="elab" x="200" y="150" text-anchor="middle">retry</text>
  </svg>
  <figcaption>Happy-path request flow. Click any step for details.</figcaption>
  <button class="btn btn-ghost-sm" onclick="copySvg(this)">Copy SVG</button>
</figure>
```

The classes come from the house token block:

```css
.n-box{fill:var(--canvas-elevated);stroke:var(--hairline);stroke-width:1}
.n-box.hot{stroke:var(--ink);stroke-width:1.5}      /* happy path */
.n-lab{font:500 13px var(--sans);fill:var(--ink)}
.n-sub{font:11px var(--mono);fill:var(--faint)}
.edge{stroke:var(--mute);stroke-width:1;fill:none}
.edge.hot{stroke:var(--ink);stroke-width:1.5}
.edge.back{stroke:var(--link);stroke-dasharray:4 4;stroke-width:1.5}
.elab{font:11px var(--mono);fill:var(--faint);text-transform:uppercase}
```
