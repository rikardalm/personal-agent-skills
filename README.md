# Personal Agent Skills

Private, canonical collection of reusable skills for Codex, Claude Code, Cursor, and other Agent Skills-compatible tools.

## Install

Install every skill globally for Codex, Claude Code, and Cursor:

```bash
npx skills@latest add rikardalm/personal-agent-skills \
  --global \
  --skill '*' \
  --agent codex \
  --agent claude-code \
  --agent cursor \
  --yes
```

List the available skills without installing:

```bash
npx skills@latest add rikardalm/personal-agent-skills --list
```

The CLI uses existing Git, GitHub CLI, or SSH authentication to access this private repository.

## Skills

- `agent-browser`
- `claude-cli`
- `executive-summary`
- `explain-like-junior`
- `find-skills`
- `html-artifacts`
- `release-readiness`
- `review-merit`

## Upstream sources

Three skills are vendored from public upstream projects:

- `agent-browser`: <https://github.com/vercel-labs/agent-browser>
- `find-skills`: <https://github.com/vercel-labs/skills>
- `html-artifacts`: <https://github.com/dogum/html-artifacts>

Review upstream changes before replacing the copies stored here. The other skills were imported from the existing local Codex installation.

## Repository rules

- Keep one folder per skill under `skills/`.
- Keep credentials and machine-specific configuration out of the repository.
- Review skill changes before installing updates across agents.
