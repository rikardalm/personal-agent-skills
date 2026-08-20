# Decks

A handful of `<section>` tags and twenty lines of JavaScript is a slide deck. Use this for short presentations the user arrow-keys through in a meeting — no Keynote, no export step.

A deck is **the one artifact that inverts the house palette** (`../design.md`): rooms project dark, so canvas and ink swap. Everything else holds — the type scale, the tight display tracking, mono eyebrows, and the single mesh gradient on the title slide.

## When to make one

The user says "deck," "slides," or "presentation"; the content is presented *to others* with someone narrating; ~5–20 slides with minimal per-slide text and natural beats.

If it's dense reference material the reader studies alone, a deck is wrong — use the report pattern.

## Per-slide

- **One idea per slide.** Two ideas means two slides; forced focus is the whole point.
- **Big type.** Readable from the back of a room — the display scale, sized to the viewport with `clamp()`.
- **Minimal words.** A slide is a visual aid, not a document. A paragraph on a slide means the speaker is competing with their own slide.
- Title, the one main thing (chart, quote, code), maybe a small footnote. That's it.

## Load-bearing

- Arrow-key navigation. A deck without it is a webpage that has slides on it.
- A slide counter. Presenter and audience both want to know where they are.
- Real fullscreen — a "press F" hint or a button. Browser chrome is distracting.
- Fixed aspect handling; don't let layout shift between slides.

## Avoid

- Making every slide a markdown-rendered card. One slide is a chart, the next a quote — don't homogenise them.
- Cramming. Five bullets is two slides, or a table.
- Transition animations. They distract and break when the user clicks fast.

## Skeleton

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8"/>
  <meta name="viewport" content="width=device-width,initial-scale=1"/>
  <title>Deck title</title>
  <style>
    /* house tokens, inverted for projection */
    :root{
      --ink:#171717; --canvas:#fafafa; --mute:#8f8f8f;
      --cyan:#50e3c2; --violet:#7928ca; --magenta:#eb367f;
      --sans:Geist,"Geist Sans",Inter,ui-sans-serif,system-ui,sans-serif;
      --mono:"Geist Mono","JetBrains Mono",ui-monospace,Menlo,monospace;
      --md:16px; --lg:24px;
    }
    html,body{margin:0;height:100%;background:var(--ink);color:var(--canvas);
              font:400 14px/20px var(--sans);-webkit-font-smoothing:antialiased}
    .slide{display:none;height:100vh;padding:6vh 8vw;box-sizing:border-box;
           flex-direction:column;justify-content:center;position:relative}
    .slide.active{display:flex}
    .slide .eyebrow{font:500 12px/16px var(--mono);text-transform:uppercase;
                    color:var(--mute);margin:0 0 var(--lg)}
    /* display scale sized to the room, tracking kept tight */
    .slide h1{font:600 clamp(40px,7vmin,88px)/1.02 var(--sans);
              letter-spacing:-.04em;margin:0 0 var(--md);max-width:22ch}
    .slide h2{font:400 clamp(20px,3vmin,32px)/1.3 var(--sans);
              color:var(--mute);margin:0;max-width:40ch}
    .counter{position:fixed;bottom:var(--lg);right:var(--lg);
             font:500 12px/16px var(--mono);color:var(--mute)}
    /* the single flourish, title slide only */
    .mesh{position:absolute;inset:auto -10% -30% 20%;height:90%;filter:blur(80px);
      opacity:.3;pointer-events:none;
      background:
        radial-gradient(38% 46% at 20% 40%, var(--cyan) 0%, transparent 70%),
        radial-gradient(36% 48% at 56% 30%, var(--violet) 0%, transparent 70%),
        radial-gradient(32% 42% at 88% 44%, var(--magenta) 0%, transparent 72%);
    }
  </style>
</head>
<body>
  <section class="slide active">
    <div class="mesh" aria-hidden="true"></div>
    <p class="eyebrow">Platform · Aug 2026</p>
    <h1>Why HTML beats markdown</h1>
    <h2>For the things agents now make</h2>
  </section>

  <section class="slide">
    <p class="eyebrow">01 — Density</p>
    <h1>Tables, SVG, colour, interactivity</h1>
    <h2>Markdown can't.</h2>
  </section>

  <div class="counter"><span id="i">1</span> / <span id="n"></span></div>

  <script>
    const slides = document.querySelectorAll('.slide');
    let i = 0;
    document.getElementById('n').textContent = slides.length;
    function go(n) {
      i = Math.max(0, Math.min(slides.length - 1, n));
      slides.forEach((s, idx) => s.classList.toggle('active', idx === i));
      document.getElementById('i').textContent = i + 1;
    }
    document.addEventListener('keydown', e => {
      if (e.key === 'ArrowRight' || e.key === ' ') go(i + 1);
      else if (e.key === 'ArrowLeft') go(i - 1);
      else if (e.key === 'f') document.documentElement.requestFullscreen();
    });
  </script>
</body>
</html>
```

Twenty lines of JS, no build step, opens directly in a browser.
