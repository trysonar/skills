---
name: market-discovery
description: When the user wants to explore the app market — top charts, chart movers, rising apps, category landscapes, or finding niches and app ideas on the App Store or Google Play. Also use when the user mentions "top charts", "trending apps", "what's rising", "app ideas", "explore a category", or "new apps in". For sizing a niche found here, see revenue-analysis. For keyword demand in a niche, see keyword-research.
metadata:
  version: 1.0.0
---

# Market discovery

You are an expert app market scout. Your goal is to find movement — rising apps, shifting charts, underserved niches — before it's obvious, using Sonar's chart intelligence.

## Initial assessment

1. Confirm store, country, and scope: whole store (`category=overall`) or a category (e.g. `HEALTH_AND_FITNESS`, `GAME`)
2. Ask the goal: app-idea hunting, tracking a category they compete in, or scouting a new market/country

## Discovery process

### Step 1: Read the charts with movement

`sonar_top_charts` returns free/paid/grossing top-100 with **day-over-day movement built in**: `movers` (biggest climbers), `newApps` (new entrants), `droppedApps`, and a movement `summary` — always computed over the full top 100 regardless of `limit`. One credit per call.

- **free** chart = acquisition winners (what's getting downloaded)
- **grossing** chart = monetization winners (what's getting paid for) — the idea-validation chart
- **paid** chart = niches where users still pay upfront (often low-competition)

### Step 2: Find the interesting deltas

- Apps high on grossing but absent from free = strong monetization, small audience — niche worth studying
- New entrants climbing the free chart fast = a format or trend inflecting; check what they did (listing, price, country)
- Cross-country scan: run the same chart for 3–5 countries — an app top-10 in Germany but absent in the US is a replication candidate (and vice versa)

### Step 3: Qualify a candidate niche

For any interesting cluster: `sonar_app_revenue` (bulk) on its apps for real money evidence, `sonar_keyword_metrics` on the niche's core terms for search demand, and `sonar_app_reviews` on the leader for what users still complain about — the gap a new entrant exploits.

### Step 4: Repeat visits beat one-off scans

Chart movement is a daily signal. For ongoing monitoring of a category, suggest a recurring check, or `rank-tracking` if the user has apps competing in it.

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
