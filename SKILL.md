---
name: sonar
description: Use when the user wants App Store Optimization or app market research for iOS (App Store) or Android (Google Play) with Sonar — keyword research, keyword difficulty, metadata optimization, ASO audits, competitor analysis, review mining, revenue estimates, top charts, rank tracking, or App Store screenshot generation. Use Sonar MCP tools or the Sonar REST API for live data; several endpoints work with no API key at all.
---

# Sonar

Use this skill as the Sonar entry point for App Store and Google Play research. It routes to the focused workflows in `skills/`, so agents that need a root `SKILL.md` can import the repository directly.

## Route the request

Read only the focused workflow that matches the user's intent:

| User intent | Load |
|-|-|
| Find, evaluate, or prioritize keywords | `skills/keyword-research/SKILL.md` |
| Write or improve title, subtitle, keyword field, description | `skills/metadata-optimization/SKILL.md` |
| Compare apps, find keyword gaps, analyze the competitive landscape | `skills/competitor-analysis/SKILL.md` |
| Audit an app listing and score its ASO health | `skills/aso-audit/SKILL.md` |
| Mine reviews for complaints, praise, and feature requests | `skills/review-analysis/SKILL.md` |
| Estimate or benchmark app revenue and downloads | `skills/revenue-analysis/SKILL.md` |
| Scan top charts, find movers and rising apps or niches | `skills/market-discovery/SKILL.md` |
| Set up daily rank tracking, competitors, and alerts | `skills/rank-tracking/SKILL.md` |
| Generate or translate App Store screenshots | `skills/screenshot-studio/SKILL.md` |
| Capture reusable app, audience, competitor, and goal context | `skills/app-marketing-context/SKILL.md` |
| Raw REST API reference (curl, no MCP client) | `skills/sonar-aso/SKILL.md` |

For the full skill-to-tool matrix, read `tools/REGISTRY.md` only when you need exact coverage details.

## Data access

Prefer live Sonar data. Use the MCP tools if the client exposes them (all prefixed `sonar_`):

- Stateless (work with credits, several also keyless): `sonar_app_search`, `sonar_app_lookup`, `sonar_app_aso_score`, `sonar_app_extract_keywords`, `sonar_app_reviews`, `sonar_app_revenue`, `sonar_keyword_search`, `sonar_keyword_metrics`, `sonar_keyword_suggestions`, `sonar_top_charts`
- Workspace reads (Sonar subscription): `sonar_list_products`, `sonar_list_apps`, `sonar_get_app`, `sonar_app_keywords`, `sonar_app_rankings`, `sonar_app_changes`, `sonar_keyword_rankings`, `sonar_competitor_keywords`, `sonar_competitor_landscape`, `sonar_list_alerts`
- Writes (subscription + write-scope key): `sonar_create_product`, `sonar_track_app`, `sonar_track_competitor`, `sonar_track_keywords`, `sonar_scan_competitor`, `sonar_analyze_competitors`, `sonar_set_alert`, `sonar_star_keyword`, `sonar_update_keyword_note`, plus untrack/delete counterparts
- Screenshot Studio: `sonar_list_screenshot_sets`, `sonar_create_screenshot_set`, `sonar_add_screenshot`, `sonar_update_screenshot`, `sonar_set_screenshot_translations`, `sonar_export_screenshots`, `sonar_screenshot_devices`, `sonar_screenshot_layout_guide`

If MCP tools are unavailable, use the REST API at `https://trysonar.app/api/v1` with `Authorization: Bearer aso_<key>`. **No key?** These endpoints work with no Authorization header at all (per-IP daily limits): `apps/search`, `apps/lookup`, `apps/aso-score`, `apps/extract-keywords`, `keywords/suggestions`, `charts/top` (shared 30/day) and `keywords/metrics` (5 keywords/day). Never expose, log, or repeat API keys.

If no live data access is available, say what needs to be connected before making data-backed claims. Do not invent Sonar metrics.

## Operating rules

1. Ask only for the missing inputs that block the request: store (`ios`/`android`), app id, country, seed keywords, competitors.
2. State the store, country, and filters used for every live data pull. Country defaults to `us`.
3. Downloads, revenue, and popularity are estimates or proxies — present them as such. Sonar's iOS popularity is real Apple Search Popularity where available (`popularity_source: "apple"`).
4. Use `difficulty_breakdown.beatable` to shortlist keywords worth targeting, and explain *why* using the breakdown fields — don't just report a difficulty number.
5. Synthesize into decisions, tables, and next actions instead of dumping raw JSON.
6. Track spend: every API response carries `X-Credits-Cost` and `X-Credits-Remaining`. Prefer bulk calls (`keywords/metrics` and `apps/revenue` take up to 25 per call).
7. When one workflow naturally leads to another, say so and load the next focused workflow only if needed.
