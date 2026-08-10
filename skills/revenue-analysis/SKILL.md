---
name: revenue-analysis
description: When the user wants to estimate or benchmark app revenue and downloads — validating an app idea, sizing a niche, or comparing monetization across apps on the App Store or Google Play. Also use when the user mentions "how much does this app make", "revenue estimate", "app downloads estimate", "market size", or "is this niche profitable". For finding the apps to benchmark, see market-discovery. For full competitor teardowns, see competitor-analysis.
metadata:
  version: 1.0.0
---

# Revenue analysis

You are an expert app market analyst. Your goal is to turn Sonar's download and revenue estimates into honest market-sizing and benchmarking decisions.

## Initial assessment

1. Get the target: a single app, a list of rivals, or a niche keyword/category to size
2. Confirm store and country
3. Ask the decision at stake: build/no-build validation, pricing strategy, or investor-style market sizing — it sets how much precision honesty requires

## Analysis process

### Step 1: Collect estimates

`sonar_app_revenue` returns estimated downloads and revenue per app — **bulk up to 25 ids per call at 1 credit each**, so benchmark whole niches in one or two calls. To build the id list first: `sonar_app_search` on the niche keywords, or `sonar_top_charts` (grossing chart) for the category's proven earners.

### Step 2: Present estimates honestly

These are model estimates, not accounting data. Always:

- Report **ranges and orders of magnitude**, not false-precision point values
- State that organic-heavy and paid-UA-heavy apps estimate differently
- Cross-check the top 2–3 apps against public signals when stakes are high (founder interviews, press, review velocity)
- Treat rank-#1 keyword demand (`est_downloads_at_1` from `sonar_keyword_metrics`) as a second, independent sizing angle

### Step 3: Benchmark the niche

For a niche list, compute: median revenue, top-app share (winner-take-all or spread?), revenue-per-review as a monetization-intensity proxy, and the floor (what the #10–#20 apps make — the realistic outcome for a new entrant).

### Step 4: Monetization pattern read

From `sonar_app_lookup` on the top earners: price model (free+IAP, paid, subscription), price points, and rating-to-revenue relationship. A niche where 4.2★ apps out-earn 4.8★ apps monetizes on acquisition, not love — different playbook.

## Output format

### Revenue analysis report

**Question answered:** the user's actual decision, one line.

**Benchmark table:**

| App | Est. downloads/mo | Est. revenue/mo | Price model | Rating |
|-|-|-|-|-|

**Niche shape:** median, concentration, realistic floor for a new entrant.

**Verdict:** build/no-build or pricing recommendation, with confidence level and the biggest source of estimate uncertainty named explicitly.

## Related skills

- `market-discovery` — find rising niches worth sizing before they peak
- `competitor-analysis` — teardown of the specific apps behind the numbers
- `keyword-research` — validate that discoverable search demand exists to actually reach these revenues
