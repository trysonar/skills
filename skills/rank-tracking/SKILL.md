---
name: rank-tracking
description: When the user wants ongoing, automated monitoring — daily keyword rank tracking, competitor tracking, rank history, "how is my app doing", alerts on rank drops and competitor moves, or a multi-app portfolio view for the App Store or Google Play. Also use when the user mentions "track my rankings", "rank tracker", "rank history", "set up alerts", "monitor competitors", "my dashboard", or "did my update move rankings". Requires a Sonar Indie or Agency plan (a trial counts). For one-off keyword scoring, see keyword-research.
metadata:
  version: 1.1.0
---

# Rank tracking

You are an expert ASO operator. Your goal is to set up and interpret continuous rank tracking in Sonar, so ASO changes are measured instead of guessed — this is what turns one-off research into a feedback loop.

## Access check

Every tool in this skill works on the user's Sonar workspace and needs an Indie or Agency plan (an active trial counts). On the Sonar connector the user signs in once with their Sonar account; write tools then work without any extra key. (With the local `@sonarapp/mcp` server or a raw API key, write tools need a key created with the write scope.) `sonar_portfolio` needs the Agency plan. If a tool reports a missing plan or sign-in, relay its message plainly.

Workspace ids are Sonar UUIDs, not store ids: get `product_id` from `sonar_list_products`, app `id` from `sonar_list_apps`, tracked-keyword `id` and `keyword_id` from `sonar_app_keywords`.

## "How is my app doing?"

Read these before computing anything yourself:

1. `sonar_app_overview` (own `app_id`, `days` 7–90, default 30) — visibility index and share of voice with 7-day deltas, ranked and top-10 counts, best rank, biggest 7-day improvements and drops, the rank-distribution trend, and the opportunity list (`near_page_one`, `top_three_push`, `easy_target`). Same numbers as the Sonar dashboard
2. `sonar_alert_events` — what changed recently (rank drops and gains, top-10 entries and exits, new rankings, rating drops, review spikes, competitor changes, top-chart moves). Filter by `type`, `app_id`, or `since` (ISO timestamp); events exist only for alert types the user has rules for, and detection runs once a day
3. `sonar_portfolio` (Agency plan) — for users with many apps: per-app KPIs, org-wide totals, biggest movers, a needs-attention list, and the best untracked keyword opportunities across all apps

## Setup workflow

### Step 1: Create the product

Check `sonar_list_products` first and never create duplicates. Then `sonar_create_product` with `apps`: one or two store versions (`{ store, store_id, country? }`, one iOS + one Android at most). It returns the product id and the Sonar app ids. To add the other store's version later, use `sonar_track_app`.

### Step 2: Track keywords

`sonar_track_keywords` (`app_id`, `keywords` up to 200 per call, optional `country`) with the strategy from `keyword-research` — primaries, secondaries, and 2–3 aspirationals per market. It's idempotent: already-tracked terms come back as `already_tracked`. Track per country the app actually competes in. Sonar refreshes every tracked keyword's rank daily. For what to track next, read `sonar_discovered_keywords` — Sonar's verified ranked, gap, and idea suggestions with an `opportunity` score.

Plans cap the number of tracked apps and keywords; a plan-limit error states the cap — tell the user rather than retrying.

### Step 3: Track competitors

`sonar_track_competitor` (`product_id`, competitor `store`/`store_id`; the product must already have its own app in that store), then `sonar_scan_competitor` (`competitor_app_id`, `own_app_id`) to run AI competitor-keyword discovery — surfacing terms they rank for that the user never thought to check. Competitor ranks are recorded from the same daily SERPs.

### Step 4: Set alerts

`sonar_set_alert` upserts one rule per `type` and scope, so movement arrives by email digest instead of requiring dashboard visits. Types: `rank_drop`, `rank_gain`, `entered_top10`, `left_top10`, `new_ranking`, `rating_drop`, `review_spike`, `competitor_change`, `top_chart`. Omit `scope_app_id` for a rule covering all tracked apps, omit `threshold` for the per-type default; `countries` applies to `top_chart` only (up to 10 storefronts). `sonar_list_alerts` shows existing rules; `sonar_delete_alert` removes one.

### Step 5: Annotate

`sonar_star_keyword` marks the keywords the user is actively pursuing; `sonar_update_keyword_note` records why a keyword is tracked or what changed ("added to subtitle in v2.3"), which makes later rank moves attributable.

## Interpretation workflow

- `sonar_app_rankings` (`app_id`, `days` up to 365, optional `keyword_id`) — daily rank history per keyword. Read the observations: `ranked` has a numeric rank, `not_found` means a completed search didn't return the app, `not_observed` means no confirmed check that day. Never treat a missing rank as a collection failure or invent a number for it. Metadata changes usually show effects in 3–14 days
- `sonar_keyword_rankings` (`keyword_id`) — who held the top spots on one keyword over time; distinguishes "you dropped" from "someone else surged"
- `sonar_app_changes` — release, metadata, screenshot, price, and category changes for the user's app or a competitor; correlate them with rank shifts to reverse-engineer what worked
- `sonar_get_app` — up to 90 daily snapshots of rating, review count, version, and installs
- Judge trends over 7+ days, not day-to-day jitter; a one-day dip is noise, a week-long slide after a competitor's update is signal

## Cleanup

`sonar_delete_tracked_keyword`, `sonar_untrack_keywords` (`all: true` or `ids`), `sonar_remove_competitor`, `sonar_untrack_app`, and `sonar_delete_product` remove tracking. Confirm with the user before any of them — they stop data collection for what's removed.

## Output format

**After setup:** what's now tracked (apps, keyword count per market, competitors, alerts) and when the first full data lands (next daily cycle).

**For interpretation:** finding first ("Your update on the 3rd moved 8 of 12 primaries up, median +4"), then the evidence table:

| Keyword | Country | Rank | 7d Δ | 30d Δ | Note |
|-|-|-|-|-|-|

**Maintenance cadence:** monthly — retire keywords stuck below #50 for 60 days, promote long-tails that reached the top 10, re-run `keyword-research` on freed slots.

## Related skills

- `keyword-research` — decide what deserves a tracking slot
- `competitor-analysis` — act on the competitor moves tracking surfaces
- `metadata-optimization` — ship changes, then measure them here
