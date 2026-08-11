---
name: release-readiness
description: Run when the user asks whether current changes are ready to merge to main, e.g. "do you judge that we're ready to merge?"
---

## Steps
1. Review the diff against main: scope, completeness, and whether it matches the stated intent of the change.
2. Check correctness signals: tests exist and pass, edge cases covered, no obvious regressions or broken contracts.
3. Check hygiene: leftover debug code, TODOs, dead code, secrets, migration/config completeness.
4. Assess risk: blast radius, rollback difficulty, and anything that should be split out or feature-flagged.
5. Deliver a clear verdict: **Ready / Ready with nits / Not ready**, with blocking issues listed first, then non-blocking suggestions.

## Execution notes
- Give a direct judgment — do not hedge into "it depends" without committing to a recommendation.
- Blockers must be concrete and actionable (file, line, what to fix), not vague concerns.