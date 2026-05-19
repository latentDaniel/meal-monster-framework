# Notebook B — Techniques & Ingredients (Ingestion Prompt)

**How to use this file:** Paste this prompt into NotebookLM when adding new sources to Notebook B. Sources can be anything you trust — articles, recipe pages, cookbook excerpts, video transcripts, your own meal logs. This prompt tells NotebookLM how to extract content into a consistent, queryable structure so the downstream generator (Gemini or Claude Code) can pull building blocks reliably.

You can re-run this prompt periodically against all sources to rebuild the inventory as the source set grows.

---

## Ingestion Prompt (paste into NotebookLM)

You are building a structured ingredients and techniques library for high-protein, calorie-controlled meal planning. From the sources I have provided, extract content into the following six inventories. Use the exact section headers and table structures below. If a source contains content for multiple inventories, populate each.

**Hard constraints to respect when extracting** (fill in from Notebook A before pasting):
- Exclude any recipe or technique that uses an item on Notebook A Section 4's hard-exclusion list: `<list>`
- Deprioritize anything on the soft-avoid list: `<list>`
- Prioritize content suitable for the daily macro target in Notebook A Section 2 (`<DAILY_KCAL>` kcal, `<PROTEIN_G>` g protein across `<MEALS_PER_DAY>` meals)

---

### Inventory 1: Proteins

For each protein source mentioned in the sources, extract:

| Protein | Cut/Form | Cal per 100g | Protein per 100g | g protein per 100 cal | Prep methods | Freezer life | Source |
|---|---|---|---|---|---|---|---|
| (e.g.) Chicken breast | Boneless skinless | 165 | 31 | 18.8 | Grill, smoke, sous vide, air fry | 6 months cooked | [source title] |

Rank within inventory by g protein per 100 cal (highest first).

### Inventory 2: Carbohydrates

| Carb source | Form | Cal per 100g cooked | Carbs per 100g | Glycemic load | Satiety rating (low/med/high) | Pre or post workout fit | Source |
|---|---|---|---|---|---|---|---|
| (e.g.) White rice | Cooked, long grain | 130 | 28 | Medium | Medium | Post-workout | [source title] |

### Inventory 3: Volume Vegetables

Vegetables that maximize satiety per calorie. Exclude anything on Notebook A's hard-exclusion list.

| Vegetable | Cal per 100g | Fiber per 100g | Prep methods | Pairs well with | Source |
|---|---|---|---|---|---|

### Inventory 4: Flavor Stacks

Sauces, rubs, marinades, and finishes. Each entry is a discrete recipe or technique.

| Name | Type (rub / wet sauce / marinade / finish) | Key ingredients | Macros per serving | Pairs with | Recipe / instructions | Source |
|---|---|---|---|---|---|---|
| (e.g.) Chimichurri | Wet finish | Parsley, garlic, red wine vinegar, olive oil, oregano, red pepper flake | ~45 cal, 5g fat per 2 tbsp | Steak, chicken, bison | [extracted recipe] | [source title] |

### Inventory 5: Bulk Cooking & Prep Techniques

Standalone procedures for batch cooking, freezing, portioning. Each entry should be a complete how-to.

| Technique name | What it produces | Equipment needed | Time required | Yield (servings) | Steps | Storage | Source |
|---|---|---|---|---|---|---|---|
| (e.g.) Smoked chicken breast batch | 6 lbs cooked chicken | Smoker, meat thermometer | 3 hrs active, 4 hrs smoke | 12 servings | [extracted steps] | Fridge 5 days, freezer 6 months | [source title] |

### Inventory 6: Meal Templates

Reusable meal skeletons, not specific recipes. Each template is a slot pattern that the generator can fill from inventories 1–4.

| Template name | Slot pattern | Target macros | Best meal slot | Source |
|---|---|---|---|---|
| (e.g.) Smoked protein bowl | 1 protein (smoked) + 1 carb + 2 volume veg + 1 flavor stack | 50g P / 40g C / 15g F / ~500 cal | Lunch or dinner | [source title] |

---

## Source Metadata Section

After extracting into the six inventories, add a final section listing each source with:

| Source title | Type (video / article / book / podcast) | Creator | URL | Date ingested | Key contributions (which inventories were populated) |
|---|---|---|---|---|---|

---

## Re-ingestion Rule

When new sources are added later, append to the inventories rather than rebuilding from scratch. If a new source contradicts an existing entry (e.g., different macros for the same food), keep both and note the discrepancy in a footnote — do not silently overwrite.
