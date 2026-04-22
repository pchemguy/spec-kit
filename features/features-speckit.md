---
url: https://chatgpt.com/g/g-p-69e6210469388191b8880a8407594f1a-rpn-calculator/c/69e842e4-1524-83eb-8979-ded19381dba1
---
Generate a feature roadmap for the following project.

The output MUST be a complete `roadmap.md` document following the repository’s canonical structure.

## Objective

Decompose the target system into an ordered sequence of minimal features suitable for execution via `/speckit.specify`.

## Input

```
$ARGUMENTS
```

---

## Feature Decomposition Rules

Each feature MUST:

- be minimal in scope;
- be self-sufficient;
- be independently implementable and testable;
- deliver standalone, user-visible value;
- NOT depend on future features to become meaningful;
- NOT combine unrelated or merely convenient future work.

A feature MUST NOT be:

- a partial slice that requires completion by another feature;
- purely internal or infrastructural without delivering user-visible value;
- an arbitrary grouping of multiple capabilities.

---

## Ordering Rules

Features MUST be ordered such that:

- each feature can be implemented after all preceding features;
- earlier features provide foundational or core capabilities;
- later features extend or enhance existing behavior;
- no feature requires a later feature to be usable.

Do NOT introduce MVP, phases, or grouping labels. The roadmap itself defines the delivery sequence.

---

## Output Requirements

Output MUST be a valid `roadmap.md` document.

### Structure

- Use one section per feature.
- Each feature section MUST include:
  - feature ID (F001, F002, …)
  - name
  - status
  - description
  - `### Specify Prompt`

### Status

- All features MUST have `Status: planned`

### Specify Prompt

Each feature MUST include a `### Specify Prompt` section whose contents:

- define exactly one feature;
- are directly usable as input to `/speckit.specify` without modification;
- include:
  - clear capability definition;
  - constraints and invariants where applicable;
  - required behaviors;
- MUST NOT:
  - reference other roadmap features;
  - include implementation details;
  - include planning, tasks, or architecture.

---

## Quality Constraints

The roadmap MUST:

- contain no overlapping features;
- contain no gaps in user-visible capability progression;
- avoid redundancy;
- avoid vague feature definitions;
- avoid “future work” placeholders;
- ensure each feature can be specified independently.

---

## Output Only

Return ONLY the complete `roadmap.md` document.
Do not include explanations, commentary, or analysis.
