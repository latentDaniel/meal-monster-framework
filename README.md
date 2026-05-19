# Meal Monster

> A prompt-driven meal-planning framework that turns **your** macros, exclusions, and ingredient library into a weekly plan, grocery list, and prep timeline. Pair it with any capable AI.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Setup: single prompt](https://img.shields.io/badge/setup-single%20prompt-success.svg)](prompts/setup-interview-prompt.md)
[![AI-agnostic](https://img.shields.io/badge/AI--agnostic-bring%20your%20own%20LLM-lightgrey.svg)](#what-youll-need)
[![Storage: your choice](https://img.shields.io/badge/storage-your%20choice-informational.svg)](#where-to-store-your-reference-documents)

---

Most meal planners hand you somebody else's recipes and hope your goals fit. Meal Monster inverts that. It's a framework you fill in, not a database you browse — built for people running a structured cut, recomp, or lean-bulk who already know their macros, train seriously, and want every meal to enforce things like a per-meal protein floor, a leucine threshold, carbs front-loaded around training, and a caffeine ceiling **you** define.

## What you'll need

Meal Monster is the framework; an AI is what runs it. You'll need:

1. **An AI you trust to read text and follow instructions.** Anything frontier-class works: Claude Code, Codex, ChatGPT, Claude in a Project, Gemini, etc. The prompts assume nothing model-specific.
2. **Somewhere to keep your reference documents.** See [Where to store your reference documents](#where-to-store-your-reference-documents) below — options range from "a folder of markdown files on your laptop" to "a NotebookLM notebook with RAG over dozens of sources." Pick what fits your habits.
3. **About 20 minutes** for the one-shot setup interview that produces your Notebook A.

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
                  │  (your AI of choice)  │
                  └───────────┬───────────┘
                              ▼
              7-day meal grid · daily totals · grocery
              list · parallel prep-day timeline · subs
```

**Notebook A** is the rules engine — profile, macros, exclusions, supplement stack, equipment, phase logic with explicit ramp-in, diet break, and stall triggers. It's one document; you populate it once and update it rarely.

**Notebook B** is your living ingredient and technique library, grown over time by feeding any source you trust — recipes, articles, cookbooks, video transcripts, your own notes — through a structured ingestion prompt that sorts everything into six inventories: proteins, carbs, volume vegetables, flavor stacks, bulk-cook techniques, and meal templates.

**"Notebook" is just our shorthand for "a reference document your AI can read."** The names come from NotebookLM, where this framework was first built, but Meal Monster isn't tied to it. Both notebooks can live wherever you want.

**The generator prompt** reads both reference documents weekly and emits a meal grid, daily totals with caffeine flags, a grocery list grouped by store section, and a prep-day timeline that schedules parallel appliance tracks — smoker, pressure cooker, oven, rice cooker — reconciling cooked-gram yield against the week's plan.

## Where to store your reference documents

Pick whichever fits how you already work — Meal Monster doesn't care:

| Option | Best for | How it works |
|---|---|---|
| **Local markdown files** | You already use a file-aware AI tool (Claude Code, Codex, Aider, Cursor). Simplest possible setup. | Save `notebook-a-framework.md` and a `notebook-b.md` (or a folder) in a directory. Your tool reads them directly. |
| **Claude Project or Custom GPT** | You want persistent context in a dedicated chat thread, no file-aware tool required. | Upload both files to a Project / GPT once. The model has them in every conversation. |
| **NotebookLM** | Notebook B is growing past a dozen sources and you want RAG-style queries across all of them. | Create two notebooks; upload Notebook A as a single source, ingest Notebook B sources via the ingestion prompt. Pair with Gemini or any AI that can talk to NotebookLM (directly or via MCP). |

You can mix-and-match (e.g., local Notebook A + NotebookLM for Notebook B) or move between options as your setup evolves. The prompts work the same way regardless.

## Single-prompt setup

The hardest part of any framework is filling it in. Meal Monster ships with **one setup prompt** ([`prompts/setup-interview-prompt.md`](prompts/setup-interview-prompt.md)) that you drop into your AI of choice. It will:

- Interview you conversationally, in small batches of 2–4 questions at a time
- **Offer to research on your behalf** — macro calculation from body weight + goal, caffeine content of named supplements, technique substitutions for missing equipment, default fat / fiber floors with citations, sensible cut-and-diet-break schedules
- Push back when something looks off (protein anchored to current weight at very high body weight, a fat target that compromises hormones, an unsustainable deficit)
- Output a fully populated `notebook-a-framework.md` ready to save into your chosen reference store
- Suggest seed sources for Notebook B so your first weekly plan is one ingestion away

If you'd rather fill the template by hand, the placeholders in `notebook-a-framework.md` are all yours to define — but the interview is faster and catches mistakes.

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
2. **Run the setup interview** — paste [`prompts/setup-interview-prompt.md`](prompts/setup-interview-prompt.md) into your AI. Answer its questions; accept its research offers when convenient. Save its output as `notebook-a-framework.md`.
3. **Pick your reference store** — see the table above. Save Notebook A to it.
4. **Seed Notebook B** by ingesting sources with [`prompts/notebook-b-ingestion-prompt.md`](prompts/notebook-b-ingestion-prompt.md). Good starting sources: recipe sites with macro data, cookbooks you already trust, transcripts from creators whose programming you follow, your own past meal logs. Aim for **3–6 entries per inventory** before generating your first plan — quality over quantity. Re-run ingestion any time you add new sources.
5. **Generate a weekly plan** by pasting [`prompts/generator-prompt.md`](prompts/generator-prompt.md) into your AI with both reference documents reachable. Fill in the variables block and the agent emits the artifact.
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

Meal Monster only processes the information **you** share with it about your diet, body metrics, training, and personal preferences. Any populated document, generated plan, or log this system produces should be treated as **personal health data**.

**The framework itself has no telemetry, no analytics, and no phone-home.** It doesn't transmit, sync, or share your information beyond your own environment, the AI tooling you've chosen to pair it with, and the locations where you save its output. The only parties that ever see your data are you, your chosen AI, and any storage service you decide to put it in.

**It's recommended that you keep this information private** — store it somewhere you trust (your own device, a private cloud you control, or a service whose privacy posture you've reviewed). It's your decision if you choose to share it more broadly; just make that decision deliberately.

- Avoid committing populated documents, weekly plans, grocery lists, or prep schedules to **public** git repositories or shared drives unless you intend them to be public.
- If you pair Meal Monster with a hosted LLM, NotebookLM, a Claude Project, or any cloud service, your prompts and uploaded files may be stored and processed by that provider under their own terms and privacy policies. Review those before sending anything you wouldn't want retained.

## License

MIT — see [LICENSE](LICENSE).
