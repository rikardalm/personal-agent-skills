# Matching the User's Style

Bad-looking HTML is worse than good markdown. The harm this skill can do is generic output: gradient cards, emoji headers, four shades of indigo. Read this before drafting anything that will be shared or kept.

Precedence, always:

1. The user's own design system, if the codebase has one.
2. A `frontend-design` plugin or skill, if installed.
3. **The house style — `../design.md`, rendered in `../design-reference.html`.**

Never invent a fourth thing.

## Three rules

1. **Restraint over decoration.** One near-black ink on a near-white canvas, hairline borders, generous spacing. Colour appears in exactly one place per page.
2. **Let size do the ranking.** Tight negative tracking on display headings, neutral on body. Weight is near-binary: 600 headings, 500 buttons/labels, 400 the rest. No light weights, no italics for emphasis.
3. **Colour carries meaning.** Severity, status, category, axis, focus — or remove it.

## The token block

Paste this whole thing — it's `design-reference.html`'s block, trimmed to what the patterns below need. The specimen also carries `--pink`, the rest of the gradient trio, `--error-deep`, `--r-pill-category` and its own `--rail`/`--container` sizes; copy those from it if you need them.

```css
:root{
  /* brand & accent */
  --ink:#171717; --primary:#171717; --on-primary:#ffffff;
  --link:#0070f3; --link-deep:#0761d1; --link-soft:#d3e5ff;
  --violet:#7928ca; --cyan:#50e3c2; --magenta:#eb367f;
  /* surface */
  --canvas:#fafafa; --canvas-elevated:#ffffff; --hairline-soft:#f2f2f2;
  /* text ladder — step it deliberately */
  --body:#4d4d4d; --mute:#8f8f8f; --faint:#a1a1a1;
  /* border — the structural workhorse */
  --hairline:#ebebeb;
  /* semantic */
  --error:#ee0000; --warning:#f5a623; --warning-soft:#fdf3e5; --warning-deep:#b06c08;
  /* gradient stops, hero mesh only */
  --gradient-develop-start:#007cf0; --gradient-ship-end:#f9cb28;
  /* type — Geist first, documented fallbacks after; no webfont fetch */
  --sans:Geist,"Geist Sans",Inter,ui-sans-serif,system-ui,-apple-system,Arial,sans-serif;
  --mono:"Geist Mono","JetBrains Mono","IBM Plex Mono",ui-monospace,"SF Mono",Menlo,monospace;
  /* spacing — 4px base */
  --xxs:4px; --xs:8px; --sm:12px; --md:16px; --lg:24px; --xl:32px;
  --2xl:40px; --3xl:64px; --4xl:96px;
  /* radius — bimodal: 6px chrome, 100px pill, 12–16px cards */
  --r-sm:6px; --r-md:12px; --r-lg:16px; --r-pill:100px; --r-full:9999px;
  /* elevation — hairline first, shadow only if it floats */
  --whisper:0px 1px 1px rgba(0,0,0,.04);
  --floating:0px 2px 2px rgba(0,0,0,.04),0px 8px 16px -4px rgba(0,0,0,.06);
}
/* Dark is NOT in design.md — this derivation keeps the ladder and the accents. */
@media (prefers-color-scheme:dark){:root{
  --ink:#ededed; --primary:#ededed; --on-primary:#0a0a0a;
  --canvas:#0a0a0a; --canvas-elevated:#111111; --hairline-soft:#161616;
  --body:#a1a1a1; --mute:#8f8f8f; --faint:#6f6f6f;
  --hairline:#262626; --link:#3291ff; --link-soft:#10233f; --error:#f33;
}}

html{background:var(--canvas);color:var(--ink)}
body{margin:0;font:400 14px/20px var(--sans);font-variant-numeric:tabular-nums;
     -webkit-font-smoothing:antialiased}
h1{font:600 48px/48px var(--sans);letter-spacing:-2.4px;margin:0}
h2{font:600 32px/40px var(--sans);letter-spacing:-1.28px;margin:0}
h3{font:600 20px/28px var(--sans);letter-spacing:-.4px;margin:0}
.eyebrow{font:500 12px/16px var(--mono);text-transform:uppercase;color:var(--mute);margin:0}
.intro,.dek{font:400 16px/24px var(--sans);color:var(--body)}
code{font:400 14px/20px var(--mono)}
.card{background:var(--canvas-elevated);border:1px solid var(--hairline);border-radius:var(--r-md)}
.card.inset{padding:var(--lg)}
/* page wrapper: .container for a plain document, .shell for the contents-rail
   layout (see design-reference.html). .measure caps prose at a readable line. */
.container{max-width:1040px;margin:0 auto;padding:0 var(--lg)}
.measure{max-width:58rem}
```

