# Sonar — ASO skills for AI agents

Agent skills for **App Store Optimization**, **keyword research**, and **app market intelligence** — for both the **Apple App Store and Google Play**. Built for indie developers, app marketers, and growth teams who want Claude Code, Cursor, OpenClaw, or any Agent Skills-compatible assistant to do real ASO work: keyword strategy, metadata optimization, competitor analysis, review mining, revenue benchmarking, rank tracking, and screenshot generation.

Powered by live data from the [Sonar API](https://trysonar.app) — keyword popularity (real Apple Search Popularity on iOS, not a proxy), explainable difficulty scores, daily rank tracking, top charts with movement, reviews, and revenue estimates.

**Try it with no account at all** — several endpoints work keyless with per-IP daily limits, so your agent can run its first keyword lookups before you ever sign up.

## Quick start

**Claude Code** — `npx skills add trysonar/skills`, or add the MCP server:

```bash
claude mcp add --transport http sonar https://trysonar.app/mcp --header "Authorization: Bearer aso_YOUR_KEY"
```

**Cursor** — Settings → Rules → Add Rule → Remote Rule (GitHub) → paste this repo's URL.

**OpenClaw** — `openclaw skills install sonar-aso` (published on [ClawHub](https://clawhub.ai/petersutarik/sonar-aso)).

**Manual (any agent)** — `git clone https://github.com/trysonar/skills.git && cp -r skills/skills/* .claude/skills/` (also works with `.cursor/skills/`, `.agents/skills/`, `.codex/skills/`).

Then ask your agent:

```
"Research keywords for a sleep tracking app in the US"
"Which of these keywords can I actually rank for?"
"Audit my app's listing — my iOS app id is 1234567890"
"What do users complain about in [competitor]'s reviews?"
"How much do the top meditation apps make?"
"What's moving on the health & fitness grossing chart this week?"
"Set up daily rank tracking for my app and 3 competitors"
"Write me 3 title/subtitle variants under the character limits"
```

Or invoke a workflow directly: `/keyword-research`, `/metadata-optimization`, `/competitor-analysis`, `/aso-audit`, `/review-analysis`, `/revenue-analysis`, `/market-discovery`, `/rank-tracking`, `/screenshot-studio`, `/app-marketing-context`.

## Skills

### Research

| Skill | What it does |
|-|-|
| [keyword-research](skills/keyword-research/SKILL.md) | Discover and score keywords by popularity, difficulty, and the `beatable` flag — build a prioritized strategy |
| [competitor-analysis](skills/competitor-analysis/SKILL.md) | Keyword gaps, benchmark matrix, positioning map, and weaknesses mined from competitor reviews |
| [market-discovery](skills/market-discovery/SKILL.md) | Top charts with day-over-day movement — find rising apps and niches across countries |
| [revenue-analysis](skills/revenue-analysis/SKILL.md) | Download and revenue estimates (bulk), niche sizing, monetization pattern analysis |
| [review-analysis](skills/review-analysis/SKILL.md) | Mine reviews for complaints, feature requests, and the fan language worth reusing |

### Execution

| Skill | What it does |
|-|-|
| [metadata-optimization](skills/metadata-optimization/SKILL.md) | Title, subtitle, keyword field, and description — 3 variants with exact character counts |
| [aso-audit](skills/aso-audit/SKILL.md) | 0–100 listing health score with factor breakdown and prioritized fixes |
| [rank-tracking](skills/rank-tracking/SKILL.md) | Set up daily rank tracking, competitor monitoring, and email alerts; interpret rank history |
| [screenshot-studio](skills/screenshot-studio/SKILL.md) | Build, localize, and export store-ready screenshot sets as code |

### Foundation

| Skill | What it does |
|-|-|
| [app-marketing-context](skills/app-marketing-context/SKILL.md) | A shared context document (app, audience, competitors, goals) that every other skill reads first |
| [sonar-aso](skills/sonar-aso/SKILL.md) | Raw REST API reference — curl-based access for agents without an MCP client |

Skills reference each other — `competitor-analysis` hands gap keywords to `keyword-research`, which feeds `metadata-optimization`, whose changes are measured by `rank-tracking`. The root [SKILL.md](SKILL.md) routes between them for agents that import the repo root.

## MCP server

Sonar ships an official MCP server two ways:

**Hosted (no install):** streamable HTTP at `https://trysonar.app/mcp`

```json
{
  "mcpServers": {
    "sonar": {
      "url": "https://trysonar.app/mcp",
      "headers": { "Authorization": "Bearer aso_YOUR_KEY" }
    }
  }
}
```

**Local (npm):** [`@sonarapp/mcp`](https://www.npmjs.com/package/@sonarapp/mcp)

```json
{
  "mcpServers": {
    "sonar": {
      "command": "npx",
      "args": ["-y", "@sonarapp/mcp"],
      "env": { "SONAR_API_KEY": "aso_YOUR_KEY" }
    }
  }
}
```

**Free mode:** leave `SONAR_API_KEY` unset and the server still works — `sonar_app_search`, `sonar_app_lookup`, `sonar_app_aso_score`, `sonar_app_extract_keywords`, `sonar_keyword_suggestions` (shared 30 requests/day per IP) and `sonar_keyword_metrics` (5 keywords/day per IP) run on the anonymous free tier. Every other tool returns an actionable message explaining what to connect.

Get an API key at [trysonar.app/settings/developers](https://trysonar.app/settings/developers). 47 tools cover the entire v1 API — stateless research, workspace reads, tracking writes, and Screenshot Studio. Full tool reference: [trysonar.app/docs/mcp](https://trysonar.app/docs/mcp).

## API

Prefer raw HTTP? Base URL `https://trysonar.app/api/v1`, Bearer auth:

```bash
curl "https://trysonar.app/api/v1/keywords/metrics?qs=sleep+tracker,meditation&store=ios&country=us" \
  -H "Authorization: Bearer aso_YOUR_KEY"
```

- **Machine-readable docs:** [openapi.json](https://trysonar.app/openapi.json), [llms.txt](https://trysonar.app/llms.txt), [agent-skill.md](https://trysonar.app/agent-skill.md), [/.well-known/agent-skills/index.json](https://trysonar.app/.well-known/agent-skills/index.json)
- **Human docs:** [trysonar.app/docs/api](https://trysonar.app/docs/api)
- **Credits:** 50 free on signup; packs from $10 (1,000 credits), never expire. Most calls cost 1 credit; `keywords/search` costs 10 (it fans out into ~11 upstream lookups). Bulk endpoints (`keywords/metrics`, `apps/revenue`) take 25 items per call at 1 credit each. Responses carry `X-Credits-Cost` / `X-Credits-Remaining` so agents can budget.
- **Subscribers:** flat 1,000 requests/day instead of credits, plus workspace endpoints (products, tracked keywords, rank history, alerts) and write access.

## Why Sonar data

- **Real Apple Search Popularity** on iOS where available (`popularity_source: "apple"`) — not a review-count proxy
- **Explainable difficulty** — `difficulty_breakdown` shows title matches, top-3 strength, and the weak spot; `beatable: true` means a top-3 slot looks winnable and the fields tell you why
- **Both stores, 50+ countries** — popularity, difficulty, ranks, reviews, and charts per store per country
- **Daily rank tracking with alerts** — research becomes a measurable feedback loop, not a one-off report
- **Keyless trial tier** — agents can evaluate the data quality before anyone creates an account

## Related

- [MCP server](https://github.com/trysonar/mcp) — the same capabilities as native MCP tools (`npx @sonarapp/mcp` or hosted at `https://trysonar.app/mcp`)
- [CLI](https://github.com/trysonar/cli) — ASO from the terminal ([`@sonarapp/cli`](https://www.npmjs.com/package/@sonarapp/cli))
- [API docs](https://trysonar.app/docs/api)

## Repository structure

```
SKILL.md                   — entry point / router (for agents that import the repo root)
skills/<name>/SKILL.md     — focused workflows (Agent Skills standard)
tools/REGISTRY.md          — skill × tool coverage matrix
.claude-plugin/            — Claude Code plugin manifest
```

## Contributing

PRs welcome — fix an inaccuracy, sharpen a workflow, or add a skill. See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT
