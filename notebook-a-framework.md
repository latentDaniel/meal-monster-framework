# Notebook A — Framework & Constraints (Template)

**Purpose:** This is the rules engine for a high-protein, calorie-controlled meal planning system. Every meal plan, grocery list, and prep schedule produced downstream must respect the constraints, macros, exclusions, and phase logic defined here. The companion **Notebook B (Techniques & Ingredients)** supplies the building blocks; this notebook tells the generator how to assemble them.

**Update cadence:** Rarely. Update only when the active phase changes, daily macros are recalibrated, new exclusions are added, or the supplement/beverage stack is modified.

> **How to use this template:** Fork the repo, replace every `<PLACEHOLDER>` and example value below with your own. Upload the populated document as a source into a NotebookLM notebook titled "Meal Plan Requirements" (or your chosen name). The generator prompt in `prompts/generator-prompt.md` will then call into it.

---

## Section 1: User Profile

| Field | Value |
|---|---|
| Current weight | `<CURRENT_WEIGHT>` |
| Goal weight | `<GOAL_WEIGHT>` |
| Total target change | `<DELTA>` |
| Body composition goal | `<e.g., low body-fat, recomp, lean bulk>` |
| Training summary | `<resistance / cardio frequency>` |

---

## Section 2: Daily Macro Target

**Total calories:** `<DAILY_KCAL>` / day

| Macro | Grams | Calories | % of Total |
|---|---|---|---|
| Protein | `<PROTEIN_G>` | | |
| Carbohydrates | `<CARB_G>` | | |
| Fat | `<FAT_G>` | | |

**Protein anchor rule.** Anchor protein grams to **goal body weight**, not current weight, if the goal is fat loss from a high starting point. (1 g per current pound at very high body weights blows out the calorie budget.) Reconsider this rule for recomp or bulk phases.

**Fat floor rule.** Fat must not drop below `<FAT_FLOOR>` g/day to support endocrine function and fat-soluble vitamin absorption. Do not trade fat down to add more protein or carbs.

**Carb placement rule.** Carbs are **front-loaded around training**. The post-workout meal receives the largest carb portion of the day. On non-training days carb totals stay the same but are distributed more evenly across meals.

---

## Section 3: Meal Architecture

| Rule | Value |
|---|---|
| Meals per day (range) | `<3–4 typical>` |
| Default | `<DEFAULT_MEALS>` |
| Protein per meal (4-meal day) | `<PROTEIN_PER_MEAL_4>` |
| Protein per meal (3-meal day) | `<PROTEIN_PER_MEAL_3>` |
| Calories per meal (4-meal day) | `<KCAL_PER_MEAL_4>` |
| Calories per meal (3-meal day) | `<KCAL_PER_MEAL_3>` |
| Post-workout meal | Largest meal of the day; biggest carb portion; fastest-digesting protein |

**MPS rule (Muscle Protein Synthesis).** Each meal must contain **≥3 g leucine** to maximally stimulate MPS — typically achieved with **40 g+ of high-quality animal protein** (chicken, beef, fish, eggs, dairy) or a serving of whey isolate. Plant proteins generally fall short of the leucine threshold at equivalent gram totals and should not be a meal's sole protein source.

---

## Section 4: Food Exclusions & Preferences

**Hard exclusions (never include):** `<LIST_YOUR_HARD_EXCLUSIONS>`

**Avoid (only use if no viable alternative exists):** `<LIST_SOFT_AVOIDS>`

**Menu direction (preferences to bias inventory picks):**
- Texture / cut preferences (e.g., whole-cut vs. ground vs. shredded)
- Cuisine rotation targets (e.g., "seafood weekly", "Mediterranean twice a week")
- Carb variety beyond rice and potato
- Flavor profile rotation

**Prioritize these flavor profiles:** `<LIST_YOUR_FLAVOR_STACKS>`

**Preferred cooking methods:** `<LIST_YOUR_METHODS>`

---

## Section 5: Supplement & Beverage Stack

| Item | Use | Macro contribution |
|---|---|---|
| `<protein supplement>` | `<servings/day, when>` | `<g protein, kcal per serving>` |
| `<caffeine source 1>` | `<when>` | `<kcal>` |
| `<caffeine source 2>` | `<when>` | `<kcal>` |
| `<pre-workout, if any>` | Pre-workout only | `<kcal>` |
| `<fiber / other>` | `<when>` | `<kcal>` |

Document the composition of any blend supplement (e.g., whey-casein-egg ratios), and note whether single-source whey isolate is preferred for the post-workout window.

---

## Section 6: Phase Logic

Define your phases here. A common pattern:

