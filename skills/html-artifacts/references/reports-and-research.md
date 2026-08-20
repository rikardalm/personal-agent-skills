# Reports, Research & Explainers

Recurring documents get read when they're scannable and ignored when they're walls of text. House style: `../design.md`; token block and patterns in `matching-your-style.md`.

## Concept explainer

For "explain consistent hashing to me."

**Layout**
- Masthead with the mesh, then a one-paragraph TL;DR *before* any technical content. Give away the answer.
- The core insight as a single sentence, set as the section `h2`.
- A **live demo** if the concept is spatial or stateful — slider or drag that makes the system rearrange itself. Figure in a hairline card, controls in a `controlbar`, a `sidecard` beside it holding the comparison numbers.
- A comparison against the naive approach with concrete metrics, not adjectives. "Moves 1/N keys instead of (N−1)/N" is meaningful; "better" isn't.
- "Where you'll meet it" — real systems the reader knows.
- Glossary in the margin, not at the bottom. Bottom glossaries are never read.

**Avoid** a Wikipedia-style overview, burying the punchline, and skipping the demo because "the reader can imagine it." They cannot — that's why they're here.

## Feature explainer (code in a repo)

For "how does our auth flow work."

TL;DR card at the top: what it does, where it lives, key files. Collapsible sections per lifecycle phase — default-open for the overview, closed for the deep stuff. Code with annotations rather than bare blocks. An FAQ at the bottom, which is where the reader's real questions live. "Where to look next" links into the codebase.

## Status report

For "summarize what shipped this week."

Shipped / in flight / blocked as three visually distinct groups, colour-coded. One line per item with a link — a sentence of context only if the item needs it. A `stats` row or sparkline somewhere; recurring reports get skimmed and a number is what the eye lands on. **Asks** separated visually — they get lost when intermixed with status.

**Load-bearing:** brevity per item. A status report is read in 90 seconds or not at all.

## Incident report

For "write up yesterday's outage."

**Layout**
- Header: severity, duration, customer impact in one line. Leadership reads only this and the action items.
- A **stat row** — severity, duration, blast radius, time to detect — over a **timeline table** with a proportional rail per row, so the pace is visible: long flat stretches, then clusters.
- A dashed error-coloured band for any silent stretch. A twelve-minute gap should be a shape, not a sentence.
- Root cause as its own section, written for someone who skipped the timeline.
- "What worked" alongside "what didn't." Without the former, post-mortems become punitive.
- Action items with owners and dates. Commitments, not a wishlist.

## Sketch

```html
<main class="container">
  <header class="masthead">
    <div class="mesh" aria-hidden="true"></div>
    <p class="eyebrow">Explainer</p>
    <h1>Consistent hashing, in one ring</h1>
    <p class="dek">N caches, K keys. Add or remove a node and only ~K/N keys move —
       instead of ~all of them with hash mod N. Here's why.</p>
  </header>

  <section>
    <p class="eyebrow">01 — The trick <span class="src">reports-and-research.md</span></p>
    <h2>Hash onto a circle, not a line</h2>

    <div class="figrow">
      <figure class="card inset" style="margin:0">
        <svg id="ring" class="flow" viewBox="0 0 400 400"><!-- rendered live --></svg>
        <div class="controlbar">
          <label>Nodes <input type="range" id="nodes" min="2" max="12" value="4"></label>
          <span class="readout" id="readout"></span>
        </div>
        <figcaption>Add or remove a node and watch how little moves.</figcaption>
      </figure>

      <aside class="card inset sidecard">
        <p class="eyebrow">Versus hash mod N</p>
        <dl>
          <dt>Keys moved</dt><dd>~K/N</dd>
          <dt>Naive</dt><dd class="alert">~all</dd>
          <dt>In the wild</dt><dd>Cassandra</dd>
        </dl>
      </aside>
    </div>
  </section>
</main>
```

For an incident, the spine is a stat row over a timeline — same tokens:

```html
<div class="stats">
  <div><span class="v alert">Sev-2</span><span class="k">Severity</span></div>
  <div><span class="v">41 min</span><span class="k">Duration</span></div>
  <div><span class="v alert">17 min</span><span class="k">To detect</span></div>
</div>

<table class="tl">
  <tr><td class="t">14:02</td>
      <td class="track"><span class="rail"><i style="width:2px"></i></span></td>
      <td class="ms">Deploy of <b>chat@1.9.0</b> reaches 25% of the fleet.</td>
      <td class="d">T+0</td></tr>
  <!-- a dashed error-coloured band makes a silent stretch visible -->
  <tr class="gaprow"><td></td>
      <td class="track"><span class="bar" style="margin-left:12%;width:29%"></span>
          <span class="txt">Twelve silent minutes</span></td>
      <td colspan="2"></td></tr>
  <tr class="key"><td class="t">14:19</td>
      <td class="track"><span class="rail"><i style="width:41.5%"></i></span></td>
      <td class="ms">First customer report. Support pages on-call.</td>
      <td class="d">T+17</td></tr>
</table>
```
