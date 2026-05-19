# Meal Planner Framework — Agent Roles

| Role | Archetype | Purpose |
|---|---|---|
| [health-coach](health-coach.role.md) | arbiter | Maintains Notebook A + program state, dispatches the architect, surfaces phase transitions. User-facing. |
| [meal-architect](meal-architect.role.md) | builder | Produces `meal-plan-{WEEK_OF}.md` end-to-end. Dispatched by the coach. |

## Boundaries

```
notebook-a-framework.md          ← health-coach owns, meal-architect read-only
meal-plan-*.md                   ← meal-architect owns, health-coach read-only
state/program-state.md           ← health-coach owns
prompts/**                       ← read-only to both
```

## Workflow

1. User asks `health-coach` for the week's plan.
2. Coach checks `program-state.md` freshness + Section 6 phase-transition triggers.
3. Coach dispatches `meal-architect` with the inputs block.
4. Architect queries both notebooks, builds the artifact, emits `PLAN_OK` + `PREP_OK`, writes `meal-plan-{WEEK_OF}.md`.
5. Coach verifies the gates and presents a summary to the user.
