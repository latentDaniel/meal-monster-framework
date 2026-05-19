# Meal Monster

> A two-notebook, prompt-driven meal-planning system that turns **your** macros, exclusions, and ingredient library into a weekly plan, grocery list, and prep timeline.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Built for: NotebookLM](https://img.shields.io/badge/built%20for-NotebookLM-4285F4.svg)](https://notebooklm.google.com/)
[![Agent-ready: Claude Code](https://img.shields.io/badge/agent--ready-Claude%20Code-orange.svg)](https://claude.com/claude-code)

---

Most meal planners hand you somebody else's recipes and hope your goals fit. Meal Monster inverts that. It's a framework you fill in, not a database you browse — built for people running a structured cut, recomp, or lean-bulk who already know their macros, train seriously, and want every meal to enforce things like a 3 g leucine floor, a fat floor, carbs front-loaded around training, and a 400 mg caffeine ceiling.

## How it works

```
┌─────────────────────────────┐       ┌─────────────────────────────┐
│  Notebook A — Constraints   │       │  Notebook B — Inventory     │
│  macros · exclusions ·      │       │  proteins · carbs · veg ·   │
│  phase logic · equipment    │       │  flavor stacks · techniques │
│  (changes rarely)           │       │  (grows continuously)       │
└──────────────┬──────────────┘       └──────────────┬──────────────┘
               │                                     │
               └──────────────┬──────────────────────┘
                              ▼
                  ┌───────────────────────┐
                  │   Generator prompt    │
                  │  (Gemini or MCP-      │
                  │   capable agent)      │
                  └───────────┬───────────┘
                              ▼
              7-day meal grid · daily totals · grocery
              list · parallel prep-day timeline · subs
```

**Notebook A** is the rules engine — profile, macros, exclusions, supplement stack, equipment, phase logic with explicit ramp-in, diet break, and stall triggers.

**Notebook B** is your living ingredient and technique library, grown over time by feeding any source you trust — recipes, articles, cookbooks, video transcripts, your own notes — through a structured ingestion prompt that sorts everything into six inventories: proteins, carbs, volume vegetables, flavor stacks, bulk-cook techniques, and meal templates.

**The generator prompt** reads both notebooks weekly and emits a meal grid, daily totals with caffeine flags, a grocery list grouped by store section, and a prep-day timeline that schedules parallel appliance tracks — smoker, pressure cooker, oven, rice cooker — reconciling cooked-gram yield against the week's plan.

## Design philosophy

| Principle | What it means |
|---|---|
| **Constraints as data** | Every hard rule lives in Notebook A as structured prose. The generator quotes it back verbatim before producing any meal content — drift surfaces immediately. |
| **Inventory-grounded** | The generator never invents ingredients or techniques. Every protein, carb, veg, and bulk-cook method must trace to an entry in Notebook B. |
| **Quality gates are explicit** | The agent emits `PLAN_OK` / `PREP_OK` self-check blocks. A fail returns with revision deltas instead of silent edits. |
| **Equipment-aware prep** | The prep timeline assumes parallel appliance tracks and reconciles cooked-gram yield against the week's plan. |
| **Phase logic is first-class** | Ramp-in, aggressive cut, diet break, and phase advance / hold triggers are encoded in Notebook A — the agent enforces them rather than rediscovering them weekly. |

## Quick start

1. **Fork this repo.**
2. **Fill in `notebook-a-framework.md`** with your profile, macros, exclusions, supplement stack, equipment, and phase plan. Every `<PLACEHOLDER>` is yours to define.
3. **Create two NotebookLM notebooks** — *Meal Plan Requirements* (Notebook A) and *Meal Plan Ingredients and Techniques* (Notebook B). Upload your populated `notebook-a-framework.md` as the seed source for Notebook A.
4. **Seed Notebook B** by ingesting sources with [`prompts/notebook-b-ingestion-prompt.md`](prompts/notebook-b-ingestion-prompt.md). Good starting sources: recipe sites with macro data, cookbooks you already trust, transcripts from creators whose programming you follow, your own past meal logs. Aim for **3–6 entries per inventory** before generating your first plan — quality over quantity. Re-run ingestion any time you add new sources.
5. **Generate a weekly plan** by pasting [`prompts/generator-prompt.md`](prompts/generator-prompt.md) into Gemini with both notebooks attached, or invoking it via an MCP-capable agent. Fill in the variables block and the agent emits the artifact.
6. *(Optional)* Install the agent roles under [`docs/agents/roles/`](docs/agents/roles/) to split program orchestration (coach) from plan production (architect).

## Worked example

[`examples/sample-meal-plan.md`](examples/sample-meal-plan.md) shows what the generator emits when called against a populated Notebook A and a seeded Notebook B — three days of meals (two training, one rest), daily totals, grocery list, prep timeline, substitutions, and the `PLAN_OK` / `PREP_OK` self-check.

How the sample was produced — step by step:

1. **Notebook A** was filled in with the sample persona's macros (200 g P / 200 g C / 60 g F / ~2,140 kcal), goal-anchored protein, fat floor, 4-meal architecture, carb-placement rule, one hard exclusion (shellfish), and equipment list.
2. **Notebook B** was seeded with ~30 entries spread across the six inventories — proteins ranked by g/100 kcal, a handful of carbs and volume veg, five rotating flavor stacks, four bulk-cook techniques.
3. **The generator prompt** was invoked with the week's variables (current phase, training/rest days, prep day, meals/day, any special notes). The agent:
   - quoted Notebook A back verbatim into the `Confirmed Constraints` block, then
   - selected ranked inventory from Notebook B for the week, then
   - built the meal grid honoring per-meal protein, carb placement, variety, and exclusion rules, then
   - computed daily totals with caffeine flags, then
   - rolled the meal grid up into a consolidated grocery list and a parallel-appliance prep timeline, then
   - ran the self-check and emitted `PLAN_OK` / `PREP_OK`.
4. The whole artifact lands as a single markdown file — drop it on your fridge or feed it back into the loop next week.

In practice, a real run takes ~30 seconds of agent time once the notebooks are populated.

## Repository layout

```
notebook-a-framework.md          Notebook A template — fork & fill in
prompts/
  generator-prompt.md            Weekly meal-plan generator
  notebook-b-ingestion-prompt.md Structured ingestion into Notebook B
  cc-operational-prompts.md      Four operational helper prompts
examples/
  sample-meal-plan.md            3-day worked example (illustrative data)
docs/agents/roles/
  health-coach.role.md           Arbiter — program state + Notebook A
  meal-architect.role.md         Builder — weekly plan + prep, with gates
  README.md                      Boundaries + workflow between the two
```

## License

MIT — see [LICENSE](LICENSE).
