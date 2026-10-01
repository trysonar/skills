---
name: sonar-aso
description: Reference for Sonar's App Store Optimization MCP tools — exact tool names, required parameters, id formats, credit costs, and which tools work without an account, need a plan, or are local-only. Use when you need to pick the right sonar_ tool or fill its arguments for app store keyword research, keyword difficulty or popularity, app lookup and search, ASO scores, reviews, revenue estimates, top charts, rank tracking, alerts, or Screenshot Studio on the iOS App Store or Google Play.
homepage: https://trysonar.app/docs/mcp
license: MIT
metadata:
  version: 1.1.0
  openclaw:
    homepage: https://trysonar.app
---

# Sonar MCP tool reference

Sonar answers app-store questions with real data instead of guesses: how hard a keyword is to rank for, how much search demand it has, what a listing is missing, what users complain about, and roughly what an app earns. This reference lists every Sonar MCP tool. For workflows (what to call in which order), load the focused skills: `keyword-research`, `metadata-optimization`, `competitor-analysis`, `aso-audit`, `review-analysis`, `revenue-analysis`, `market-discovery`, `rank-tracking`, `screenshot-studio`, `app-marketing-context`.

## Connection

- **In Claude:** the Sonar plugin bundles the hosted server at `https://trysonar.app/mcp`. Free-tier tools work immediately with no account. Everything else asks the user to connect their Sonar account once (OAuth sign-in); no API key is needed or stored
- **Other MCP clients:** the same hosted URL, or the local stdio server `npx -y @sonarapp/mcp`. Docs: https://trysonar.app/docs/mcp
- **No MCP client:** the REST API at `https://trysonar.app/api/v1` exposes the same capabilities. Docs: https://trysonar.app/docs/api and https://trysonar.app/openapi.json

Never ask the user to paste an API key into the conversation, and never print or log one.

## Conventions

- `store`: `ios` or `android`. `country`: ISO 3166-1 alpha-2, default `us`. Metrics differ per store and country
- `store_id`: iOS numeric App Store id (e.g. `389801252`); Android package name (e.g. `com.spotify.music`)
- Workspace tools take Sonar UUIDs (`app_id`, `product_id`, `keyword_id`, tracked-keyword `id`) from the list tools — never a store id
- Costs below are API credits for accounts on API Credits; Indie and Agency plans use a daily request allowance instead

## Free tier (no account, per-IP daily limits)

| Tool | Required args | Notes |
|-|-|-|
| `sonar_app_search` | `query`, `store` | Store search results in ranking order; `num` 1–50 (default 10) |
| `sonar_app_lookup` | `store`, `store_id` | Metadata, rating, rating count (`reviews`), price; `installs` on Android |
| `sonar_app_aso_score` | `store`, `store_id` | 0–100 `score` + `checks[]` with `status` and `tip` |
| `sonar_app_extract_keywords` | `store`, `store_id` | Likely target keywords from the listing; `max` 1–50 |
| `sonar_keyword_suggestions` | `seed`, `store` | Store autocomplete terms with a priority score |
| `sonar_top_charts` | `store` | Free/paid/grossing chart (`chart`, `category`, `limit` ≤200) with day-over-day movement |
| `sonar_keyword_metrics` | `store` + `keyword` or `keywords` (≤25) | Free tier: 5 keywords/day |
| `sonar_screenshot_layout_guide` | — | Screenshot layout format reference |

The first six share 30 calls per day per IP. With a signed-in account each costs 1 credit.

## Research (signed-in account)

| Tool | Required args | Cost | Returns |
|-|-|-|-|
| `sonar_keyword_metrics` | `store` + `keyword` or `keywords` (≤25) | 1 per keyword | `popularity`, `popularity_source`, `difficulty`, `difficulty_breakdown` (incl. `beatable`), `est_downloads_at_1` (iOS); `pending` terms are queued and not charged |
| `sonar_keyword_search` | `query`, `store` | 10 | Seed plus related autocomplete keywords, each with difficulty, popularity, results count |
| `sonar_app_reviews` | `store`, `store_id` | 1 | Reviews; `sort` recent/helpful, `min_rating`, `max_rating`, `limit` ≤200, `lang` (Android) |
| `sonar_app_revenue` | `store`, `store_id` | 1 | Monthly revenue estimate with `confidence` grade, factors, and methodology (one app per call) |

Prefer `sonar_keyword_metrics` when the keywords are known; `sonar_keyword_search` is for discovering new ones. Difficulty ≤30 with popularity ≥40 is the classic opportunity zone for small apps, and `difficulty_breakdown.beatable` flags winnable top-3 slots.

