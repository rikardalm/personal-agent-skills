# Exploration & Planning

For when the user is deciding something and needs to weigh options, or has decided and needs a plan dense enough to hand off. House style: `../design.md`; token block and patterns in `matching-your-style.md`.

## Option comparison

The single biggest win HTML has over markdown. Three approaches in markdown is three sequential sections the reader holds in their head. In HTML it's a **criteria matrix**: criteria down the side, options across the top, so one vertical scan shows which column is losing and on what.

**Layout**
- One hairline card holding the matrix. Criteria as mono row labels, options as `h3`-scale column heads.
- A hero row first — the number that decides it — with a rail under each figure so the gap is a shape before it's a digit.
- Pro/con as a two-column-per-cell list, not prose. Bullets read as a sequence; a matrix reads as a comparison.
- The recommendation takes `--hairline-soft` plus an accent-blue eyebrow. Never an inverted black column at this density.
- A `decision` block below that picks one. Not "it depends" — actually pick.

**Load-bearing**
- Identical criteria across all columns. A missing cell reads as a weakness, not an omission.
- Hard numbers. They force the recommendation to be defensible.

**Avoid**
- Code in one block at the top with the comparison underneath. The artifact *is* the comparison.
- Three approaches that share 80% of their code. That's one approach with parameters.

## Visual design exploration

For "I'm not sure what direction to take this UI." Generate 4–6 *meaningfully different* directions in a grid — vary layout, information hierarchy, and interaction model before surface treatment. Each mockup is real rendered HTML with a caption naming the tradeoff it makes.

**Avoid** six variations of one layout with different colours, and six that all look like the default dashboard. One should feel almost wrong, to anchor the others.

## Implementation plan

For handing work to an implementer, human or agent.

**Layout**
- Problem statement, then a milestones strip — a visual timeline, not a numbered list.
- A data-flow diagram (inline SVG). Don't skip it: past two components, prose can't convey topology.
- The 2–3 load-bearing code snippets, annotated on the tricky lines.
- A risk table: risk / likelihood / mitigation. Risks in prose disappear; risks in a table get addressed.
- A "what we're explicitly not doing" section. It prevents scope creep before it starts.

**Avoid** listing every file that will be touched — the reader needs the shape of the change.

## Sketch

```html
<main class="container">
  <header class="masthead">
    <div class="mesh" aria-hidden="true"></div>   <!-- the only colour on the page -->
    <p class="eyebrow">RFC-0114 · Rev B</p>
    <h1>Three ways to debounce search</h1>
    <p class="dek">Prompt that produced this · which option I'd pick</p>
  </header>

  <section>
    <p class="eyebrow">01 — Options <span class="src">exploration-and-planning.md</span></p>
    <h2>Option matrix</h2>

    <div class="card">
      <table class="matrix">
        <thead>
          <tr>
            <th></th>
            <th><p class="eyebrow">Option A</p><span class="opt">Inline useEffect</span></th>
            <th class="pick"><p class="eyebrow">Option B · Recommended</p>
                <span class="opt">useDebounce hook</span></th>
            <th><p class="eyebrow">Option C</p><span class="opt">Debounce server-side</span></th>
          </tr>
        </thead>
        <tbody>
          <!-- hero row: the number, then a rail so the gap is a shape first -->
          <tr>
            <th>Bundle</th>
            <td class="cell"><span class="v">0<span> kb</span></span>
                <span class="rail"><i style="width:4%"></i></span></td>
            <td class="cell pick"><span class="v">0.4<span> kb</span></span>
                <span class="rail"><i style="width:18%"></i></span></td>
            <td class="cell"><span class="v">0<span> kb</span></span>
                <span class="rail"><i style="width:4%"></i></span></td>
          </tr>
          <tr>
            <th>Trade</th>
            <td><ul><li class="y">Zero abstraction</li><li class="n">Logic duplicated</li></ul></td>
            <td class="pick"><ul><li class="y">One caller, one line</li>
                <li class="n">A hook to maintain</li></ul></td>
            <td><ul><li class="y">No client change</li>
                <li class="n">A round trip per keystroke</li></ul></td>
          </tr>
          <tr><th>Risk</th>
              <td><span class="tag low">Low</span></td>
              <td class="pick"><span class="tag med">Medium</span></td>
              <td><span class="tag high">High</span></td></tr>
        </tbody>
      </table>
    </div>

    <div class="decision">
      <p class="eyebrow">Decision</p>
      <p>Go with B — here's why…</p>
    </div>
  </section>
</main>
```
