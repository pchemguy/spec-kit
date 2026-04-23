### Feature Decomposition

#### PREAMBLE

Feature decomposition workflow aims to perform which is essentially User Story decomposition performed by GitHub Spec Kit (the "Fill User Scenarios & Testing section" instruction from the `specify.md` command file). While in the context of the SpecKit workflow, a "FEATURE" is unit of SpecKit work scoped by user prompt provided to the `specify` command, "FEATURE" in the context of present work is equivalent of a user story in the context of SpecKit commands. However, while in the context of SpecKit, each user story should represent "a viable MVP" (as stated in the `spec-template.md` top HTML comment), in present context a finer granularity is required, meaning an MVP would typically be a group of features.

The objective is to rather than letting SpecKit perform user story decomposition without human in the loop, the described protocol (and template) enable interactive (HITL) decomposition of the development target. Then, the resulting features (or there subset) may be added directly to the `spec.md` file as user stories or passed to the `specify` command as user stories to be used, instead of performing decomposition by the agent.

#### Protocol

##### Feature Decomposition Rules

Each feature MUST:

- be minimal in scope relative to its delivered value;
- be self-sufficient;
- be independently implementable and testable;
- deliver standalone, user-visible value;
- encapsulate a coherent unit of behavior with consistent logic, state, workflow, and architectural context;
- NOT combine unrelated or merely convenient future work; and
- NOT depend on future features to become meaningful, complete, or correct, or to validate its core behavior.

Feature decomposition MUST strike a practical balance:

- features MUST be small enough to support focused implementation, bounded context, and deterministic agent behavior; and
- features MUST NOT be fragmented so aggressively that closely related, strongly parallel, or structurally similar behavior is split into multiple features whose implementation would substantially duplicate logic, state handling, workflow shape, architectural structure, or Spec Kit process overhead.

Feature sequencing MUST support progressive system evolution:

- the earliest feature MUST establish a minimal but complete and usable system slice that can be executed and validated end-to-end; and
- each subsequent feature MUST meaningfully extend, refine, or generalize the behavior established by earlier features without requiring redefinition of previously delivered capabilities.

Feature progression MUST preserve continuity:

- each feature MUST operate as a valid extension of the system produced by prior features; and
- the system produced after each feature MUST remain usable and internally consistent.

Feature cohesion MUST be evaluated across candidate features:

- candidate features that are narrowly scoped and represent sequential refinements of the same capability; or
- candidate features that are tightly coupled, strongly parallel, or would require substantially similar implementation and validation workflows

SHOULD be combined into a single feature when separate treatment would not meaningfully improve clarity, validation, or delivery confidence.

Reject or refine any feature that violates these constraints.

---

#### Roadmap Template

```
# Roadmap | Roadmap: Target Name

## Notes

## PREAMBLE | PREAMBLE: Scope

## Features

### Feature [N] — [Feature Name]

Status: planned | in-progress | complete

#### Description
```

Notes:

- `## Notes` section is optional and may not be present.
- `## PREAMBLE` section is optional. This is the home for definitions, conventions, etc. applicable to all features. This section should be framed as a section of a specification.

#### One-Shot Roadmap Example


The repository MAY maintain a `roadmap.md` document alongside `constitution.md` and `progress.md`.

When present, `roadmap.md` MUST:

- define an ordered list of features, each beginning with a second-level heading `## [Feature Name]`;
- have unique feature names (`## [Feature Name]`) across the document;
- contain "### Specify User Prompt" subsection in each feature section that must represent a user prompt to be used with `/speckit.specify` without modification;
- record only features that satisfy the feature-level decomposition constraints defined by this constitution.

`/speckit.specify` MUST operate on exactly one feature.

That feature MUST be defined either:

- explicitly in the user prompt; or
- implicitly by selecting the earliest feature in `roadmap.md` having the `planned` status.

When `/speckit.specify` processes a feature from the `roadmap.md` file, the agent must include an additional metadata `Roadmap Feature: [Feature Name]`, where `[Feature Name]` must match exactly feature section title in `roadmap.md`.

When both an explicit feature definition and `roadmap.md` are provided, `/speckit.specify` MUST NOT proceed without explicit user confirmation of the selected feature.

When using feature from `roadmap.md`, 

`roadmap.md` represents planned intent and MUST NOT be treated as implemented state.



### 3. Feature Decomposition

The LLM MUST assist in feature decomposition for a canonical GitHub Spec Kit workflow.

Your goal is to iteratively decompose a target system into a sequence of minimal, self-sufficient features and then produce a canonical `roadmap.md`.

Do NOT generate the roadmap immediately.

---

#### Phase 1 — Exploration and Decomposition

First, analyze the target system and guide the user through decomposition.

You MUST:

- identify major capabilities of the system;
- propose an initial feature breakdown;
- explicitly evaluate each proposed feature against the decomposition rules below;
- identify ambiguities, coupling, or oversized features;
- suggest splits or refinements where needed;
- ask targeted clarification questions when decomposition is uncertain;
- run every feature decomposition session from scratch;
- ignore any prior similar analyses, which might be available from global or project context.

You MUST NOT:

- finalize the roadmap prematurely;
- assume unclear requirements without validation;
- group multiple capabilities into a single feature without justification;
- take advantage in this session of any prior similar analyses, which might be available from global or project context.

---


##### Iteration Behavior

Work interactively:

1. Propose a candidate feature list.
2. Critically evaluate it.
3. Ask for clarification or confirm assumptions.
4. Refine the feature set.

Repeat until the feature set is:

- minimal,
- well-separated,
- correctly ordered,
- and fully aligned with the decomposition rules.

---

#### Phase 2 — Roadmap Synthesis

Only when the user explicitly confirms that the decomposition is complete:

- generate a complete `roadmap.md`.

---

##### Roadmap Output Requirements

The output MUST:

- follow the canonical `roadmap.md` structure;
- include one second-level section (`##`) per feature;
- assign `Status: planned` to all features;
- include a `##### Specify User Prompt` for each feature.

Each `##### Specify User Prompt` MUST:

- define exactly one feature;
- be directly usable as input to `/speckit.specify`;
- avoid implementation details;
- avoid references to other features.