## Workspace reads (Indie or Agency plan; trial counts)

| Tool | Required args | Returns |
|-|-|-|
| `sonar_list_products` | — | Products with linked store versions and competitor counts |
| `sonar_list_apps` | — | Tracked own and competitor apps with latest snapshot (cursor-paginated) |
| `sonar_get_app` | `app_id` | Metadata plus up to 90 daily snapshots |
| `sonar_app_overview` | `app_id` | Dashboard scoreboard: visibility, share of voice, movers, opportunities |
| `sonar_app_keywords` | `app_id` | Tracked keywords with metrics, notes, `starred_at`, ids |
| `sonar_app_rankings` | `app_id` | Daily rank history; `days` ≤365, optional `keyword_id` |
| `sonar_keyword_rankings` | `keyword_id` | Top apps on one tracked keyword over time |
| `sonar_app_changes` | `app_id` | Releases, metadata, screenshot, price, category changes |
| `sonar_competitor_keywords` | `competitor_app_id` | Keywords a competitor ranks for; `own_app_id` adds gap markers |
| `sonar_competitor_landscape` | `app_id` (own) | Gaps, threats, leads, and the latest AI insight |
| `sonar_discovered_keywords` | `app_id` | Untracked keyword suggestions with `opportunity` and `ai_relevance` |
| `sonar_review_insights` | `app_id` | Latest AI review analysis per `country` |
| `sonar_list_alerts` | — | Alert rules |
| `sonar_alert_events` | — | Detected alert events; filter by `type`, `app_id`, `since` |

## Agency plan

| Tool | Required args | Returns |
|-|-|-|
| `sonar_portfolio` | — | Per-app KPIs, totals, movers, needs-attention list across all apps |
| `sonar_app_sales` | `app_id` (own iOS) | App Store Connect downloads, redownloads, IAP units, approximate USD proceeds |
| `sonar_app_engagement` | `app_id` (own iOS) | Impressions → page views → downloads funnel, sources, installs, deletions, sessions |

The two App Store Connect tools need a connection at trysonar.app/settings/connections. Check `status` — anything but `ready` means no data.

## Workspace writes (Indie or Agency plan)

| Tool | Required args | Notes |
|-|-|-|
| `sonar_create_product` | `apps` (1–2 of `{store, store_id}`) | Starts tracking; returns product and app ids |
| `sonar_track_app` | `product_id`, `store`, `store_id` | Links the other store's version |
| `sonar_track_competitor` | `product_id`, `store`, `store_id` | Competitor must be in a store the product covers |
| `sonar_track_keywords` | `app_id`, `keywords` (≤200) | Idempotent |
| `sonar_scan_competitor` | `competitor_app_id`, `own_app_id` | AI keyword discovery on a competitor |
| `sonar_analyze_competitors` | `app_id` (own) | Fresh AI landscape insight; paid non-trial plan, once per 7 days |
| `sonar_generate_review_insights` | `app_id` | Fresh AI review analysis; paid non-trial plan, once per 90 days per country |
| `sonar_set_alert` | `type` | Upsert per type and scope |
| `sonar_star_keyword` | `tracked_keyword_id`, `starred` | |
| `sonar_update_keyword_note` | `tracked_keyword_id`, `note` | `null` or empty clears |
| `sonar_delete_tracked_keyword` | `tracked_keyword_id` | |
| `sonar_untrack_keywords` | `app_id` + `all` or `ids` | |
| `sonar_untrack_app` | `app_id` | |
| `sonar_delete_product` | `product_id` | |
| `sonar_remove_competitor` | `product_id`, `competitor_app_id` | |
| `sonar_delete_alert` | `id` | |

Confirm with the user before any untrack or delete.

## Screenshot Studio (Indie or Agency plan)

| Tool | Required args |
|-|-|
| `sonar_screenshot_devices` | — |
| `sonar_list_screenshot_sets` | `product_id` |
| `sonar_get_screenshot_set` | `set_id` |
| `sonar_create_screenshot_set` | `product_id`, `store`, `device_size` |
| `sonar_update_screenshot_set` | `set_id` |
| `sonar_delete_screenshot_set` | `set_id` |
| `sonar_add_screenshot` | `set_id` |
| `sonar_update_screenshot` | `screenshot_id`, `layout` |
| `sonar_delete_screenshot` | `screenshot_id` |
| `sonar_set_screenshot_translations` | `set_id`, `locale`, `entries` |
| `sonar_export_screenshots` | `set_id`, `output_dir` — local stdio server only, not on the hosted connector |
