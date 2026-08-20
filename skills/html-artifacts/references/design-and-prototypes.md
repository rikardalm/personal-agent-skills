# Design & Prototypes

HTML is the medium design ships in, which makes it the natural format for talking *about* design — and the only honest way to prototype motion. House style: `../design.md`; token block in `matching-your-style.md`.

## Living design system

For "show me our design tokens."

A section per category — colours, type, spacing, radii, shadows, motion — each token **rendered as itself**: colours as swatches, type as text at the real scale, spacing as visible gaps with labels, shadows on a card. A `#0070f3` next to its swatch beats the hex alone.

Every specimen gets a copy button (value on click, token name on shift-click). The artifact is half reference, half tool, and the copy action is the point.

Pull from the codebase — read the Tailwind config or theme file rather than inventing plausible tokens. If there's nothing to read, the system to render is the house style in `../design.md`; `../design-reference.html` is what it looks like built.

**Load-bearing:** source-of-truth fidelity. An artifact that diverges from the real tokens is worse than nothing.

## Component variants sheet

For "show me every variant of our button."

One component, every state on one page: sizes × intents × states, grouped by axis — a row per size, columns per intent. Render the real component, not a screenshot. Props underneath each variant.

For buttons that means **both shapes the house style defines** — the marketing pill and the 6px app square — since mixing them is exactly the mistake the sheet exists to prevent.

**Avoid** skipping the weird states (loading, empty, error) — those are the ones design systems forget and engineering ad-libs. And one component per sheet; multi-component pages become a tour, which is a different format.

## Animation prototype

For "let me play with this transition before wiring it in."

**Layout**
- The thing being animated, isolated and centred in a hairline card.
- A `controlbar` with a control per parameter that matters — duration, delay, easing, distance. Live update as the user drags.
- A replay button. One-shot animations are useless for tuning.
- A `sidecard` holding the **live code output** — current parameters as CSS or framer-motion config, updating with the sliders — and a copy button.
- An easing-curve graph beats a dropdown of names; users tune curves visually.

**Load-bearing:** the live code output. Without the copy step this is a demo, not a tool that graduates into the codebase.

**Avoid** building a generic animation playground. Stay scoped to the one transition asked about.

## Clickable flow

For "does this multi-screen sequence feel right."

3–6 screens in order, real next/back buttons, and a thumbnail tray so the user can jump — they'll want to compare screen 1 against screen 4 directly. Just enough fidelity to test the shape; render the fields the flow turns on, not every field. Pixel-perfect is a different artifact.

## Sketch

The stage is the only extra rule; everything else is the token block.

```css
.stage{display:grid;place-items:center;min-height:14rem;
       background:var(--hairline-soft);border-radius:var(--r-md)}
```

```html
<!-- a prototype is an app surface: 6px squares, pill only for the copy-out -->
<div class="figrow">
  <section class="card inset">
    <p class="eyebrow">Stage</p>
    <div class="stage">
      <button class="btn btn-primary" id="target">Place order</button>
    </div>
    <div class="controlbar">
      <label>Duration <input type="range" id="dur" min="100" max="2000" value="400"></label>
      <label>Easing
        <select id="ease">
          <option>cubic-bezier(.2,.8,.2,1)</option>
          <option>ease-out</option>
        </select>
      </label>
      <span class="readout" id="readout">
        <div><span class="v">400 ms</span><span class="k">duration</span></div>
      </span>
    </div>
    <figcaption>Replay re-triggers it — one-shot animations can't be tuned.</figcaption>
  </section>

  <aside class="card inset sidecard">
    <p class="eyebrow">Current values</p>
    <!-- live output is the reason this artifact exists -->
    <pre id="code">transition: transform 400ms cubic-bezier(.2,.8,.2,1);</pre>
    <dl><dt>Distance</dt><dd>12 px</dd><dt>Delay</dt><dd>0 ms</dd></dl>
    <button class="btn btn-ghost-sm" id="replay">Replay</button>
    <button class="btn btn-primary" id="copy">Copy CSS</button>
  </aside>
</div>
```
