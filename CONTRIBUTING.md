# Contributing

PRs welcome. The bar: every claim about Sonar data or tools must be true against the live API (https://trysonar.app/openapi.json is ground truth) and the current `@sonarapp/mcp` tool surface.

Coding agents (Claude Code, Codex, Cursor) should read [AGENTS.md](AGENTS.md) first — it holds the repo structure, SKILL.md format, writing style, and credential rules.

## What we merge

- Corrections — a tool name, parameter, cost, or limit that drifted from the API
- Sharper workflows — better phase ordering, better output templates, tighter heuristics
- New skills — a focused workflow with a clear trigger boundary that existing skills don't cover

## What we don't

- Invented metrics or endpoints
- Marketing copy inside skill bodies — skills are working instructions
- Skills that overlap an existing skill's description without a clear boundary
- Examples that read credentials from the user's environment

## Process

1. Fork, branch (`feature/skill-name` or `fix/skill-name-desc`)
2. Keep each SKILL.md under 500 lines, frontmatter valid (name matches directory)
3. If you touch tool usage, update tools/REGISTRY.md in the same PR
4. Bump `version` in `.claude-plugin/plugin.json` for any user-visible change
5. Run `claude plugin validate .` — it must pass with zero warnings
6. Conventional commits: `feat(keyword-research): ...`
