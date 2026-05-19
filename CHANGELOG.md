# Changelog

All notable changes to this project will be documented in this file. The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.1.0] - 2026-05-18

Initial public release.

### Added
- **Notebook A template** (`notebook-a-framework.md`) — the rules engine: profile, macros, exclusions, supplement stack, phase logic, equipment, generator instructions.
- **Setup interview prompt** (`prompts/setup-interview-prompt.md`) — single-shot interview that populates Notebook A by interviewing the user and offering to research on their behalf. Includes a spoken disclaimer, BMR-input gathering before any calorie math, no system-context PII leakage, and avoidance of sensitive personal topics unless the user volunteers them.
- **Generator prompt** (`prompts/generator-prompt.md`) — weekly meal-plan generator producing meal grid, daily totals, grocery list, and parallel-appliance prep timeline.
- **Notebook B ingestion prompt** (`prompts/notebook-b-ingestion-prompt.md`) — structured ingestion into six inventories (proteins, carbs, vegetables, flavor stacks, bulk-cook techniques, meal templates).
- **Operational prompts** (`prompts/operational-prompts.md`) — three helpers for Notebook A maintenance, Notebook B ingestion handoff, and weekly plan generation.
- **Agent role definitions** (`docs/agents/roles/`) — `health-coach` (arbiter) and `meal-architect` (builder) for setups that split orchestration from production.
- **Worked example** (`examples/sample-meal-plan.md`) — 3-day illustrative output for a sample persona.
- README, MIT license, disclaimer, data & privacy notes, security policy, code of conduct.

### Design notes
- **AI-agnostic**: prompts assume nothing model-specific; ChatGPT, Claude, Codex, Gemini, and open-weights models all work.
- **Storage-agnostic**: reference documents can live in local markdown, a Claude Project / Custom GPT, a NotebookLM notebook, or anywhere else an AI can read them. The "Notebook A / B" naming is shorthand, not a NotebookLM dependency.
- **Caffeine ceiling, fat floor, and other safety-adjacent thresholds** are user-set placeholders, not hardcoded values.

[Unreleased]: https://github.com/DDub-Automation-Lab/meal-monster-framework/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/DDub-Automation-Lab/meal-monster-framework/releases/tag/v0.1.0
