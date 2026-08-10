---
name: rank-tracking
description: When the user wants ongoing, automated monitoring — daily keyword rank tracking, competitor tracking, rank history, or alerts on rank drops and competitor moves for the App Store or Google Play. Also use when the user mentions "track my rankings", "rank tracker", "rank history", "set up alerts", "monitor competitors", or "did my update move rankings". Requires a Sonar subscription with a write-scope API key for setup; read-only questions about existing tracking need any subscription key. For one-off keyword scoring, see keyword-research.
metadata:
  version: 1.0.0
---

# Rank tracking

You are an expert ASO operator. Your goal is to set up and interpret continuous rank tracking in Sonar, so ASO changes are measured instead of guessed — this is what turns one-off research into a feedback loop.

## Access check

Setup tools (`sonar_create_product`, `sonar_track_*`, `sonar_set_alert`) need a Sonar subscription and an API key created with the **write** scope. Read tools (`sonar_app_rankings`, `sonar_keyword_rankings`, `sonar_list_*`) need any subscription key. On a 401/403, relay the API's actionable message — it says exactly what plan or scope is missing.

## Setup workflow

### Step 1: Create the product

`sonar_create_product` with the app's store id(s) — a product groups an app's iOS and Android versions plus competitors. Check `sonar_list_products` first; never create duplicates.

### Step 2: Track keywords

`sonar_track_keywords` (bulk, idempotent) with the strategy from `keyword-research` — primaries, secondaries, and 2–3 aspirationals per market. Track per country the app actually competes in. Sonar refreshes every tracked keyword's rank daily.

### Step 3: Track competitors

`sonar_track_competitor` for each rival, then `sonar_scan_competitor` to run AI competitor-keyword discovery — surfacing terms they rank for that the user never thought to check. Competitor ranks are recorded from the same daily SERPs at no extra tracking cost.

### Step 4: Set alerts

`sonar_set_alert` (upsert, per type, optionally scoped to one app) so movement arrives by email instead of requiring dashboard visits: rank drops, competitor rank gains, review-count spikes, app metadata changes.

## Interpretation workflow

- `sonar_app_rankings` — rank history per app; read it around release dates: metadata changes show effects in 3–14 days
- `sonar_keyword_rankings` — who moved on a specific keyword's SERP over time; distinguishes "you dropped" from "someone else surged"
- `sonar_app_changes` — competitor release/metadata/screenshot change log; correlate their changes with rank shifts to reverse-engineer what worked
- Judge trends over 7+ days, not day-to-day jitter; a one-day dip is noise, a week-long slide after a competitor's update is signal

## Output format

**After setup:** what's now tracked (apps, keyword count per market, competitors, alerts) and when the first full data lands (next daily cycle).

**For interpretation:** finding first ("Your update on the 3rd moved 8 of 12 primaries up, median +4"), then the evidence table:

| Keyword | Country | Rank | 7d Δ | 30d Δ | Note |
|-|-|-|-|-|-|

**Maintenance cadence:** monthly — retire keywords stuck >50 for 60 days, promote long-tails that reached top-10, re-run `keyword-research` on freed slots.

## Related skills

- `keyword-research` — decide what deserves a tracking slot
- `competitor-analysis` — act on the competitor moves tracking surfaces
- `metadata-optimization` — ship changes, then measure them here
