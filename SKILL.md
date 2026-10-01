---
name: sonar
description: Use when the user wants App Store Optimization or app market research for iOS (App Store) or Android (Google Play) with Sonar — keyword research, keyword difficulty, metadata optimization, ASO audits, competitor analysis, review mining, revenue estimates, top charts, rank tracking, or App Store screenshot generation. Use the Sonar MCP tools for live data; several tools work with no account at all.
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
| Estimate or benchmark app revenue, or read App Store Connect sales | `skills/revenue-analysis/SKILL.md` |
| Scan top charts, find movers and rising apps or niches | `skills/market-discovery/SKILL.md` |
| Track ranks, read how a tracked app is doing, competitors, alerts, portfolio | `skills/rank-tracking/SKILL.md` |
| Generate or translate App Store screenshots | `skills/screenshot-studio/SKILL.md` |
| Capture reusable app, audience, competitor, and goal context | `skills/app-marketing-context/SKILL.md` |
| Exact tool names, parameters, costs, and access tiers | `skills/sonar-aso/SKILL.md` |

For the full skill-to-tool matrix, read `tools/REGISTRY.md` only when you need exact coverage details.

## Data access

Prefer live Sonar data through the MCP tools (all prefixed `sonar_`). In Claude, the Sonar plugin bundles the hosted server at `https://trysonar.app/mcp`:

- **No account (free tier, per-IP daily limits):** `sonar_app_search`, `sonar_app_lookup`, `sonar_app_aso_score`, `sonar_app_extract_keywords`, `sonar_keyword_suggestions`, `sonar_top_charts` (shared 30 calls/day), `sonar_keyword_metrics` (5 keywords/day), and `sonar_screenshot_layout_guide`
- **Signed-in account (credits or plan quota):** `sonar_keyword_search` (10 credits), `sonar_keyword_metrics` (1 credit per keyword, no daily cap), `sonar_app_reviews`, `sonar_app_revenue`
- **Workspace reads (Indie or Agency plan; trial counts):** `sonar_list_products`, `sonar_list_apps`, `sonar_get_app`, `sonar_app_overview`, `sonar_app_keywords`, `sonar_app_rankings`, `sonar_app_changes`, `sonar_keyword_rankings`, `sonar_competitor_keywords`, `sonar_competitor_landscape`, `sonar_discovered_keywords`, `sonar_review_insights`, `sonar_list_alerts`, `sonar_alert_events`
- **Agency plan only:** `sonar_portfolio`, plus `sonar_app_sales` and `sonar_app_engagement` (iOS, needs an App Store Connect connection)
- **Workspace writes (Indie or Agency plan):** `sonar_create_product`, `sonar_track_app`, `sonar_track_competitor`, `sonar_track_keywords`, `sonar_scan_competitor`, `sonar_analyze_competitors`, `sonar_generate_review_insights`, `sonar_set_alert`, `sonar_star_keyword`, `sonar_update_keyword_note`, plus the untrack/delete counterparts
- **Screenshot Studio:** `sonar_screenshot_layout_guide`, `sonar_screenshot_devices`, `sonar_list_screenshot_sets`, `sonar_get_screenshot_set`, `sonar_create_screenshot_set`, `sonar_update_screenshot_set`, `sonar_delete_screenshot_set`, `sonar_add_screenshot`, `sonar_update_screenshot`, `sonar_delete_screenshot`, `sonar_set_screenshot_translations`, and `sonar_export_screenshots` (local stdio server only)

If a tool asks the user to connect their Sonar account, that is the OAuth sign-in on the Sonar connector — tell the user, then retry. If no live data access is available, say what needs to be connected before making data-backed claims. Do not invent Sonar metrics.

## Operating rules

1. Ask only for the missing inputs that block the request: store (`ios`/`android`), app id, country, seed keywords, competitors.
2. Store ids: iOS uses the numeric App Store id (e.g. `389801252`), Android the package name (e.g. `com.spotify.music`). Workspace tools take Sonar UUIDs (`app_id`, `product_id`) from `sonar_list_apps` / `sonar_list_products` — never pass a store id where a UUID is expected.
3. State the store, country, and filters used for every live data pull. Country defaults to `us`.
4. Revenue and popularity are estimates — present them as such. iOS popularity is real Apple Search Popularity where available (`popularity_source: "apple"`). App Store Connect numbers from `sonar_app_sales` are the app's own reported data; proceeds are converted to USD approximately.
5. Use `difficulty_breakdown.beatable` to shortlist keywords worth targeting, and explain *why* using the breakdown fields — don't just report a difficulty number.
6. Synthesize into decisions, tables, and next actions instead of dumping raw JSON.
7. Prefer bulk calls: `sonar_keyword_metrics` takes up to 25 keywords per call, `sonar_track_keywords` up to 200.
8. When a tool reports a missing plan, sign-in, or credits, tell the user plainly what it needs. Don't pitch plans or purchases.
9. When one workflow naturally leads to another, say so and load the next focused workflow only if needed.
