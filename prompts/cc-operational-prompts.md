# Claude Code Prompts — Operational Helpers

Four prompts to paste into Claude Code (or any capable agent) as needed. Each one operationalizes a specific workflow against the two-notebook system.

---

## CC Prompt 1: Notebook A Maintenance

**When to use:** Updating the framework — phase changes, macro recalculations, new exclusions, supplement stack changes.

```
You are helping me maintain Notebook A — my framework and constraints document for a high-protein calorie-controlled meal planning system. The current version is in this conversation.

I want to make the following update:
[describe the change — e.g., "I'm transitioning to a diet break starting next Monday. Update Section 6 to reflect Phase 1 ended at week N, and recalculate the diet break macros assuming my new weight."]

Apply the change with these rules:
1. Preserve the document structure exactly — same section numbers, same table formats
2. Recalculate any dependent values (e.g., if weight changes, recalculate macros if anchored to weight)
3. Flag any downstream sections that are now inconsistent with the change
4. Output the full updated document as a single markdown file ready to re-upload to NotebookLM
5. At the end, summarize what changed in 3 bullets for my changelog
```

---

## CC Prompt 2: NotebookLM Ingestion Handoff

**When to use:** You've collected a batch of new sources (article links, recipe URLs, transcripts, your own notes) you want to add to Notebook B. This prompt prepares the ingestion instruction set so NotebookLM extracts content into the right structure.

```
I'm adding new sources to Notebook B (Techniques & Ingredients). I need you to prepare an ingestion instruction set that I'll paste into NotebookLM alongside the new sources.

The new sources are:
[paste URLs / titles, one per line]

The Notebook B ingestion prompt template is here:
[paste contents of notebook-b-ingestion-prompt.md]

Your task:
1. Review the source titles and predict which of the six inventories each will primarily populate (Proteins / Carbs / Veg / Flavor Stacks / Bulk Cooking / Meal Templates)
2. Generate a customized ingestion prompt that emphasizes those inventories for this batch
3. Add source-specific extraction hints if the source has a known format
4. Output the final prompt ready to paste into NotebookLM
5. After ingestion, I'll re-query Notebook B to verify the new entries were captured — give me 3 verification queries to run
```

---

## CC Prompt 3: Weekly Plan Generation (via MCP)

**When to use:** You want Claude Code (or another agent) to pull from both notebooks via an MCP connection and generate the week's plan directly.

```
Generate this week's meal plan by pulling from my two NotebookLM notebooks via MCP.

Variables:
- WEEK_OF: <date>
- CURRENT_PHASE: <phase>
- CURRENT_WEIGHT: <weight>
- WEEKS_INTO_PHASE: <n of N>
- TRAINING_DAYS: <days>
- PREP_DAY: <day>
- MEALS_PER_DAY: <3 or 4>
- SPECIAL_NOTES: <any one-offs>

Follow the generator prompt template stored at prompts/generator-prompt.md exactly. Use the MCP connection to:
1. Query Notebook A for current constraints — output the confirmed constraints before proceeding
2. Query Notebook B for inventory items matching the constraints
3. Build the weekly grid, totals, grocery list, prep schedule, and substitutions per the template
4. If any required inventory is empty in Notebook B (e.g., not enough proteins ranked by g/cal), flag it and pause for me to add sources before proceeding

Write the final plan to a new markdown file in this directory named meal-plan-<WEEK_OF>.md.
```

---

## CC Prompt 4 (bonus): Generate the initial Notebook A source file

**When to use:** First-time setup. Run this once to generate the markdown file you upload to NotebookLM as the seed for Notebook A.

```
Generate the source document for Notebook A based on the template in notebook-a-framework.md (provided in this conversation).

My current state for the file:
- Current weight: <weight>
- Goal: <goal weight>
- Phase: <current phase>
- Macros: <P / C / F / kcal>
- Meal frequency: <meals/day>
- Exclusions: <list>
- Supplement stack: <list>
- Training: <summary>

Output the complete document as a single markdown file, ready to upload to NotebookLM. Use the exact section structure from the template; replace every <PLACEHOLDER> with my values above.
```

---

## Workflow Summary

| Task | Tool | Prompt |
|---|---|---|
| Initial Notebook A setup | Claude Code | CC Prompt 4 |
| Update Notebook A (phase change, etc.) | Claude Code | CC Prompt 1 |
| Add sources to Notebook B | Claude Code → NotebookLM | CC Prompt 2 |
| Generate weekly plan (browser) | Gemini | Generator prompt with both notebooks attached |
| Generate weekly plan (CLI) | Claude Code via MCP | CC Prompt 3 |
