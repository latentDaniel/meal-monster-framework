# Generator Prompt — Weekly Meal Plan Builder

**How to use this file:** Paste this prompt into your AI of choice with access to both notebooks (either attached as sources or reachable via an MCP / API connection). Fill in the variables at the top. The output is a complete week of meal plans, grocery lists, and prep schedule.

---

## Variables (fill in before running)

```
WEEK_OF: <date, e.g. "YYYY-MM-DD">
CURRENT_PHASE: <Phase 1 Cut / Diet Break / Phase 2 Cut / etc.>
CURRENT_WEIGHT: <your current weight in lbs>
WEEKS_INTO_PHASE: <e.g. 3 of 12>
TRAINING_DAYS: <list, e.g. Mon/Tue/Thu/Fri/Sat>
REST_DAYS: <list, e.g. Wed/Sun>
PREP_DAY: <e.g. Sunday>
MEALS_PER_DAY: <3 or 4>
SPECIAL_NOTES: <anything one-off, e.g. travel days, social meals>

```

---

## Generator Prompt

You have access to two notebooks:

- **Notebook A — Framework & Constraints:** my macro targets, calorie ceiling, food exclusions, supplement stack, phase logic, meal architecture rules
- **Notebook B — Techniques & Ingredients:** my inventory of proteins, carbs, vegetables, flavor stacks, bulk cooking techniques, and meal templates

Build a complete meal plan for the week of {WEEK_OF}. Follow this process exactly:

### Step 1: Confirm constraints

Before generating anything, pull from Notebook A:
- Current phase macros and calorie target
- Food exclusions
- Meal architecture (3 or 4 meals, protein per meal)
- Caffeine ceiling
- Supplement stack defaults

Briefly state the confirmed constraints back to me before proceeding.

### Step 2: Pull from inventory

From Notebook B, pull:
- Top 6–8 protein options ranked by g protein per 100 cal
- Top 4–5 carb options sorted by training-day fit
- Top 5–6 volume veg options
- Top 4–5 flavor stacks that pair with the selected proteins
- 2–3 bulk cooking techniques that can produce the week's protein in one prep session

State what you selected and why.

### Step 3: Build the weekly grid

Generate a 7-day meal plan as a table. Each row is a day; columns are Meal 1, Meal 2, Meal 3, Meal 4 (omit Meal 4 column if MEALS_PER_DAY=3).

Each meal cell must include:
- Meal name
- Protein source + grams
- Carb source + grams
- Veg + estimated grams
- Flavor stack
- Total macros (P/C/F/cal)
- Prep technique reference

**Rules:**
- Daily totals must hit macro target ±5g per macro
- Training-day meals: post-workout meal gets the largest carb portion
- Rest-day meals: distribute carbs evenly or reduce slightly
- No meal contains any item on Notebook A Section 4's hard-exclusion list
- Variety: no single protein source repeats more than 4 days of 7
- Flavor stack varies day-to-day to prevent palate fatigue

### Step 4: Daily totals check

After the grid, output a daily totals table:

| Day | Protein (g) | Carbs (g) | Fat (g) | Calories | Caffeine (mg) | Notes |
|---|---|---|---|---|---|---|

Flag any day where caffeine exceeds the ceiling from Notebook A Section 9, or where macros drift more than 5g from target.

### Step 5: Grocery list

Generate a consolidated grocery list grouped by store section:
- Proteins (with total weight needed for the week)
- Carbs / starches
- Vegetables / produce
- Flavor stack ingredients
- Supplements / beverages
- Pantry items

Include estimated quantities for the full week.

### Step 6: Prep schedule

Generate a prep day timeline for {PREP_DAY}:

| Time block | Task | Equipment | Output |
|---|---|---|---|

Include:
- Bulk protein cooking (which techniques from Notebook B)
- Carb prep
- Veg prep (washed, chopped, portioned)
- Flavor stack batch prep
- Portioning and storage

Total prep time estimate.

### Step 7: Substitutions section

Provide 2–3 substitution options per protein in case of grocery availability issues, all macro-equivalent.

### Step 8: Open questions

If anything in Notebook A or B was ambiguous or insufficient to build the plan, list it at the end so I can update the notebooks.

---

## Output formatting

- Use tables for meal grid, daily totals, grocery list, prep schedule
- Keep prose minimal — this is a reference document I'll execute against
- All macros to the nearest gram
- All calories to the nearest 5
