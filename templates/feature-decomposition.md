---
url: https://chatgpt.com/g/g-p-69e6210469388191b8880a8407594f1a-rpn-calculator/c/69e9987a-5974-83eb-a797-60c849059d3a
---

### Feature Decomposition

The LLM MUST assist in feature decomposition for a canonical GitHub Spec Kit workflow.

Your goal is to iteratively decompose a target system into a sequence of minimal, self-sufficient features and then produce a canonical `roadmap.md`.

Do NOT generate the roadmap immediately.

---

#### Motivation

> [!NOTE] Terminology Note
>
> - Features produced by the present feature decomposition workflow correspond to SpecKit user stories.
> - While "feature" is the preferred term, it should be treated as synonymous with "capability".
> - Superfeatures correspond to SpecKit features, the primary focus of `specify.md`. SpecKit features are "units of work" for SpecKit and are defined by the user prompt provided to the SpecKit `specify`.

Feature decomposition workflow is a "pre-specification" analysis of the target system focused on managing complexity of individual runs of the SpecKit core development loop (`specify → plan → tasks → implement`). The ultimate aim is to define a set of focused superfeatures for sequential execution by SpecKit and provide early user story decomposition. Each defined superfeature must represent either an MVP or a compact slice of functionality, relieving the SpecKit workflow from MVP/grouping/prioritization analysis/concerns.

The feature decomposition workflow MUST yield a list of features **ordered by implementation dependencies and value/functionality priority**, with the most important first, forming a feature development queue. Superfeatures are formed by slicing this queue into cohesive, focused subsets that naturally form an MVP or compact slice of functionality, while strictly preserving the order of included features and translating it into user story order.

Importantly, while the `spec-template.md` top HTML comment implies that each user story should represent "a viable MVP", the present approach rejects such coarse decomposition, requiring that the full user story set represents an MVP or compact slice of functionality, with higher feature granularity defined by the "Feature Decomposition Rules" below.

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
- ignore any prior similar analyses available from global or project context.

You MUST NOT:

- finalize the roadmap prematurely;
- assume unclear requirements without validation;
- group multiple capabilities into a single feature without justification;
- take advantage in this session of any prior similar analyses.

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
- fully aligned with the decomposition rules.

---

#### Phase 2 — Superfeature Synthesis Protocol

Superfeatures are **cohesive, focused groups of features** forming units of work for the canonical SpecKit loop starting from `specify`. This protocol defines how to synthesize superfeatures from a Phase 1 feature list.

---

##### Input

- Ordered list of features F[1…N] from Phase 1.
- Global policies and conventions from PREAMBLE.

---

##### Output

One or more superfeatures SF[1…M], each containing:

- Cohesive subset of roadmap features.
- Specify User Prompt describing the superfeature.
- Agent Override section including
    - Relevant global definitions, conventions, and policies.
    - Canonical user story table aligned with included features.

---

##### Stepwise Protocol

1. **Initialization**
    - Begin with the complete Phase 1 feature list.
    - Initialize an empty superfeature queue.
2. **Grouping**
    - Iteratively select features from the top of the queue.
    - Add them to the current superfeature until adding the next feature would violate semantic cohesion.
    - Maintain roadmap feature order; never reorder.
3. **Superfeature Finalization**
    - Assign an ID (SF[N]) and descriptive name summarizing included features.
    - Copy applicable global policies and conventions from PREAMBLE into Agent Override.
    - Generate the Specify User Prompt describing the combined behavior of included features.
    - Produce the canonical user story table with numbering reflecting feature order.
4. **Loop**
    - Remove finalized features from the queue.
    - Repeat grouping for subsequent superfeatures until all features are assigned.
5. **Validation**
   Verify:
     - All features are included in exactly one superfeature.
     - Superfeatures preserve roadmap order.
     - Agent Override user story numbering aligns with feature order.
     - Global policies are consistently applied.

---

#### Roadmap Template

```
# Roadmap | Roadmap: [Target Name]

## Notes

## PREAMBLE | PREAMBLE: [Scope]

## Features

### Feature F[N] — [Feature Name]

## Superfeatures

### Superfeature SF[N] — [Superfeature Name]

```

Notes:

- `## Notes` section is optional.
- `## PREAMBLE` section is optional. This is the home for **global definitions, conventions, policies, etc.** applicable to all features.
- Each `### Feature F[N] — [Feature Name]` subsection is populated following the `##### Feature Subtemplate`.
- Each `### Superfeature SF[N] — [Superfeature Name]` subsection is populated following the `##### Superfeature Subtemplate`.

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

#### Exceptions

Rejected scenarios and their expected behavior (must align with global policies).

---

```

---

##### Superfeature Subtemplate

```
### Superfeature SF[N] — [Superfeature Name]

Status: planned | in-progress | complete

#### Metadata

- ID: SF[N]
- Features included: F[M], F[M+1], … (list of features)
- Scope: textual description summarizing combined user-visible value

##### Specify User Prompt

[Superfeature description for the `/speckit.specify` command, unifying behavior of included features]

###### Agent Override

####### Global Definitions, Conventions, and Policies

Include all relevant definitions, conventions, and policies from PREAMBLE that apply to this superfeature.

####### User Story Decomposition Constraints

1. The following user story set is canonical for the spec.
2. Preserve exactly these user stories in the User Scenarios & Testing section.
3. Do not merge, split, reorder, rename, or re-prioritize these user stories.
4. Provide user story details for each story according to the spec-template.
5. Any additional detail must go into acceptance scenarios, edge cases, requirements, assumptions, non-goals, or success criteria, not into changing the story decomposition.

####### User Story Decomposition

Use exactly this canonical story set:

| #   | User Story                              |
| --- | --------------------------------------- |
| 1   | User Story 1 — Feature Name             |
| 2   | User Story 2 — Feature Name             |
| …   | …

---

```

---

#### One-Shot Roadmap Example

