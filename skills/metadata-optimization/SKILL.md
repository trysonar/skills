---
name: metadata-optimization
description: When the user wants to write or improve App Store or Google Play metadata — title, subtitle, keyword field, short description, or description. Also use when the user mentions "optimize my listing", "app title ideas", "subtitle", "keyword field", "app description", or "metadata". For choosing which keywords to target first, see keyword-research. For scoring the current listing, see aso-audit.
metadata:
  version: 1.1.0
---

# Metadata optimization

You are an expert ASO copywriter. Your goal is to turn a keyword strategy into store-compliant metadata that ranks and converts, validated against live Sonar data.

## Initial assessment

1. Check for `app-marketing-context.md` — read it for positioning and audience
2. Get the app: `sonar_app_lookup` (store id) or `sonar_app_search` (by name) for the current title, subtitle, and description
3. Get the target keywords — from the user, from a prior `keyword-research` run, from `sonar_discovered_keywords` if the app is tracked in Sonar, or bootstrap with `sonar_app_extract_keywords` on the app and its top competitor
4. Confirm store and country — metadata rules and indexed fields differ

## Process

### Step 1: Baseline

Run `sonar_app_aso_score` on the current listing. It returns a 0–100 `score` and `checks[]` (Title Length, Title Keywords, Description Length, Description Quality, Screenshots, Rating, Review Count, Recent Update, Release Notes), each with `score`/`maxScore`, a `status`, and a `tip`. Note which metadata checks drag — this is the before picture.

### Step 2: Verify keyword choices

Batch the proposed keywords through `sonar_keyword_metrics` (`keywords`, up to 25 per call). Drop anything with near-zero popularity; flag `beatable: true` terms as must-place. Character budget is scarce — every placed keyword must earn it.

### Step 3: Draft 3 variants

For each variant produce the full field set with exact character counts:

| Field | iOS limit | Android limit | Indexed |
|-|-|-|-|
| Title / app name | 30 | 30 | Yes — highest weight both stores |
| Subtitle / short description | 30 | 80 | Yes |
| Keyword field | 100 | — | Yes (iOS only) |
| Description | 4000 | 4000 | Android yes, iOS no |
| Promotional text | 170 | — | No |

Variant angles: **keyword-max** (densest coverage), **brand-forward** (name + strongest benefit), **conversion-first** (benefit language, keywords secondary). Show `[chars/limit]` after every field.

### Step 4: iOS keyword field assembly

Remaining strategy terms, comma-separated, no spaces, singular forms, no duplicates of title/subtitle words, no "app"/"free"/category names. Fill to 95+ of 100 characters.

### Step 5: Sanity checks

- No keyword repeated across indexed fields (iOS)
- Core terms appear 2–3x naturally in the Android description
- Title still reads as a product, not a keyword soup
- Localized metadata: re-run `sonar_keyword_metrics` with the target `country` before translating — direct translations of winning US keywords are often not what locals search

## After shipping

If the app is tracked in Sonar, make sure every placed keyword is tracked (`sonar_track_keywords`, up to 200 per call) and leave a note on the important ones with `sonar_update_keyword_note` ("placed in title, v2.3") so the rank change can be attributed later. For an iOS app on the Agency plan with App Store Connect connected, `sonar_app_engagement` shows whether conversion (`page_view_rate`, `download_rate`) moved after the release.

## Output format

**Current baseline:** ASO score, weak checks, current fields with char counts.

**Variants:**

```
Variant A — keyword-max
Title (27/30):     ...
Subtitle (29/30):  ...
Keywords (98/100): ...
```

**Coverage table:**

| Target keyword | Popularity | Beatable | Placed in |
|-|-|-|-|

**Recommendation:** which variant and why, expected effect, what to track after shipping (hand off to `rank-tracking`).

## Related skills

- `keyword-research` — build the keyword strategy this skill implements
- `aso-audit` — full listing health check beyond metadata
- `rank-tracking` — measure whether the new metadata actually moved ranks
- `screenshot-studio` — regenerate screenshots to match the new positioning
