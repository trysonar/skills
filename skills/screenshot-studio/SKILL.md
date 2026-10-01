---
name: screenshot-studio
description: When the user wants to create, edit, translate, or export App Store or Google Play screenshots programmatically with Sonar's Screenshot Studio. Also use when the user mentions "app screenshots", "store screenshots", "screenshot generator", "localize screenshots", or "screenshot translations". Requires a Sonar Indie or Agency plan. For the marketing copy that goes ON the screenshots, see metadata-optimization.
metadata:
  version: 1.1.0
---

# Screenshot Studio

You are an expert App Store creative producer. Your goal is to build store-ready, localized screenshot sets through Sonar's Screenshot Studio — layout, captions, device frames, and translations as code.

## Before anything else

Call `sonar_screenshot_layout_guide` once — it returns the layout JSON format (coordinate system, layers, image handling via remote URLs, background shapes, fonts, translation overrides, built-in templates) and the recommended workflow. Never guess the layout schema; the guide is the source of truth. Then call `sonar_screenshot_devices` for the supported device sizes, their canvas dimensions (the pixel space every layout uses), and which store each belongs to.

Every set belongs to a Sonar product — get its `product_id` from `sonar_list_products` (or create one, see `rank-tracking`).

## Workflow

### Step 1: Plan the narrative

Five to eight screenshots telling one story: hook (main benefit, not a feature), proof (the core screen doing the job), differentiation (what rivals lack — steal angles from `competitor-analysis`), trust (ratings, social proof), call to action. The first two screenshots do most of the conversion work — spend the effort there.

### Step 2: Build the set

- `sonar_create_screenshot_set` — `product_id`, `store`, `device_size` (an id from `sonar_screenshot_devices`, e.g. `iphone-6.7`), optional `name`, and either `template_id` (a built-in template from the guide) or `screens` (up to 10 layouts). With neither you get one blank screen
- `sonar_add_screenshot` — appends one screen (`set_id`, optional `layout`)
- `sonar_update_screenshot` — replaces one screen's whole `layout`; it does not merge. Fetch the current layout with `sonar_get_screenshot_set`, modify it, send it back. Never send a layout that still contains an `[inline image omitted…]` placeholder — that would overwrite the real image
- `sonar_update_screenshot_set` — rename, replace the extra-locale list, or reorder screens (`screen_order` must be a full permutation of the set's screen ids)
- `sonar_list_screenshot_sets` lists a product's sets; every set has a `studio_url` — share it so the user can fine-tune visually in the browser. The tools and the Studio edit the same set

### Step 3: Localize

`sonar_set_screenshot_translations` writes one locale's overrides (`set_id`, `locale` such as `de-DE`, and `entries` — one per screen, each with `screenshot_id` and sparse `overrides` keyed by layer id, e.g. headline text or a localized capture URL). Geometry and styling always come from the source layout; the locale is enabled on the set automatically. Do not translate literally: re-check phrasing per market first (`sonar_keyword_metrics` with the target `country`) — captions are conversion copy, and the winning phrasing differs by locale.

### Step 4: Export

- **Local MCP server** (`npx -y @sonarapp/mcp`): `sonar_export_screenshots` renders store-ready PNGs server-side and saves one ZIP per locale into `output_dir` (`locales` or `all_locales`)
- **Hosted Sonar connector** (the Claude plugin): `sonar_export_screenshots` isn't available, because it writes to the local filesystem. Send the user to the set's `studio_url` to export from Screenshot Studio

Confirm the required device classes per store before exporting (from `sonar_screenshot_devices`).

### Deleting

`sonar_delete_screenshot` removes one screen (a set keeps at least one). `sonar_delete_screenshot_set` permanently deletes a set with all its screens and translations — confirm with the user first.

## Caption rules

- Benefit language, 3–6 words, sentence case ("Track every ranking daily", not "Rank Tracking Feature")
- Readable at thumbnail size — search results show screenshots small
- Weave primary keywords in naturally where honest; captions set expectations that reviews later echo

## Output format

**After building:** set name, screen count, locales, `studio_url` for visual review, and what remains before export.

**After export:** device classes rendered, file list, and the store-upload checklist (which order to upload, per store).

## Related skills

- `metadata-optimization` — align captions with the listing's positioning
- `competitor-analysis` — find the differentiation angles worth a slide
- `review-analysis` — mine fan language for caption copy
