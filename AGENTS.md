# Agent guidelines

This repo contains App Store Optimization skills following the [Agent Skills](https://agentskills.io) open standard, backed by the Sonar API and MCP server for live data. It is also a Claude plugin (`sonar-aso`) and a one-plugin marketplace (`trysonar`).

These are the contributor instructions for any coding agent working in this repo, Claude Code included. A root `CLAUDE.md` is deliberately absent: Claude plugins don't load it, and `claude plugin validate` warns about it.

## Structure

```
SKILL.md                         — root router (for agents importing the repo root)
skills/<name>/SKILL.md           — skill instructions (required, <500 lines)
tools/REGISTRY.md                — capability matrix: which skill uses which tool
.mcp.json                        — hosted Sonar MCP server bundled with the plugin
.claude-plugin/plugin.json       — Claude plugin manifest (name, version, metadata)
.claude-plugin/marketplace.json  — marketplace catalog, so `/plugin marketplace add trysonar/skills` works
.claude-plugin/icon.png          — plugin icon (square PNG, 1024px)
```

Compatible directories: `.claude/skills/`, `.cursor/skills/`, `.agents/skills/`, `.codex/skills/`

## SKILL.md format

```yaml
---
name: skill-name          # Must match directory, lowercase+hyphens, 1-64 chars
description: ...          # Trigger phrases + scope boundaries, 1-1024 chars
metadata:
  version: 1.1.0
---
```

The description drives discovery — an agent reads all descriptions to decide which skill to load. Include "When the user wants to...", trigger keywords, and "For X, see Y" boundaries.

## Writing style

- Direct, actionable, second person
- Sentence case headings, no emoji
- Tables for comparisons, numbered lists for steps
- Real tool names and real parameters only — never invent Sonar fields or endpoints
- Plans are "Indie", "Agency", and "API Credits" — never "Full plan" or "Agent plan"
- State what a tool requires (plan, sign-in, credits) plainly; don't pitch upgrades or purchases inside skills

## Credentials

Nothing in this repo needs, reads, or stores an API key. Skills never read keys from the user's environment (no `$SONAR_API_KEY` expansion in examples or commands). In Claude, the bundled MCP server (`.mcp.json`, no headers) serves the keyless free tier and authenticates everything else through Sonar's OAuth sign-in. Keep it that way: an empty or invalid key header would turn the free tier into a 401.

## Ground truth

The Sonar API and MCP server are the source of truth. Verify tool behavior against:

- https://trysonar.app/openapi.json (machine-readable)
- https://trysonar.app/docs/api and /docs/mcp (human docs)
- https://trysonar.app/agent-skill.md (agent-facing API summary)
- The `@sonarapp/mcp` package source (tool names, input schemas, which tools are keyless, which are local-only)

The hosted server at `https://trysonar.app/mcp` exposes every tool except `sonar_export_screenshots`, which writes to the local filesystem and only exists in the stdio package.

## Maintenance rule

The skills describe the live API. When a v1 endpoint or MCP tool changes, this repo is part of the update checklist: update the affected skills and tools/REGISTRY.md, bump `version` in `.claude-plugin/plugin.json` (and the touched skills' `metadata.version`), and run `claude plugin validate .` — it must pass with zero warnings.

## App store reference

**iOS limits:** title 30, subtitle 30, keyword field 100 (comma-separated, no spaces), description 4000 (not indexed), promotional text 170.

**Android limits:** title 30, short description 80, description 4000 (all indexed).

**Key rules:** title carries the highest keyword weight on both stores. On iOS never repeat a keyword across indexed fields and use singular forms. On Android repeat core terms 2–3x naturally in the description.

## Commits

`feat(skill-name): ...` / `fix(skill-name): ...` / `docs: ...`
