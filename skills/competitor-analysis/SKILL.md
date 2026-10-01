---
name: competitor-analysis
description: When the user wants to analyze competitors, compare apps, find keyword gaps, or understand a competitive landscape on the App Store or Google Play. Also use when the user mentions "competitor analysis", "compare apps", "keyword gap", "who are my competitors", "benchmark my app", or "what are similar apps doing". For pure keyword scoring, see keyword-research. For revenue benchmarking, see revenue-analysis.
metadata:
  version: 1.1.0
---

# Competitor analysis

You are an expert competitive intelligence analyst for the app stores. Your goal is to map the user's competitive landscape, find exploitable weaknesses, and turn them into actions.

## Initial assessment

1. Check for `app-marketing-context.md` — read it for the app and known rivals
2. Identify the user's app (`sonar_app_lookup` / `sonar_app_search`)
3. Get 3–5 competitors, or discover them: `sonar_app_search` on the user's primary keywords — apps that repeatedly rank near the user ARE the competitors
4. Ask what matters most: positioning, keyword gaps, or monetization comparison
5. Check whether the app is tracked in Sonar (`sonar_list_apps`, Indie or Agency plan) — if so, the workspace path in Step 2 replaces most manual work

## Analysis framework

### Step 1: Gather intelligence per competitor

For each competitor, collect via `sonar_app_lookup`:

| Data point | What to look for |
|-|-|
| Title and subtitle | Keywords they target |
| Description | Value props, social proof |
| Rating and review count | Satisfaction, market traction |
| Installs (Android only) | Download scale |
| Category and price | Positioning, monetization model |

Add `sonar_app_revenue` for a monthly revenue estimate (one app per call, with a `confidence` grade and its factors — always report the confidence), and `sonar_app_reviews` with `max_rating: 2` for their weaknesses in users' own words.

### Step 2: Keyword gap analysis

**Manual path (any account):**

1. `sonar_app_extract_keywords` on each competitor AND the user's app
2. Union the lists, batch through `sonar_keyword_metrics` (`keywords`, 25 per call)
3. Gap = terms with real popularity that competitors target and the user doesn't; flag the `beatable: true` ones as immediate opportunities

**Workspace path (Indie or Agency plan):**

1. `sonar_track_competitor` adds each rival under the user's product (`product_id`, competitor `store_id`)
2. `sonar_scan_competitor` (`competitor_app_id` + the user's `own_app_id`) runs AI keyword discovery on the competitor and verifies the first batch inline; the rest verify in the background over the following hours
3. `sonar_competitor_keywords` with `own_app_id` returns what the competitor ranks for, with `gap=missing` on keywords the user doesn't rank for
4. `sonar_competitor_landscape` (the user's own `app_id`) is the one-call picture: gaps, winnable gaps, competitors climbing on the user's keywords, keywords the user leads, and the latest AI insight if one exists
5. `sonar_analyze_competitors` generates a fresh AI insight (opportunity clusters, threat narratives). It needs a paid, non-trial plan and runs at most once per app every 7 days — read the current one with `sonar_competitor_landscape` first
6. `sonar_app_changes` on a competitor's `app_id` shows their releases, metadata edits, screenshot swaps, and price moves — correlate with their rank gains

### Step 3: Positioning map

Plot traction (estimated revenue, Android installs, or rating count) against rating (satisfaction):

- High traction + high rating = **market leaders** — don't fight head-on, differentiate
- High traction + low rating = **vulnerable incumbents** — the opportunity quadrant; mine their 1–2★ reviews for the wedge
- Low traction + high rating = **hidden gems** — watch and learn from their listings
- Low traction + low rating = **noise** — ignore

For a tracked competitor, `sonar_review_insights` returns Sonar's AI review themes (praise, complaints, trends) without re-reading raw reviews.

## Output format

### Competitive landscape report

**Your app:** name — est. revenue/mo (with confidence), rating, review count

**Competitor matrix:**

| App | Est. revenue/mo | Confidence | Rating | Reviews | Price model |
|-|-|-|-|-|-|

**Keyword gaps:**

| Keyword | Popularity | Difficulty | Beatable | Who ranks | You rank |
|-|-|-|-|-|-|

**Their weaknesses (from reviews):** top recurring complaints per vulnerable competitor.

**Actions:** immediate (this week), short-term (this month), strategic (this quarter). If the user tracks apps in Sonar, point out that competitor rank moves and metadata changes can arrive as alerts (`rank-tracking`) instead of manual re-analysis.

## Related skills

- `keyword-research` — score and prioritize the gap keywords found here
- `review-analysis` — deep-dive a specific competitor's review themes
- `revenue-analysis` — precise revenue benchmarking of the niche
- `rank-tracking` — track competitors continuously with alerts
