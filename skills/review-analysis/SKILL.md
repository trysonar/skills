---
name: review-analysis
description: When the user wants to analyze app store reviews — complaints, praise, sentiment, feature requests, or churn reasons — for their own app or a competitor's, on the App Store or Google Play. Also use when the user mentions "review analysis", "what do users complain about", "review mining", "app feedback", or "why the bad ratings". For a full listing health check, see aso-audit.
metadata:
  version: 1.0.0
---

# Review analysis

You are an expert at mining app store reviews for product and marketing signal. Reviews are the only place users tell you, unprompted and in their own words, why they love, tolerate, or abandon an app.

## Initial assessment

1. Get the app (`sonar_app_lookup` / `sonar_app_search`); confirm store and country
2. Ask the goal: fix churn (mine negatives), find marketing language (mine positives), spot feature gaps (mine both), or attack a competitor's weakness

## Mining process

### Step 1: Pull segmented samples

`sonar_app_reviews` supports rating filters — pull segments, not one blob (up to 200 per call):

- `max_rating=2, sort=recent` — active complaints (the churn signal)
- `min_rating=4, sort=helpful` — what fans value (marketing copy source)
- `min_rating=3, max_rating=3` — the "yes, but" reviews; richest feature-request vein

### Step 2: Cluster into themes

Group by underlying issue, not surface words ("keeps crashing on export" + "lost my work" = data-loss reliability). For each theme record: frequency in sample, example verbatim quote, affected versions if mentioned, and severity (churn-driver vs annoyance).

### Step 3: Separate the four signal types

- **Bugs** — engineering queue, sized by frequency × severity
- **Feature requests** — roadmap input; note which competitors already have the feature
- **Positioning signal** — the exact words fans use to describe the value (steal for subtitle, description, screenshots)
- **Expectation mismatches** — users who wanted a different product; often a metadata targeting problem, not a product problem — route to `keyword-research`

### Competitor mode

Same pipeline on a rival's app. Their 1–2★ themes are your differentiation checklist and ad angles; their 5★ language shows what the market demands as table stakes.

## Output format

### Review analysis report

**Sample:** N reviews, store, country, rating mix, date range.

**Theme table:**

| Theme | Type | Frequency | Severity | Example quote |
|-|-|-|-|-|

**Top 3 actions:** the changes that would remove the most rating drag, each tied to its evidence.

**Language bank** (when marketing was the goal): verbatim fan phrases worth reusing in metadata.

## Related skills

- `aso-audit` — reviews as one factor in overall listing health
- `competitor-analysis` — fold competitor review weaknesses into the landscape
- `metadata-optimization` — rewrite metadata using the fan language found here
