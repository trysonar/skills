# Sonar — ASO skills for AI agents

Agent skills for **App Store Optimization**, **keyword research**, and **app market intelligence** — for both the **App Store and Google Play**. Built for indie developers, app marketers, and growth teams who want Claude, Claude Code, Cursor, OpenClaw, or any Agent Skills-compatible assistant to do real ASO work: keyword strategy, metadata optimization, competitor analysis, review mining, revenue benchmarking, rank tracking, and screenshot generation.

Powered by live data from [Sonar](https://trysonar.app) — keyword popularity (real Apple Search Popularity on iOS, not a proxy), explainable difficulty scores, daily rank tracking, top charts with movement, reviews, and revenue estimates.

**Try it with no account at all** — several tools work keyless with per-IP daily limits, so your agent can run its first keyword lookups before you ever sign up.

## Install in Claude

This repo is a Claude plugin (`sonar-aso`): the ten workflow skills below plus Sonar's hosted MCP server (`https://trysonar.app/mcp`), the same server as the Sonar ASO connector.

**Claude (claude.ai, desktop, Cowork):** open **Customize > Plugins**, find **Sonar ASO** in the plugin directory, and add it. Then connect the bundled Sonar connector from the plugin's **Connectors** tab.

**Claude Code:**

```
/plugin marketplace add trysonar/skills
/plugin install sonar-aso@trysonar
```

Free-tier tools work right away. Tools that read or change your Sonar workspace ask you to connect your Sonar account once (OAuth sign-in); the plugin never asks for or stores an API key.

## What the plugin sends and where

- **Sonar's MCP server.** When Claude calls a `sonar_` tool, the tool name and its arguments go to `https://trysonar.app/mcp`, operated by Edge Analytics s.r.o. (Sonar). Arguments are what the task needs: store ids, search queries and keywords, country codes, Sonar workspace ids, and for write tools the things you ask Sonar to save (tracked apps, keywords, competitors, keyword notes, alert rules, screenshot layouts, captions, and translations, including any image URLs you supply, which Sonar fetches).
- **Account and limits.** Free-tier calls are anonymous; Sonar uses your IP address to enforce the free tier's daily limits. Signed-in calls carry an OAuth access token from Sonar's sign-in that identifies your Sonar account, and anything you save is stored in that account.
- **Third parties.** To answer, Sonar queries public App Store and Google Play data. The App Store Connect tools (Agency plan) read the connection you set up on trysonar.app.
- **On your machine.** The skills are Markdown instructions. The plugin runs no hooks, scripts, or local servers and installs no packages. The `app-marketing-context` skill may save an `app-marketing-context.md` file in your project when you ask it to.

Privacy policy: [trysonar.app/privacy](https://trysonar.app/privacy).

## Other agents

**Any Agent Skills client** — `npx skills add trysonar/skills`.

**Claude Code without the plugin** — add the hosted MCP server directly:

```bash
claude mcp add --transport http sonar https://trysonar.app/mcp
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

Or invoke a workflow directly: `/keyword-research`, `/metadata-optimization`, `/competitor-analysis`, `/aso-audit`, `/review-analysis`, `/revenue-analysis`, `/market-discovery`, `/rank-tracking`, `/screenshot-studio`, `/app-marketing-context` (in the Claude Code plugin they're namespaced, e.g. `/sonar-aso:keyword-research`).

## Skills

### Research

| Skill | What it does |
|-|-|
| [keyword-research](skills/keyword-research/SKILL.md) | Discover and score keywords by popularity, difficulty, and the `beatable` flag — build a prioritized strategy |
| [competitor-analysis](skills/competitor-analysis/SKILL.md) | Keyword gaps, benchmark matrix, positioning map, and weaknesses mined from competitor reviews |
| [market-discovery](skills/market-discovery/SKILL.md) | Top charts with day-over-day movement — find rising apps and niches across countries |
| [revenue-analysis](skills/revenue-analysis/SKILL.md) | Revenue estimates with confidence grades, niche sizing, monetization patterns, and your own App Store Connect sales |
| [review-analysis](skills/review-analysis/SKILL.md) | Mine reviews for complaints, feature requests, and the fan language worth reusing |

### Execution

| Skill | What it does |
|-|-|
| [metadata-optimization](skills/metadata-optimization/SKILL.md) | Title, subtitle, keyword field, and description — 3 variants with exact character counts |
| [aso-audit](skills/aso-audit/SKILL.md) | 0–100 listing health score with check breakdown and prioritized fixes |
| [rank-tracking](skills/rank-tracking/SKILL.md) | Daily rank tracking, competitor monitoring, alerts, and the dashboard scoreboard; interpret rank history |
| [screenshot-studio](skills/screenshot-studio/SKILL.md) | Build, localize, and export store-ready screenshot sets as code |

### Foundation

| Skill | What it does |
|-|-|
| [app-marketing-context](skills/app-marketing-context/SKILL.md) | A shared context document (app, audience, competitors, goals) that every other skill reads first |
| [sonar-aso](skills/sonar-aso/SKILL.md) | Tool reference — every Sonar MCP tool with required arguments, costs, and access tiers |

Skills reference each other — `competitor-analysis` hands gap keywords to `keyword-research`, which feeds `metadata-optimization`, whose changes are measured by `rank-tracking`. The root [SKILL.md](SKILL.md) routes between them for agents that import the repo root.

## MCP server

Sonar ships an official MCP server two ways:

**Hosted (no install):** streamable HTTP at `https://trysonar.app/mcp`. Free-tier tools work without credentials; everything else uses Sonar's OAuth sign-in.

```json
{
  "mcpServers": {
    "sonar": {
      "type": "http",
      "url": "https://trysonar.app/mcp"
    }
  }
}
```

**Local (npm):** [`@sonarapp/mcp`](https://www.npmjs.com/package/@sonarapp/mcp), configured with an API key from [trysonar.app/settings/developers](https://trysonar.app/settings/developers):

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

**Free tier:** with no account, `sonar_app_search`, `sonar_app_lookup`, `sonar_app_aso_score`, `sonar_app_extract_keywords`, `sonar_keyword_suggestions`, `sonar_top_charts` (shared 30 calls/day per IP), `sonar_keyword_metrics` (5 keywords/day per IP), and `sonar_screenshot_layout_guide` work. Every other tool explains what it needs.

55 tools cover the entire v1 API — stateless research, workspace reads, tracking writes, App Store Connect data, and Screenshot Studio. The hosted server serves 54 of them; `sonar_export_screenshots` writes files locally and is only in the npm package. Full tool reference: [trysonar.app/docs/mcp](https://trysonar.app/docs/mcp).

## API

Prefer raw HTTP? Base URL `https://trysonar.app/api/v1`, Bearer auth:

```bash
curl "https://trysonar.app/api/v1/keywords/metrics?qs=sleep+tracker,meditation&store=ios&country=us" \
  -H "Authorization: Bearer aso_YOUR_KEY"
```

- **Machine-readable docs:** [openapi.json](https://trysonar.app/openapi.json), [llms.txt](https://trysonar.app/llms.txt), [agent-skill.md](https://trysonar.app/agent-skill.md), [/.well-known/agent-skills/index.json](https://trysonar.app/.well-known/agent-skills/index.json)
- **Human docs:** [trysonar.app/docs/api](https://trysonar.app/docs/api)
- **API Credits:** 50 free on signup; packs from $10 (1,000 credits), never expire. Most calls cost 1 credit; `keywords/search` costs 10 (it fans out into ~11 upstream lookups). Bulk endpoints (`keywords/metrics`, `apps/revenue`) take 25 items per call at 1 credit each. Responses carry `X-Credits-Cost` / `X-Credits-Remaining` so agents can budget.
- **Indie and Agency plans:** a daily request allowance instead of credits (1,000/day on Indie, 5,000/day on Agency), plus workspace endpoints (products, tracked keywords, rank history, alerts) and write access.

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
SKILL.md                         — entry point / router (for agents that import the repo root)
skills/<name>/SKILL.md           — focused workflows (Agent Skills standard)
tools/REGISTRY.md                — skill × tool coverage matrix
.mcp.json                        — hosted Sonar MCP server bundled with the Claude plugin
.claude-plugin/plugin.json       — Claude plugin manifest
.claude-plugin/marketplace.json  — marketplace catalog (trysonar)
```

## Contributing

PRs welcome — fix an inaccuracy, sharpen a workflow, or add a skill. See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT — see [LICENSE](LICENSE).
