---
url: https://chatgpt.com/g/g-p-69e6210469388191b8880a8407594f1a-rpn-calculator/c/69e9987a-5974-83eb-a797-60c849059d3a
---

### Feature Decomposition

The LLM MUST assist in feature decomposition for a canonical GitHub Spec Kit workflow.

Your goal is to iteratively decompose a target system into a sequence of minimal, self-sufficient features and then produce a canonical `roadmap.md`.

Do NOT generate the roadmap immediately.

---

#### PREAMBLE

Feature decomposition workflow aims to perform which is essentially User Story decomposition performed by GitHub Spec Kit (the "Fill User Scenarios & Testing section" instruction from the `specify.md` command file). While in the context of the SpecKit workflow, a "FEATURE" is unit of SpecKit work scoped by user prompt provided to the `specify` command, "FEATURE" in the context of present work is equivalent of a user story in the context of SpecKit commands. However, while in the context of SpecKit, each user story should represent "a viable MVP" (as stated in the `spec-template.md` top HTML comment), in present context a finer granularity is required, meaning an MVP would typically be a group of features.

The objective is to rather than letting SpecKit perform user story decomposition without human in the loop, the described protocol (and template) enable interactive (HITL) decomposition of the development target. Then, the resulting features (or there subset) may be added directly to the `spec.md` file as user stories or passed to the `specify` command as user stories to be used, instead of performing decomposition by the agent.

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

#### Phase 2 — Superfeatures as SpecKit Work Packets

---

#### Roadmap Template

```
# Roadmap | Roadmap: [Target Name]

## Notes

## PREAMBLE | PREAMBLE: [Scope]

## Features

### Feature F[N] — [Feature Name]

## Superfeatures

_(Superfeatures are cohesive focused groups of `## Features` forming a unit of work for the canonical SpecKit loop starting from specify)_


```

Notes:

- `## Notes` section is optional and may not be present.
- `## PREAMBLE` section is optional. This is the home for global definitions, conventions, policies, etc. applicable to all features. This section should be framed as a section of a specification.
- Each feature subsection `### Feature F[N] — [Feature Name]` is populated following the `##### Feature Subtemplate` below.

---

##### Feature Subtemplate

```
### Feature F[N] — [Feature Name]

Status: planned | in-progress | complete

#### Description
Short statement of intent and user-visible value.

#### Scope

##### Included Behavior

##### Excluded Behavior (optional)

#### State Interaction

- Reads:
- Mutates:
- Must Not Affect:

#### Acceptance Scenarios

1. **Given** [initial state], **When** [action], **Then** [expected outcome]
2. ...

#### Edge Cases

Valid but non-trivial scenarios that must be handled correctly.

#### Exceptions

Rejected operations and their expected behavior (must align with global policies).

#### Preconditions (optional)

Conditions that must be satisfied for the feature to be applicable.

```

#### One-Shot Roadmap Example

