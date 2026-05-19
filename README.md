# Meal Monster

> A two-notebook, prompt-driven meal-planning framework that turns **your** macros, exclusions, and ingredient library into a weekly plan, grocery list, and prep timeline.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Built for: NotebookLM](https://img.shields.io/badge/built%20for-NotebookLM-4285F4.svg)](https://notebooklm.google.com/)
[![Setup: single prompt](https://img.shields.io/badge/setup-single%20prompt-success.svg)](prompts/setup-interview-prompt.md)
[![AI-agnostic](https://img.shields.io/badge/AI--agnostic-bring%20your%20own%20LLM-lightgrey.svg)](#bring-your-own-ai)

---

Most meal planners hand you somebody else's recipes and hope your goals fit. Meal Monster inverts that. It's a framework you fill in, not a database you browse — built for people running a structured cut, recomp, or lean-bulk who already know their macros, train seriously, and want every meal to enforce things like a per-meal protein floor, a leucine threshold, carbs front-loaded around training, and a caffeine ceiling **you** define.

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
                  │  (any capable LLM)    │
                  └───────────┬───────────┘
                              ▼
              7-day meal grid · daily totals · grocery
              list · parallel prep-day timeline · subs
```

**Notebook A** is the rules engine — profile, macros, exclusions, supplement stack, equipment, phase logic with explicit ramp-in, diet break, and stall triggers.

**Notebook B** is your living ingredient and technique library, grown over time by feeding any source you trust — recipes, articles, cookbooks, video transcripts, your own notes — through a structured ingestion prompt that sorts everything into six inventories: proteins, carbs, volume vegetables, flavor stacks, bulk-cook techniques, and meal templates.

**The generator prompt** reads both notebooks weekly and emits a meal grid, daily totals with caffeine flags, a grocery list grouped by store section, and a prep-day timeline that schedules parallel appliance tracks — smoker, pressure cooker, oven, rice cooker — reconciling cooked-gram yield against the week's plan.

## Single-prompt setup

The hardest part of any framework is filling it in. Meal Monster ships with **one setup prompt** ([`prompts/setup-interview-prompt.md`](prompts/setup-interview-prompt.md)) that you drop into your AI of choice. It will:

- Interview you conversationally, in small batches of 2–4 questions at a time
- **Offer to research on your behalf** — macro calculation from body weight + goal, caffeine content of named supplements, technique substitutions for missing equipment, default fat / fiber floors with citations, sensible cut-and-diet-break schedules
- Push back when something looks off (protein anchored to current weight at very high body weight, a fat target that compromises hormones, an unsustainable deficit)
- Output a fully populated `notebook-a-framework.md` ready to upload to NotebookLM
- Suggest seed sources for Notebook B so your first weekly plan is one ingestion away

If you'd rather fill the template by hand, the placeholders in `notebook-a-framework.md` are all yours to define — but the interview is faster and catches mistakes.

## Bring your own AI

Meal Monster is **AI-agnostic**. Any capable LLM with NotebookLM access (either as a connected source or via MCP) can run the generator: ChatGPT, Claude, Gemini, open-weights models with the right tool wiring. The prompts assume nothing model-specific.

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
2. **Run the setup interview** — paste [`prompts/setup-interview-prompt.md`](prompts/setup-interview-prompt.md) into your AI of choice. Answer its questions; accept its research offers when convenient. Save its output as `notebook-a-framework.md`.
3. **Create two NotebookLM notebooks** — *Meal Plan Requirements* (Notebook A) and *Meal Plan Ingredients and Techniques* (Notebook B). Upload your populated `notebook-a-framework.md` as the seed source for Notebook A.
4. **Seed Notebook B** by ingesting sources with [`prompts/notebook-b-ingestion-prompt.md`](prompts/notebook-b-ingestion-prompt.md). Good starting sources: recipe sites with macro data, cookbooks you already trust, transcripts from creators whose programming you follow, your own past meal logs. Aim for **3–6 entries per inventory** before generating your first plan — quality over quantity. Re-run ingestion any time you add new sources.
5. **Generate a weekly plan** by pasting [`prompts/generator-prompt.md`](prompts/generator-prompt.md) into your AI with both notebooks reachable. Fill in the variables block and the agent emits the artifact.
6. *(Optional)* Install the agent roles under [`docs/agents/roles/`](docs/agents/roles/) to split program orchestration (coach) from plan production (architect).

## Worked example

[`examples/sample-meal-plan.md`](examples/sample-meal-plan.md) shows what the generator emits when called against a populated Notebook A and a seeded Notebook B — three days of meals (two training, one rest), daily totals, grocery list, prep timeline, substitutions, and the `PLAN_OK` / `PREP_OK` self-check.

How the sample was produced — step by step:

1. **Notebook A** was filled in (via the setup interview) with the sample persona's macros (200 g P / 200 g C / 60 g F / ~2,140 kcal), goal-anchored protein, fat floor, 4-meal architecture, carb-placement rule, one hard exclusion (shellfish), a 350 mg caffeine ceiling, and equipment list.
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
notebook-a-framework.md             Notebook A template — fork & fill in
prompts/
  setup-interview-prompt.md         One-shot setup: interview → populated Notebook A
  generator-prompt.md               Weekly meal-plan generator
  notebook-b-ingestion-prompt.md    Structured ingestion into Notebook B
  operational-prompts.md            Three operational helpers (maintenance, ingestion handoff, weekly run)
examples/
  sample-meal-plan.md               3-day worked example (illustrative data)
docs/agents/roles/
  health-coach.role.md              Arbiter — program state + Notebook A
  meal-architect.role.md            Builder — weekly plan + prep, with gates
  README.md                         Boundaries + workflow between the two
```

## Important disclaimer

This project is provided for **informational and educational purposes only**. It is **not** a medical device, does **not** provide medical or nutritional advice, and is **not** a substitute for professional diagnosis, treatment, or individualized care.

The meal plans, macro targets, and related suggestions generated by this software are general, non-clinical guidelines intended for recreational, self-experimentation, and learning. They may be incomplete, inaccurate, or inappropriate for your specific circumstances.

**Always consult a qualified healthcare professional** (such as a physician or registered dietitian) before making changes to your diet, exercise routine, or medication use — especially if any of the following apply:

- You are pregnant, trying to conceive, or breastfeeding
- You have a history of disordered eating
- You have a diagnosed medical condition (e.g., diabetes, heart disease, kidney disease, gastrointestinal disorders)
- You take prescription medications or have been advised to follow a specific diet
- You experience significant weight change, dizziness, fainting, chest pain, shortness of breath, or other worrying symptoms

By using this software, you acknowledge and agree that you use it **at your own risk**, that the authors and contributors make **no guarantees** about accuracy, safety, or suitability for any purpose, and that the authors and contributors are **not responsible** for any harm, injury, loss, or adverse outcome that may result from using or relying on this software or its outputs. This project is not developed, reviewed, or endorsed by medical professionals, and is not intended to comply with any medical, nutrition, or health regulation (including but not limited to FDA, HIPAA, or MDR).

## Data & privacy

This framework collects and processes information about your diet, body metrics, training, and personal preferences. **Treat any populated document, generated plan, or log produced by this system as sensitive personal health data.**

- Prefer running the system **locally** (on your computer or phone).
- **Do not** commit your populated `notebook-a-framework.md`, weekly plans, grocery lists, or prep schedules to **public** git repositories, shared drives, or other services you don't control. Keep them in a private location.
- If you use a hosted LLM, NotebookLM, or any cloud service, be aware that your prompts and uploaded sources may be stored and processed by third parties under their own terms and privacy policies. Review those before sharing anything you wouldn't want retained.

## License

MIT — see [LICENSE](LICENSE).
