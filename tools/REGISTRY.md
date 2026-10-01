# Tool registry — skill × tool matrix

Which Sonar MCP tools each skill uses. Tool names are the MCP names; the REST path follows in parentheses where one exists. Covers all 55 tools of `@sonarapp/mcp` 0.10.0; the hosted server at `https://trysonar.app/mcp` (bundled with the Claude plugin) serves 54 of them — `sonar_export_screenshots` is local-only. Full parameter reference: https://trysonar.app/docs/mcp and https://trysonar.app/openapi.json.

## Access tiers

- **Free** — works with no account (per-IP daily limits: 30 calls/day shared, keyword metrics 5 keywords/day)
- **Account** — a signed-in Sonar account: API Credits (50 free on signup) or an Indie/Agency plan's daily allowance
- **Indie** — Indie or Agency plan (an active trial counts); workspace reads and writes
- **Paid Indie** — Indie or Agency plan, not on trial
- **Agency** — Agency plan only
- **Local** — only on the local stdio server (`npx -y @sonarapp/mcp`), not the hosted connector

Writes on the hosted connector authorize through the OAuth sign-in. With the local server or a raw API key, writes need a key with the write scope.

## Stateless research tools

| Tool (REST) | Tier | Credits | Used by |
|-|-|-|-|
| `sonar_app_search` (`GET /apps/search`) | Free | 1 | keyword-research, competitor-analysis, aso-audit, metadata-optimization, review-analysis, revenue-analysis, app-marketing-context |
| `sonar_app_lookup` (`GET /apps/lookup`) | Free | 1 | competitor-analysis, aso-audit, metadata-optimization, review-analysis, revenue-analysis, app-marketing-context |
| `sonar_app_aso_score` (`GET /apps/aso-score`) | Free | 1 | aso-audit, metadata-optimization, app-marketing-context |
| `sonar_app_extract_keywords` (`GET /apps/extract-keywords`) | Free | 1 | keyword-research, competitor-analysis, aso-audit, metadata-optimization, app-marketing-context |
| `sonar_keyword_suggestions` (`GET /keywords/suggestions`) | Free | 1 | keyword-research, aso-audit |
| `sonar_top_charts` (`GET /charts/top`) | Free | 1 | market-discovery, revenue-analysis |
| `sonar_keyword_metrics` (`GET /keywords/metrics`, ≤25) | Free (5 kw/day), Account | 1/kw | keyword-research, metadata-optimization, competitor-analysis, aso-audit, market-discovery, revenue-analysis, screenshot-studio |
| `sonar_keyword_search` (`GET /keywords/search`) | Account | 10 | keyword-research |
| `sonar_app_reviews` (`GET /apps/reviews`) | Account | 1 | review-analysis, competitor-analysis, aso-audit, market-discovery |
| `sonar_app_revenue` (`GET /apps/revenue`; REST takes `ids` ≤25) | Account | 1/app | revenue-analysis, competitor-analysis, market-discovery |

## Workspace reads

| Tool | Tier | Used by |
|-|-|-|
| `sonar_list_products`, `sonar_list_apps`, `sonar_get_app` | Indie | rank-tracking, keyword-research, competitor-analysis, review-analysis, app-marketing-context, screenshot-studio |
| `sonar_app_overview` | Indie | rank-tracking, aso-audit, app-marketing-context |
| `sonar_app_keywords`, `sonar_app_rankings`, `sonar_keyword_rankings` | Indie | rank-tracking |
| `sonar_app_changes` | Indie | rank-tracking, competitor-analysis |
| `sonar_competitor_keywords`, `sonar_competitor_landscape` | Indie | competitor-analysis |
| `sonar_discovered_keywords` | Indie | keyword-research, metadata-optimization, rank-tracking |
| `sonar_review_insights` | Indie | review-analysis, competitor-analysis, aso-audit |
| `sonar_list_alerts`, `sonar_alert_events` | Indie | rank-tracking |
| `sonar_portfolio` | Agency | rank-tracking |
| `sonar_app_sales` | Agency + App Store Connect (iOS) | revenue-analysis |
| `sonar_app_engagement` | Agency + App Store Connect (iOS) | aso-audit, metadata-optimization |

## Workspace writes

| Tool | Tier | Used by |
|-|-|-|
| `sonar_create_product`, `sonar_track_app`, `sonar_track_keywords` | Indie | rank-tracking, keyword-research, metadata-optimization |
| `sonar_track_competitor`, `sonar_scan_competitor` | Indie | rank-tracking, competitor-analysis |
| `sonar_analyze_competitors` | Paid Indie (once per 7 days per app) | competitor-analysis |
| `sonar_generate_review_insights` | Paid Indie (once per 90 days per app and country) | review-analysis |
| `sonar_set_alert`, `sonar_delete_alert` | Indie | rank-tracking, market-discovery |
| `sonar_star_keyword`, `sonar_update_keyword_note` | Indie | rank-tracking, metadata-optimization |
| `sonar_delete_tracked_keyword`, `sonar_untrack_keywords`, `sonar_untrack_app`, `sonar_delete_product`, `sonar_remove_competitor` | Indie | rank-tracking |

## Screenshot Studio

| Tool | Tier | Used by |
|-|-|-|
| `sonar_screenshot_layout_guide` | Free | screenshot-studio |
| `sonar_screenshot_devices` | Indie | screenshot-studio |
| `sonar_list_screenshot_sets`, `sonar_get_screenshot_set` | Indie | screenshot-studio |
| `sonar_create_screenshot_set`, `sonar_update_screenshot_set`, `sonar_delete_screenshot_set` | Indie | screenshot-studio |
| `sonar_add_screenshot`, `sonar_update_screenshot`, `sonar_delete_screenshot` | Indie | screenshot-studio |
| `sonar_set_screenshot_translations` | Indie | screenshot-studio |
| `sonar_export_screenshots` | Indie, Local | screenshot-studio |

`sonar-aso` is the reference skill and lists every tool above.
