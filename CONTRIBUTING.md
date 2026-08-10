# Contributing

PRs welcome. The bar: every claim about Sonar data or tools must be true against the live API (https://trysonar.app/openapi.json is ground truth).

## What we merge

- Corrections — a tool name, parameter, cost, or limit that drifted from the API
- Sharper workflows — better phase ordering, better output templates, tighter heuristics
- New skills — a focused workflow with a clear trigger boundary that existing skills don't cover

## What we don't

- Invented metrics or endpoints
- Marketing copy inside skill bodies — skills are working instructions
- Skills that overlap an existing skill's description without a clear boundary

## Process

1. Fork, branch (`feature/skill-name` or `fix/skill-name-desc`)
2. Keep each SKILL.md under 500 lines, frontmatter valid (name matches directory)
3. If you touch tool usage, update tools/REGISTRY.md in the same PR
4. Conventional commits: `feat(keyword-research): ...`
