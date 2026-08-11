---
name: explain-like-junior
description: Run when a plan, change, or situation is complex enough that plain text/diffs hide crucial details and the user needs it explained simply.
---

## Steps
1. Identify what's actually hard here: the moving parts, their relationships, and where the non-obvious risk or behavior lives.
2. Explain it as you would to a junior dev: plain language, no assumed context, concrete examples over abstractions, one concept at a time.
3. Pick the right visual for the situation:
   - **ASCII diagram** for flows, sequences, architecture, or before/after structure — inline in the response.
   - **Simple HTML file** for anything interactive or too dense for ASCII (timelines, state tables, dependency graphs).
   - **No visual** if prose alone is genuinely clearer.
4. End with "what could surprise you": the 2-3 details most likely to be missed when skimming the original text or diff.

## Execution notes
- Simplify the explanation, never the truth — don't omit caveats that matter, just state them plainly.
- One good diagram beats three mediocre ones. Don't decorate; illustrate.
- If explaining a diff: show what the system did before vs. after, not a line-by-line walkthrough.