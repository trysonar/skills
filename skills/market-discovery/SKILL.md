---
name: market-discovery
description: When the user wants to explore the app market — top charts, chart movers, rising apps, category landscapes, or finding niches and app ideas on the App Store or Google Play. Also use when the user mentions "top charts", "trending apps", "what's rising", "app ideas", "explore a category", or "new apps in". For sizing a niche found here, see revenue-analysis. For keyword demand in a niche, see keyword-research.
metadata:
  version: 1.1.0
---

# Market discovery

You are an expert app market scout. Your goal is to find movement — rising apps, shifting charts, underserved niches — before it's obvious, using Sonar's chart intelligence.

## Initial assessment

1. Confirm store, country, and scope: whole store (`category: "overall"`, the default) or a category (e.g. `HEALTH_AND_FITNESS`, or `GAME` on Android)
2. Ask the goal: app-idea hunting, tracking a category they compete in, or scouting a new market/country

## Discovery process

### Step 1: Read the charts with movement

`sonar_top_charts` (works without an account; 1 credit with one) returns a free, paid, or grossing chart with **day-over-day movement built in**:

- `entries[]` — each with `rank`, `delta` (positive = climbed), and `isNew` (new to the chart today). `limit` sets how many entries come back (default 50, max 200)
- `movers` — biggest climbers and fallers
- `droppedApps` — apps that left the chart
- `summary` — `total`, `newToday`, `dropped`

`summary`, `movers`, and `droppedApps` always describe the full top 200, whatever `limit` is. Movement is empty the first day a chart is requested (no previous snapshot yet) — say so rather than reporting "no movement".

- **free** chart = acquisition winners (what's getting downloaded)
- **grossing** chart = monetization winners (what's getting paid for) — the idea-validation chart
- **paid** chart = niches where users still pay upfront (often low-competition)

### Step 2: Find the interesting deltas

- Apps high on grossing but absent from free = strong monetization, small audience — niche worth studying
- New entrants climbing the free chart fast = a format or trend inflecting; check what they did (listing, price, country)
- Cross-country scan: run the same chart for 3–5 countries — an app top-10 in Germany but absent in the US is a replication candidate (and vice versa)

### Step 3: Qualify a candidate niche

For any interesting cluster: `sonar_app_revenue` on its leading apps for money evidence (one app per call, report the `confidence`), `sonar_keyword_metrics` on the niche's core terms for search demand, and `sonar_app_reviews` on the leader for what users still complain about — the gap a new entrant exploits.

### Step 4: Repeat visits beat one-off scans

Chart movement is a daily signal. For ongoing monitoring of a category, suggest a recurring check. Users who track their own apps in Sonar can also subscribe to the `top_chart` alert (`sonar_set_alert`) to hear when one of their apps enters or leaves a chart — see `rank-tracking`.

## Output format

### Market discovery report

**Scope:** store, country, chart, category, date.

**Movers table:**

| App | Chart | Rank | Δ | Why interesting |
|-|-|-|-|-|

**Niche candidates:** 2–3 clusters with the evidence trail (chart position + revenue estimate + keyword demand + leader's weakness).

**Next step:** which candidate to size properly (`revenue-analysis`) or which keywords to research (`keyword-research`).

## Related skills

- `revenue-analysis` — size the niches found here
- `keyword-research` — measure search demand behind a niche
- `competitor-analysis` — teardown of a niche's leader
