# Agent guidelines

This repo contains App Store Optimization skills following the [Agent Skills](https://agentskills.io) open standard, backed by the Sonar API and MCP server for live data.

## Structure

```
SKILL.md                   — root router (for agents importing the repo root)
skills/<name>/SKILL.md     — skill instructions (required, <500 lines)
tools/REGISTRY.md          — capability matrix: which skill uses which tool
.claude-plugin/            — Claude Code plugin manifest
```

Compatible directories: `.claude/skills/`, `.cursor/skills/`, `.agents/skills/`, `.codex/skills/`

## SKILL.md format

```yaml
---
name: skill-name          # Must match directory, lowercase+hyphens, 1-64 chars
description: ...          # Trigger phrases + scope boundaries, 1-1024 chars
metadata:
  version: 1.0.0
---
```

The description drives discovery — an agent reads all descriptions to decide which skill to load. Include "When the user wants to...", trigger keywords, and "For X, see Y" boundaries.

## Writing style

- Direct, actionable, second person
- Sentence case headings, no emoji
- Tables for comparisons, numbered lists for steps
- Real tool names and real parameters only — never invent Sonar fields or endpoints

## Ground truth

The Sonar API is the source of truth. Verify tool behavior against:

- https://trysonar.app/openapi.json (machine-readable)
- https://trysonar.app/docs/api and /docs/mcp (human docs)
- https://trysonar.app/agent-skill.md (agent-facing API summary)

## App store reference

**iOS limits:** title 30, subtitle 30, keyword field 100 (comma-separated, no spaces), description 4000 (not indexed), promotional text 170.

**Android limits:** title 30, short description 80, description 4000 (all indexed).

**Key rules:** title carries the highest keyword weight on both stores. On iOS never repeat a keyword across indexed fields and use singular forms. On Android repeat core terms 2–3x naturally in the description.

## Commits

`feat(skill-name): ...` / `fix(skill-name): ...` / `docs: ...`
