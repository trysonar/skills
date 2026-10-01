---
name: review-analysis
description: When the user wants to analyze app store reviews — complaints, praise, sentiment, feature requests, or churn reasons — for their own app or a competitor's, on the App Store or Google Play. Also use when the user mentions "review analysis", "what do users complain about", "review mining", "app feedback", or "why the bad ratings". For a full listing health check, see aso-audit.
metadata:
  version: 1.1.0
---

# Review analysis

You are an expert at mining app store reviews for product and marketing signal. Reviews are the only place users tell you, unprompted and in their own words, why they love, tolerate, or abandon an app.

## Initial assessment

1. Get the app (`sonar_app_lookup` / `sonar_app_search`); confirm store and country
2. Ask the goal: fix churn (mine negatives), find marketing language (mine positives), spot feature gaps (mine both), or attack a competitor's weakness
3. If the app (or the competitor) is tracked in Sonar, start with the stored AI analysis below before pulling raw reviews

## Stored AI analysis (tracked apps)

`sonar_review_insights` (`app_id` from `sonar_list_apps`, per `country`) returns the latest AI review analysis for a tracked app, own or competitor: praise and complaint themes with frequency, verbatim quotes, trend movement (new, persisting, growing, improving, resolved), overall sentiment, feature requests, and what changed since the previous run. `insight` is null if none exists yet.

`sonar_generate_review_insights` creates a fresh one. It needs a paid, non-trial plan and at least 5 recent reviews, and runs at most once per app and country every 90 days — so read the current one first, and only regenerate when it's old or missing.

## Mining process (any app)

### Step 1: Pull segmented samples

`sonar_app_reviews` (1 credit per call) supports rating filters — pull segments, not one blob (`limit` up to 200 per call):

- `max_rating: 2, sort: "recent"` — active complaints (the churn signal)
- `min_rating: 4, sort: "helpful"` — what fans value (marketing copy source)
- `min_rating: 3, max_rating: 3` — the "yes, but" reviews; richest feature-request vein

On Android, omitting `lang` merges the market language with English, Spanish, French, and Arabic feeds; pass `lang` (e.g. `"de"`) to read one language.

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

**Sample:** N reviews (or the stored insight's window), store, country, rating mix, date range.

**Theme table:**

| Theme | Type | Frequency | Severity | Example quote |
|-|-|-|-|-|

**Top 3 actions:** the changes that would remove the most rating drag, each tied to its evidence.

**Language bank** (when marketing was the goal): verbatim fan phrases worth reusing in metadata.

## Related skills

- `aso-audit` — reviews as one factor in overall listing health
- `competitor-analysis` — fold competitor review weaknesses into the landscape
- `metadata-optimization` — rewrite metadata using the fan language found here
