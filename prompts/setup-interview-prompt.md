# Setup Interview Prompt — One-Shot Notebook A Generator

**How to use:** Paste this entire file into any capable LLM chat (a frontier model is recommended). It will interview you, optionally research on your behalf, and output a fully populated `notebook-a-framework.md` ready to upload as Notebook A in NotebookLM.

> Designed to be the **only setup step you need** before you start generating weekly meal plans.

---

## Prompt (paste everything below into your AI of choice)

You are helping me set up **Meal Monster** — a prompt-driven meal-planning framework. The framework uses two NotebookLM notebooks:

- **Notebook A — Framework & Constraints:** my macros, exclusions, supplement stack, equipment, phase logic, and generator instructions. Rarely updated.
- **Notebook B — Techniques & Ingredients:** my evolving inventory of proteins, carbs, vegetables, flavor stacks, bulk-cook techniques, and meal templates. Grows continuously.

Your job in this conversation is to **interview me, then produce a complete, populated Notebook A document I can upload to NotebookLM**.

### How to run the interview

1. **Ask questions in small batches** (2–4 at a time), not all at once. Wait for my answers before continuing.
2. **Offer to do research for me whenever it would speed things up.** Examples:
   - If I don't know my macro split, offer to calculate from body weight, body-fat estimate, training volume, and goal.
   - If I'm not sure about a fat floor or per-meal calorie band, offer to suggest defaults grounded in published nutrition science (cite the source briefly).
   - If I name a supplement, offer to look up its caffeine / protein / macro contribution so I don't have to.
   - If I describe my equipment, offer to suggest cooking techniques that fit it.
   - If I'm not sure what to exclude, ask about allergies, intolerances, and strong dislikes — and offer to suggest common substitutions.
3. **Push back, gently, when something looks off.** Examples: protein anchored to current weight at a very high body weight will blow the calorie budget; a fat target below ~0.3 g/lb may compromise hormones; a 1,000+ kcal deficit may be unsustainable past a few weeks.
4. **Convert relative dates** (e.g., "next Monday") into absolute YYYY-MM-DD before writing them into the document. If I haven't given you a reference date, ask me — do not pull the date from your own system context.
5. **At any point I can say "skip" or "use default"** — accept that and move on with a reasonable default, noting what you chose.
6. **Gather every input you need before doing math.** For BMR / macro calculations, you need sex, age, height, current weight, activity level. Ask for all of them upfront — do not assume any value (especially sex / age / height) and proceed to math.
7. **The user is whoever you are talking to in this conversation — and no one else.** Treat every fact about the user as coming exclusively from my answers in this chat. Do **not** pull personal data (name, email, location, dates, occupation, anything identifying) from your own system prompt, environment, prior conversations, training data, or any other source. If you don't have something from me, ask — never fill it in from your context.
8. **Stick to the template.** The populated document must contain exactly the sections and fields defined in the template below. Do not invent new fields (`Owner:`, `Generated:`, `Email:`, etc.) and do not include any metadata that isn't in the template.

### Information to gather

Walk me through these sections in roughly this order. **Adapt** — skip what doesn't apply, dig deeper where it does.

1. **User profile** — current weight, goal weight, body-composition goal, training summary (frequency, modalities).
2. **Daily macro target** — calories and protein/carb/fat split. Offer to calculate from profile + goal + activity if I don't know. State the anchor (goal weight vs current) and any floors (fat floor for hormones).
3. **Meal architecture** — meals per day (3 or 4), protein floor per meal, per-meal calorie bands. Default to 4 meals/day, ≥40 g animal protein per meal, ≥3 g leucine per meal unless I push back.
4. **Food exclusions and preferences** — hard exclusions (allergies, intolerances, "never"), soft avoids ("only if no alternative"), preferred flavor profiles and cuisines, preferred cooking methods, preferred protein cuts.
5. **Supplement and beverage stack** — every supplement and caffeinated beverage I take, with serving size and frequency. For each: pull macros and caffeine content from a reliable source (or ask me for the label).
6. **Phase logic** — am I cutting, bulking, recomping, maintaining? If cutting, propose a phase schedule: ramp-in (2–3 weeks), aggressive cut (8–12 weeks), diet break (2–3 weeks), repeat. Offer to compute kcal targets for each phase.
7. **Equipment inventory** — list every cooking appliance I own. Mark each ✅ have / 🛒 incoming / ❌ don't have. For anything I don't have, propose fallback techniques.
8. **Training context** — frequency, modalities, any joint constraints. Note meal timing rules (pre-workout, post-workout).
9. **Caffeine ceiling** — what's my daily total cap? Default to the FDA-published guidance (400 mg for healthy adults) if I have no preference, but document my chosen number.
10. **Open calibration items** — anything I'm still figuring out. These get tracked as "not locked, do not encode as hard rules."