## The patterns

Reach for these before inventing. All are rendered in `../design-reference.html`.

| Pattern | What it is |
|---|---|
| **Mono eyebrow** | `<p class="eyebrow">01 — Options <span class="src">source.md</span></p>` above every `h2`. Labels the section like a spec sheet. |
| **Hairline card** | White on the canvas, 1px `--hairline`, 12px radius. Default container for a table, figure, or diff. Flat unless it genuinely floats. |
| **Contents rail** | Sticky left column, mono numerals, active item on `--hairline-soft`. For anything with four-plus sections. |
| **Criteria matrix** | Criteria down the side, options across the top. The recommendation takes `--hairline-soft` plus an accent-blue eyebrow — never an inverted black column. |
| **Rail under a number** | 3px `--hairline` track with an `--ink` (or `--mute`) fill, so the size of a gap is legible before the digits are. |
| **Two-pane diff** | Diff left, notes right, cross-linked. Notes pane takes `--hairline-soft` so the shorter column reads as a panel, not a void. |
| **Stat row** | `h2`-scale numerals with mono uppercase labels, between two hairlines. |
| **Semantic tag** | 6px mono chip: `Blocking` on error, `Nit` on warning, `Question` on link. Never an emoji. |
| **CTA band** | End of document. Display heading, one sentence, then the export pills. |

## Two button shapes, by surface

The system's clearest signal — honour it.

- **Marketing surface** (hero, CTA band, the terminal export): black pill, `--r-pill`, 16px/500, 44px high.
- **App surface** (nav, contents rail, editor toolbars): 6px square, `--r-sm`, 14px/500, 32px high.

Don't mix them inside one context. An editor's toolbar is squares; its single final "copy out" can be a pill.

## No borrowed marks

Set the wordmark as plain text — the project's own name at 14px/500, `-.28px` tracking. Don't draw Vercel's triangle or any other company's logo, and don't leave a logo placeholder either.

## The one flourish

A soft multi-stop mesh gradient behind or beside the headline. It is the **entire** decorative system — nowhere else gets a gradient, glow, or saturated fill.

```css
.mesh{position:absolute;top:-60%;right:-8%;width:60%;height:220%;pointer-events:none;
  filter:blur(68px);opacity:.34;   /* .5 on a hero-led page, .34 on a dense one */
  background:
    radial-gradient(38% 46% at 16% 26%, var(--cyan) 0%, transparent 70%),
    radial-gradient(34% 42% at 36% 8%,  var(--gradient-develop-start) 0%, transparent 72%),
    radial-gradient(36% 48% at 58% 30%, var(--violet) 0%, transparent 70%),
    radial-gradient(30% 40% at 78% 12%, var(--magenta) 0%, transparent 72%),
    radial-gradient(34% 42% at 96% 36%, var(--gradient-ship-end) 0%, transparent 74%);
}
```

## Tools and editors: same tokens, tighter

An editor is an app surface. Keep the 14px body and hairline cards, drop the display scale, use 6px squares throughout, give the work area the width. Density comes from removing padding, never from type below 14px.

## When the user has their own system

Don't invent one and don't reach for Geist. Read their Tailwind config / theme file / CSS variables, generate a one-time `design-system.html` (see `design-and-prototypes.md` for the layout; `../design.md` is a worked example of the written form), save it where it can be reused, and read it first on every subsequent artifact. Suggest this the first time the user asks for an artifact in a project with a real design system.

If a `frontend-design` plugin or skill is installed, defer to it (likely `/mnt/skills/public/frontend-design/SKILL.md`).

## Avoid

- Cards everywhere with shadows on a grey background.
- A full-bleed gradient hero. One soft mesh behind the headline is the system; a saturated banner is not.
- Emoji as section headers or severity markers.
- Four shades of indigo doing nothing.
- Shadcn-shaped components with no shadcn.
- Glass morphism, frosted blur, animated backgrounds.
- Centered everything.
- A logo placeholder — or worse, a borrowed logo.

Any three of those: restart.

## What good looks like

Calm typography, restrained ink, real diagrams instead of icon decoration. Vercel's own documentation. Stripe Press. Bartosz Ciechanowski's explainers. The New York Times graphics desk. The OEIS. Things that look like *someone read them*.
