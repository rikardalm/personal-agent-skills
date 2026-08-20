# Custom Editors

The most distinctive use of the format. The user has a one-off task — triage 30 tickets, tune a regex, curate a dataset — and a chat box is the wrong shape for it. Build a throwaway editor: one HTML file, purpose-built, that always ends with an export. House style: `../design.md`; token block in `matching-your-style.md`.

## The non-negotiable rule

**Every editor ends with an export.** Copy as markdown, copy as JSON, copy as prompt, download CSV — whatever turns UI state into something pasteable into a commit, a ticket, or the next prompt. Without it the editor is a toy; with it the loop closes and the user stays in control.

If you're building one without an export path, stop and add the export first.

## When to build one

The signal: the user is trying to express something that's hard to type. Reordering or bucketing many items. Structured config with constraints. Tuning a prompt with live preview. Curating a dataset — approve/reject/tag. Annotating a transcript or diff. Picking values that are painful in text: colours, easing curves, crop regions, cron schedules, regex with live test strings.

## Layout

An editor is an **app surface**, so the chrome is 6px squares throughout and the pill is reserved for the single terminal export.

- The work area dominates. A contents rail or toolbar holds counts and controls around it.
- One sentence of header saying what this editor is for.
- **Pre-loaded data.** The user already gave you it in the prompt — don't make them paste it twice.
- Interaction primitives matched to the data: drag-and-drop for ordering, toggles for booleans, selects for enums, sliders for ranges.
- A live state indicator — counts per bucket, character count, validation errors — visible immediately.
- Constraints shown at the moment of conflict, as a `tag`, not as a footer disclaimer.
- Keyboard support for anything repetitive. Labelling 100 examples needs `j`/`k` or `1`/`2`/`3`, not clicks.
- Keep the 14px body and hairline cards; drop the 48px display type. Density comes from removing padding, never from type below 14px.
- `localStorage` is fine and worth it for a local `.html` file; in-memory only for Claude.ai artifacts.

## Avoid

- **Making it generic.** This is a throwaway tool for *this* task — the triage board for cycle 14's 24 tickets, not a task management system.
- **Skipping the export.** The most common failure mode, worth repeating.
- **Adding settings.** Settings are for products.
- **Server-style state.** No backend, no auth, everything in one file.
- **Pretty over usable.** Judged by whether the user finishes and leaves.

## Sketches

Triage board — drag across buckets, export one heading per bucket. The only CSS beyond the token block:

```css
.board{display:grid;grid-template-columns:repeat(4,minmax(0,1fr));gap:var(--md)}
.board ul{list-style:none;margin:0;padding:var(--sm)}
@media (max-width:860px){.board{grid-template-columns:1fr}}
```

```html
<div class="shell">
  <nav class="rail">
    <span class="wordmark">cycle 14</span>
    <dl>
      <dt>Now</dt><dd class="ink">6</dd>
      <dt>Next</dt><dd>8</dd>
      <dt>Later</dt><dd>7</dd>
      <dt>Cut</dt><dd class="alert">3</dd>
    </dl>
    <div class="rail-actions">
      <button class="btn btn-primary-sm" id="copy-md">Copy as markdown</button>
      <button class="btn btn-ghost-sm" id="reset">Reset</button>
    </div>
  </nav>

  <main>
    <header class="masthead">
      <p class="eyebrow">Triage</p>
      <h1>Cycle 14</h1>
      <p class="dek">24 tickets, pre-sorted into a best guess. Drag until the cut
         feels right, then copy out as markdown.</p>
    </header>

    <div class="board">
      <section class="card" data-bucket="now">
        <div class="codebar"><span class="file">Now</span><span class="right">6</span></div>
        <ul></ul>
      </section>
      <section class="card" data-bucket="next">…</section>
    </div>
  </main>
</div>

<script>
  const tickets = [/* pre-filled from the prompt */];
  /* render, HTML5 drag-drop, state in a Map */
  /* on copy-md: ## Now\n- TICK-101: short title\n... */
</script>
```

Flag editor — the constraint surfaces at the conflict:

```html
<fieldset class="card inset">
  <legend class="eyebrow">Checkout</legend>
  <label class="row"><input type="checkbox" data-key="checkout.express"> Express checkout</label>
  <label class="row">
    <input type="checkbox" data-key="checkout.applePay" data-requires="checkout.express">
    Apple Pay
    <span class="tag high" hidden>Requires Express checkout</span>
  </label>
</fieldset>

<div class="cta-band">
  <button class="btn btn-primary" id="copy-diff">Copy diff</button>
</div>
```

Prompt tuner — template left, filled samples live on the right:

```html
<div class="figrow">
  <section class="card inset">
    <p class="eyebrow">Prompt template</p>
    <textarea id="tmpl">You are a {{role}}. Given {{input}}, respond with...</textarea>
  </section>

  <aside class="card inset sidecard">
    <p class="eyebrow">Filled samples</p>
    <pre id="out-1"></pre>
    <dl><dt>Chars</dt><dd id="chars">0</dd><dt>Tokens</dt><dd id="tokens">~0</dd></dl>
    <button class="btn btn-primary" id="copy">Copy filled prompt</button>
  </aside>
</div>
```

All three: pre-filled, focused, exportable.