### Phase 0: Ramp-In (Weeks −N to 0)
A 2–3 week glide path into the Phase 1 deficit. Purpose: blunt the initial adherence and energy shock of dropping from maintenance to the target deficit in one step.

| Sub-phase | Calories | Protein | Carbs | Fat | Notes |
|---|---|---|---|---|---|
| Week −2 | `<+25% above Phase 1 kcal>` | `<anchor>` | | | |
| Week −1 | `<+25%>` | | | | |
| Week 0  | `<+12%>` | | | | |
| Phase 1 start | `<target>` | | | | |

**Trigger to advance** between sub-steps: hunger manageable AND sleep stable AND no training-performance dip for **3+ consecutive days**.

### Phase 1: Aggressive Cut (Weeks 1–N)
- Calories: target deficit
- Macros: per Section 2
- Expected loss: define rate
- Meal generation focus: maximum protein, maximum volume veg, strategic carbs around training

### Diet Break (Weeks N+1 to N+3)
- Calories: new maintenance after Phase 1 loss
- Macros: protein stays anchored; add carbs and modest fat to reach the new calorie target
- Food quality stays clean — same exclusions, same staples, just more volume
- Purpose: hormonal reset (leptin, thyroid T3), psychological relief, training recovery

### Phase 2+: Repeat Cycle
- Re-enter Phase 1 deficit at the new lower body weight
- Recalculate diet-break maintenance figure based on the new weight
- Continue alternating Phase 1 / Diet Break until goal weight is reached

### Phase transition triggers
Phase length may shorten if:
- Rate of loss stalls for **2+ consecutive weeks**
- Sleep quality drops significantly
- Training performance crashes (strength loss, persistent fatigue, missed sessions)

---

## Section 7: Equipment Inventory

The architect must constrain bulk-cook techniques to equipment actually owned. Do not propose techniques requiring equipment marked unavailable.

| Equipment | Status | Notes |
|---|---|---|
| `<e.g., Pellet smoker>` | ✅ / 🛒 / ❌ | |
| `<Grill>` | | |
| `<Oven + stove>` | | |
| `<Instant Pot / pressure cooker>` | | |
| `<Rice cooker>` | | |
| `<Sous-vide circulator>` | | |
| `<Air fryer>` | | |

Add fallback rules for any equipment marked incoming or unavailable (e.g., "sous-vide chicken → smoker 250 °F to 160 °F internal, ~1 hr").

---

## Section 8: Training Context

| Modality | Frequency / Detail |
|---|---|
| Resistance training | `<sessions/week>` |
| Cardio | `<frequency, modality>` |
| Joint-friendly cardio modalities | List those that fit your body weight and joint history |

**Meal timing relative to training:**
- **Pre-workout:** small protein/carb feeding if training falls more than 2 hours after the last meal
- **Post-workout:** largest meal of the day, fastest carbs, fastest-digesting protein

---

## Section 9: Caffeine Ceiling

- **Daily limit:** **400 mg** (FDA guidance)
- Document the mg contribution of every caffeine source in your stack (cold brew, energy drink, pre-workout)

**Generator rule.** When proposing daily beverage layouts, **flag any day projected to exceed 400 mg total caffeine** and suggest electrolyte alternatives (LMNT, DIY sodium / potassium / magnesium mix) in place of the most flexible source.

---

## Section 10: Open Calibration Items

List items that are **not yet locked**. The generator should treat them as in-flux and not encode them as hard rules. Examples:
- Supplement under review
- Diet-break duration default
- LBM measurement method (DEXA / BIA / calipers)
- Refeed vs. diet break preference

---

## Section 11: Generator Instructions

The downstream generator (Gemini, Claude Code, or any other agent producing meal plans, grocery lists, or prep schedules) **must** follow these rules:

1. **Confirm current phase** (Section 6) before proposing macros for any plan.
2. **Respect the calorie and macro envelope** (Section 2) exactly — do not round up totals.
3. **Honor exclusions** (Section 4) — never include any item on the hard-exclusion list.
4. **Stay within meal architecture** (Section 3) — meals per day in range; hit the per-meal protein floor and the ≥3 g leucine requirement.
5. **Pull ingredients and techniques from the companion ingredients/techniques notebook** — do not invent recipes outside the cataloged inventory.
6. **Flag caffeine totals** (Section 9) on any day that exceeds the ceiling and propose electrolyte substitutions.
7. **Front-load carbs** (Section 2) around training; the post-workout meal gets the largest carb portion.
8. **Output structure:**
   - **Meal plan** as a table with columns: meal / macros / ingredients / prep technique
   - **Grocery list** grouped by store section
   - **Prep schedule** as a day-by-day timeline
