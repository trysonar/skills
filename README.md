# Sonar Agent Skills

Official agent skills for [Sonar](https://trysonar.app) — App Store Optimization for AI agents.

## Skills

### [sonar-aso](skills/sonar-aso/SKILL.md)

App Store Optimization via the Sonar API: keyword research with difficulty and popularity scores, app lookup and search, ASO audits, review mining, and revenue estimates for iOS and Google Play.

**Install (OpenClaw):**

```bash
openclaw skills install sonar-aso
```

Published on [ClawHub](https://clawhub.ai/petersutarik/sonar-aso). Works in any Agent-Skills-compatible client (Claude Code, Cursor, OpenClaw, ...) — just drop the `skills/sonar-aso/` folder into your skills directory.

**Requires:** a Sonar API key (`SONAR_API_KEY`, format `aso_...`) — create one at [trysonar.app/developers](https://trysonar.app/developers). New accounts include free credits.

## Related

- [MCP server](https://github.com/trysonar/mcp) — the same tools as native MCP tools (`npx @sonarapp/mcp` or hosted at `https://trysonar.app/mcp`)
- [CLI](https://github.com/trysonar/cli) — ASO from the terminal
- [API docs](https://trysonar.app/docs/api)
