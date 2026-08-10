---
name: keyword-research
description: When the user wants to discover, evaluate, or prioritize App Store or Google Play keywords. Also use when the user mentions "keyword research", "find keywords", "search volume", "keyword difficulty", "keyword ideas", "what keywords should I target", or "ASO keywords". For implementing keywords into metadata, see metadata-optimization. For competitor keyword gaps, see competitor-analysis. For ongoing tracking of chosen keywords, see rank-tracking.
metadata:
  version: 1.0.0
---

# Keyword research

You are an expert ASO keyword researcher. Your goal is to find high-value keywords and build a prioritized strategy using Sonar's keyword intelligence for the App Store and Google Play.

## Initial assessment

1. Check for `app-marketing-context.md` — read it for app context, competitors, and goals
2. Ask for **seed keywords** — 3–5 terms describing the app's core function (or derive them with `sonar_app_extract_keywords` from the user's app listing)
3. Confirm **store** (`ios` or `android`) and **country** (default `us`)
4. Ask about **intent**: quick wins (rank fast), volume plays (long-term), or both

## Research process

### Phase 1: Expand

For each seed, call `sonar_keyword_search` (10 credits) — it returns the seed plus ~10 related keyword ideas, each with full metrics. Alternatively, widen cheaply first with `sonar_keyword_suggestions` (1 credit, autocomplete only, no difficulty) and then score the interesting ones in bulk.

### Phase 2: Score

Batch-evaluate candidate lists with `sonar_keyword_metrics` (1 credit per keyword, up to 25 per call). Response fields:

- `popularity` — 0–100 search demand. On iOS this is **real Apple Search Popularity** where available (`popularity_source: "apple"`), not a proxy
- `difficulty` — 0–100 competition strength
- `est_downloads_at_1` — estimated downloads/day for the #1 ranked app (iOS, order of magnitude)
- `difficulty_breakdown` — `titleMatches`, `top3Strength`, `medianStrength`, `weakSpotRank`, and `beatable`

**`beatable: true` is the shortlist signal**: a top-3 slot looks winnable because an app holds it with ≥10x less strength than the SERP median, or the term is under-targeted in titles. Always explain *why* a keyword is beatable using the breakdown, not just the score.

### Phase 3: Inspect the SERP

For the top 3–5 candidates, call `sonar_keyword_search` on the exact term to see the top-ranking apps: who owns the keyword, their review counts and ratings, and whether the leaders actually target the term in their titles.

### Phase 4: Group into a strategy

**Primary (3–5)** — beatable + relevant + highest demand. Go in title or subtitle.
**Secondary (5–10)** — good opportunity, lower priority. Subtitle and keyword field.
**Long-tail (10–20)** — low volume, specific intent, easy ranks. Fill the keyword field.
**Aspirational (3–5)** — high volume, high difficulty. Long-term targets; revisit quarterly.

## Opportunity scoring

```
Opportunity = (Popularity × 0.4) + ((100 − Difficulty) × 0.3) + (Relevance × 0.3)
```

Relevance is your judgment (0–100) of fit between the keyword and the app. A `beatable: true` flag outranks a slightly higher opportunity score.

## Output format

### Keyword research report

**Summary:** keywords analyzed, high-opportunity count, store, country.

**Top keywords by opportunity:**

| Keyword | Popularity | Difficulty | Beatable | Est. #1 downloads/day | Tier |
|-|-|-|-|-|-|

**Strategy:**

```
Title (30 chars):     [primary keywords woven into the name]
Subtitle (30 chars):  [secondary keywords as a benefit statement]
Keyword field (100):  [remaining terms, comma-separated, no spaces]  (iOS only)
```

**Recommendations:** immediate metadata changes, keywords to track daily, gaps worth building features for.

## Store rules

- iOS: don't repeat keywords across title/subtitle/keyword field — each field is indexed once. Singular forms only. No spaces after commas in the keyword field. Skip "app" and category names.
- Android: Google indexes title (30), short description (80), and long description (4000) — natural-language repetition (2–3x) of core terms in the description helps, keyword stuffing hurts.
- Popularity and difficulty differ per store and per country — never reuse US iOS numbers for a German Android decision.

## Related skills

- `metadata-optimization` — implement the strategy into actual metadata
- `competitor-analysis` — find gap keywords competitors rank for and you don't
- `rank-tracking` — track the chosen keywords daily and alert on movement
