---
name: app-marketing-context
description: When the user wants to set up, save, or update reusable marketing context for their app — the app's identity, audience, competitors, keywords, and goals — so every other Sonar skill starts informed instead of asking the same questions again. Also use when the user mentions "set up my app context", "remember my app", "marketing context", or at the start of a long ASO engagement. All other skills check for this file first.
metadata:
  version: 1.1.0
---

# App marketing context

You are setting up the shared context document that every other skill in this repository reads first. Ten minutes here removes the repeated twenty questions from every future session.

## Process

### Step 1: Gather

Ask for the app (store id or name), then pull live data instead of interrogating the user:

- `sonar_app_lookup` — canonical metadata, rating, category, price
- `sonar_app_aso_score` — current listing health baseline
- `sonar_app_extract_keywords` — what the listing currently targets
- `sonar_app_search` on the core keywords — the actual SERP competitors
- If the app is tracked in Sonar: `sonar_list_products` / `sonar_list_apps` for the workspace ids, and `sonar_app_overview` for the current visibility and top-10 counts

Then ask only what data can't tell you: target audience, business model and goals, markets that matter, known rivals the SERP missed, and constraints (brand rules, localization budget).

### Step 2: Write `app-marketing-context.md`

Save to the project root (or where the user keeps agent context). Structure:

```markdown
# App marketing context — [App name]

Updated: [date]

## App
- Store ids: ios [id] / android [id]
- Sonar workspace (if tracked): product_id [uuid], app ids ios [uuid] / android [uuid]
- Category, price model, current rating and review count
- One-line positioning

## Audience
- Who, what job they hire the app for, where they search from (countries)

## Competitors
- [name] — [store id] — why they matter

## Keyword strategy
- Primaries: ...   Secondaries: ...   Aspirationals: ...
- Countries tracked: ...

## Baselines ([date])
- ASO score: N/100 — weak checks: ...
- Est. revenue/mo (Sonar estimate, with its confidence grade); Android installs or iOS rating count
- Visibility and top-10 keyword count (if tracked in Sonar)

## Goals and constraints
- 90-day goal, brand rules, no-go areas
```

### Step 3: Maintain

Update after every significant action (metadata ship, new market, new competitor) and refresh baselines monthly. Stale context is worse than none — date every baseline.

## How other skills use this

Every skill in this repo checks for `app-marketing-context.md` in its first step and skips the questions it answers. When a skill produces a durable decision (a keyword strategy, a chosen variant), append it here.

## Related skills

- `keyword-research` — fills the keyword strategy section
- `aso-audit` — fills the baseline section
- `rank-tracking` — makes the baselines update themselves in Sonar
