# Code Review & PR Writeups

Diffs and call graphs are spatial; markdown flattens them. House style: `../design.md`; token block and patterns in `matching-your-style.md`.

## Annotated diff

For "review this PR."

**Layout**
- A hairline card. `codebar` header: filename, `+`/`−` counts, note count.
- **Two panes** — diff left, notes right, cross-linked: clicking a note or its pin highlights the line, and vice versa. That cross-link is the highest-value thing on the page.
- Notes pane takes `--hairline-soft` so the shorter diff column reads as a panel split rather than dead space.
- Severity as `tag` chips in the semantic colours: `Blocking` on error, `Nit` on warning, `Question` on link. Mono uppercase, 6px radius, no emoji.
- Additions read blue and deletions red — the system maps success onto the accent blue.
- A "where to focus" line above the card. Reviewers don't have time for everything.

**Load-bearing**
- Margin notes, not interleaved comments. Interleaving breaks the diff's visual flow; a side pane preserves both readings.
- Severity colour. Reviewers scan for red first.

**Avoid**
- Re-printing the whole diff in `<pre>` with explanations between chunks. That's a markdown article. The diff is the spine; notes attach to it.
- Bullet lists of suggestions. Pin them to lines as numbered pins.

## PR writeup

For "write the description for this PR."

Title (short, imperative), motivation in 2–3 sentences, a real side-by-side before/after if anything visible changed, a file-by-file tour grouped by *theme* rather than alphabetically (plumbing / core logic / tests / docs), "where to focus the review," risks and how it was tested, open questions.

**Load-bearing:** before/after as actual side-by-side, not "before: X, after: Y" prose. And "where to focus" — it saves reviewer time and signals you thought about it.

## Module map / "explain this code"

For "walk me through this package."

One-sentence summary, then a boxes-and-arrows SVG of the modules with the **hot path** in a heavier ink stroke. Entry points called out by use case ("if you're trying to do X, start at Y") — code is rarely read top to bottom. Per-module cards below with what it does, key types, gotchas. Then one realistic input traced through.

**Avoid** drawing every relationship — show structural, not textual, relationships. And this is an explainer, not generated API docs.

## Sketch

```html
<section class="card">
  <div class="codebar">
    <span class="file">src/chat/handler.ts</span>
    <span class="plus">+14</span><span class="minus">−3</span>
    <span class="right">4 notes · 2 blocking</span>
  </div>

  <div class="diffgrid">
    <div class="diff">
      <div class="l"><span class="g">41</span>function handleChat(req) {</div>
      <div class="l del"><span class="g">42</span>-  const msg = await complete(req);</div>
      <div class="l add"><span class="g">42</span>+  const stream = await completeStream(req);</div>
      <div class="l add" data-n="1"><span class="g">43</span>+  return pipeBackpressure(stream, req.signal);<span class="pin b" data-n="1">1</span></div>
      <div class="l"><span class="g">44</span>}</div>
    </div>

    <ol class="notes">
      <li data-n="1"><span class="pin b">1</span>
        <div>
          <div class="hd">
            <span class="tag blocking">Blocking</span>
            <span class="t">Undefined <code>req.signal</code></span>
          </div>
          <p>Undefined on the Node adapter, so aborts never reach the model.
             A closed tab leaves the stream running to completion.</p>
        </div>
      </li>
    </ol>
  </div>
</section>

<script>
  /* the cross-link is the whole point — keep it this small */
  const sel = n => {
    document.querySelectorAll('.diff .l').forEach(l => l.classList.toggle('on', l.dataset.n === n));
    document.querySelectorAll('.notes li').forEach(l => l.classList.toggle('on', l.dataset.n === n));
  };
  document.querySelectorAll('.notes li').forEach(li => li.onclick = () => sel(li.dataset.n));
  document.querySelectorAll('.diff .pin').forEach(p => p.onclick = () => sel(p.dataset.n));
</script>
```