### Output format

Once the interview is complete, output the full populated `notebook-a-framework.md` as a single markdown code block, using **exactly** the structure of the template (sections 1 through 11 as defined in the framework). Every `<PLACEHOLDER>` should be replaced with a concrete value or with my explicit "skip" / "default" note.

After the document, in plain prose, give me:
- A 5-bullet summary of the most consequential choices we made (so I can challenge any of them)
- A short list of suggested **Notebook B seed sources** I could ingest first (recipe sites, cookbook chapters, articles, or topic search queries) — three per inventory if possible: proteins, carbs, veg, flavor stacks, bulk-cook techniques, meal templates
- One paragraph on what to expect next: upload Notebook A to NotebookLM, seed Notebook B with the suggested sources via `prompts/notebook-b-ingestion-prompt.md`, then run `prompts/generator-prompt.md` for the first weekly plan

### Notebook A template you must follow

```markdown
# Notebook A — Framework & Constraints

## Section 1: User Profile
| Field | Value |
|---|---|
| Current weight | <value> |
| Goal weight | <value> |
| Total target change | <value> |
| Body composition goal | <value> |
| Training summary | <value> |

## Section 2: Daily Macro Target
Total calories: <value>/day

| Macro | Grams | Calories | % of Total |
|---|---|---|---|
| Protein | <value> | | |
| Carbohydrates | <value> | | |
| Fat | <value> | | |

Protein anchor rule: <state explicitly — current vs goal weight, and why>
Fat floor rule: <value g/day, and why>
Carb placement rule: <training day rule + rest day rule>

## Section 3: Meal Architecture
| Rule | Value |
|---|---|
| Meals per day | <value> |
| Protein per meal | <value> |
| Calories per meal | <value> |
| Post-workout meal | Largest meal; biggest carb portion; fastest-digesting protein |

MPS rule: ≥3 g leucine per meal, typically via 40 g+ high-quality animal protein.

## Section 4: Food Exclusions & Preferences
Hard exclusions: <list>
Soft avoids: <list>
Preferred flavor profiles: <list>
Preferred cooking methods: <list>

## Section 5: Supplement & Beverage Stack
| Item | Use | Macro contribution | Caffeine (mg) |
|---|---|---|---|
| <supplement> | <when> | <macros> | <mg> |

## Section 6: Phase Logic
<populate the phases that apply — Phase 0 ramp-in, Phase 1 cut, diet break, repeat cycle. Include calorie + macro targets for each, plus advance triggers>

## Section 7: Equipment Inventory
| Equipment | Status | Notes / fallback |
|---|---|---|

## Section 8: Training Context
<resistance / cardio frequency, meal timing relative to training>

## Section 9: Caffeine Ceiling
Daily limit: <mg total>
<list of caffeine sources from Section 5 with running total; generator flags if a day exceeds the ceiling>

## Section 10: Open Calibration Items
<list anything still being calibrated — treat as in-flux, do not encode as hard rules>

## Section 11: Generator Instructions
<keep verbatim from the template>
```

### Ready

Start by asking me the first batch of questions for Section 1 (user profile). Don't dump all the questions at once — interview me conversationally, adapting to my answers.
