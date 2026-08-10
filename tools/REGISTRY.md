# Tool registry — skill × tool matrix

Which Sonar MCP tools (or REST endpoints) each skill uses. Tool names are the MCP names; the REST path follows in parentheses. Full parameter reference: https://trysonar.app/openapi.json.

## Access tiers

- **Keyless** — works with no API key (per-IP daily limits: 30/day shared, keyword metrics 5 keywords/day)
- **Credits** — any account with credit balance (50 free on signup)
- **Sub** — Sonar subscription (workspace reads)
- **Write** — subscription + write-scope API key

## Stateless research tools

| Tool (REST) | Tier | Cost | Used by |
|-|-|-|-|
| `sonar_app_search` (`GET /apps/search`) | Keyless | 1 | keyword-research, competitor-analysis, aso-audit, revenue-analysis, market-discovery, app-marketing-context |
| `sonar_app_lookup` (`GET /apps/lookup`) | Keyless | 1 | competitor-analysis, aso-audit, metadata-optimization, revenue-analysis, app-marketing-context |
| `sonar_app_aso_score` (`GET /apps/aso-score`) | Keyless | 1 | aso-audit, metadata-optimization, app-marketing-context |
| `sonar_app_extract_keywords` (`GET /apps/extract-keywords`) | Keyless | 1 | keyword-research, competitor-analysis, aso-audit, metadata-optimization, app-marketing-context |
| `sonar_keyword_suggestions` (`GET /keywords/suggestions`) | Keyless | 1 | keyword-research, aso-audit |
| `sonar_keyword_metrics` (`GET /keywords/metrics`, bulk ≤25) | Keyless (5 kw/day) | 1/kw | keyword-research, metadata-optimization, competitor-analysis, aso-audit, market-discovery, screenshot-studio |
| `sonar_keyword_search` (`GET /keywords/search`) | Credits | 10 | keyword-research |
| `sonar_app_reviews` (`GET /apps/reviews`) | Credits | 1 | review-analysis, competitor-analysis, aso-audit, market-discovery |
| `sonar_app_revenue` (`GET /apps/revenue`, bulk ≤25) | Credits | 1/app | revenue-analysis, competitor-analysis, market-discovery |
| `sonar_top_charts` (`GET /charts/top`) | Keyless | 1 | market-discovery, revenue-analysis |

## Workspace reads (Sub)

| Tool | Used by |
|-|-|
| `sonar_list_products`, `sonar_list_apps`, `sonar_get_app` | rank-tracking |
| `sonar_app_keywords`, `sonar_app_rankings`, `sonar_keyword_rankings` | rank-tracking |
| `sonar_app_changes` | rank-tracking, competitor-analysis |
| `sonar_competitor_keywords`, `sonar_competitor_landscape` | competitor-analysis |
| `sonar_list_alerts` | rank-tracking |

## Writes (Write scope)

| Tool | Used by |
|-|-|
| `sonar_create_product`, `sonar_track_app`, `sonar_track_keywords` | rank-tracking |
| `sonar_track_competitor`, `sonar_scan_competitor`, `sonar_analyze_competitors` | rank-tracking, competitor-analysis |
| `sonar_set_alert`, `sonar_delete_alert` | rank-tracking |
| `sonar_star_keyword`, `sonar_update_keyword_note` | rank-tracking |
| `sonar_untrack_keywords`, `sonar_delete_tracked_keyword`, `sonar_untrack_app`, `sonar_delete_product`, `sonar_remove_competitor` | rank-tracking |

## Screenshot Studio (Sub; mutations Write)

| Tool | Used by |
|-|-|
| `sonar_screenshot_layout_guide`, `sonar_screenshot_devices` | screenshot-studio |
| `sonar_list_screenshot_sets`, `sonar_get_screenshot_set`, `sonar_create_screenshot_set`, `sonar_delete_screenshot_set` | screenshot-studio |
| `sonar_add_screenshot`, `sonar_update_screenshot`, `sonar_delete_screenshot` | screenshot-studio |
| `sonar_set_screenshot_translations`, `sonar_export_screenshots` | screenshot-studio |
