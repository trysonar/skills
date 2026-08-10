# Distribution checklist (internal — delete before or after launch, or keep, it's harmless)

Goal: replicate AppKittie's agent-native distribution — free backlinks, agent-directory presence, and "connected Claude to Sonar" social proof. Their repo (5 GitHub stars) earned listings on LobeHub, PulseMCP, and MCP Market that now rank for "app store intelligence MCP".

## 1. Repo hygiene

- [x] Home: `trysonar/skills` (existing org, alongside trysonar/mcp and trysonar/cli)
- [ ] Add topics: `aso`, `app-store-optimization`, `mcp`, `mcp-server`, `agent-skills`, `keyword-research`, `app-store`, `google-play`, `claude`, `ai-agents`
- [ ] Repo description: "AI agent skills for App Store Optimization — keyword research, competitor analysis, rank tracking. Powered by the Sonar API (free tier, no key needed)."
- [ ] Social preview image (reuse a Sonar OG asset)

## 2. MCP directories (each = a DF backlink + agent-channel discovery)

| Directory | How to submit |
|-|-|
| PulseMCP | Submit form at pulsemcp.com (they list "AppKittie App Store Intelligence" — same category) |
| LobeHub | lobehub.com/mcp → Publish flow (agent-prompt or PR-based) |
| MCP Market | mcpmarket.com → Submit |
| Glama | glama.ai/mcp/servers → Add Server |
| mcp.so | GitHub issue on their repo (Submit button in nav) |
| Smithery | smithery.ai — register the hosted server; NB Sonar's /mcp already accepts `X-API-Key` for gateways that reserve the Authorization header |
| Awesome MCP lists | PR to punkpeye/awesome-mcp-servers (and forks) under "Marketing" or "Search" |

Listing copy to reuse: "App Store intelligence and ASO for AI agents — keyword research with real Apple Search Popularity, competitor analysis, rank tracking, revenue estimates. Free tier works without an API key."

The keyless free tier is the wedge no competitor listing has — lead with it everywhere.

## 3. Skills registries

- [ ] `npx skills add trysonar/skills` works the moment the repo is public (the skills CLI resolves GitHub repos directly) — verify once live
- [ ] agentskills.io directory — Sonar already serves `/.well-known/agent-skills/index.json`; add this repo as a second advertised skill source if the spec allows multiple

## 4. Cross-link from Sonar properties (SEO loop)

- [ ] trysonar.app/docs/mcp → "Agent skills" section linking the repo
- [ ] agent-skill.md + llms.txt → mention the skills repo
- [ ] Blog post: "Teach your AI agent ASO" — install guide + example transcript (targets "aso mcp", "app store mcp", "aso ai agent")
- [ ] Changelog entry (user-facing: "Sonar skills for Claude Code and Cursor")

## 5. Social proof flywheel (what actually converted for AppKittie)

- [ ] Launch thread on X: video/gif of Claude Code running /keyword-research end-to-end
- [ ] Ask the existing heavy API/MCP users (the 100%-retention segment) to try a skill and quote-post their reaction
- [ ] Reply with the repo link wherever "how do I do ASO with Claude" comes up (X, r/AppStoreOptimization, r/iOSProgramming)

## Maintenance rule

The skills describe the live API. When a v1 endpoint or MCP tool changes, this repo is part of the update checklist alongside docs/API.md, docs/MCP.md, openapi.json, agent-skill.md, and llms-full.txt.

## 6. Sibling repo fixes

- [ ] trysonar/mcp README says "32 ASO tools" — now 47 (screenshots + charts + newer writes); refresh from packages/mcp
- [ ] trysonar/cli README — verify it reflects CLI 0.6.0 surface
- [ ] ClawHub listing (clawhub.ai/petersutarik/sonar-aso) — consider publishing the workflow skills there too, not just the API-reference skill
