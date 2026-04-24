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
> - Superfeatures are cohesive, focused groups of features forming units of work for the canonical SpecKit loop starting from `specify`.
> - Superfeatures correspond to SpecKit features - the primary focus of `specify.md`.

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

#### Phase 2 — Superfeature Synthesis

Using the finalized feature list, iteratively synthesize superfeatures as cohesive SpecKit work packets.

Your goal is to partition the ordered feature list into a sequence of superfeatures that define a valid execution plan for the SpecKit core workflow (`specify → plan → tasks → implement`), where each superfeature produces a coherent, bounded, and actionable specification input.

Do NOT generate superfeature definitions immediately.

You MUST:

* operate strictly on the finalized, ordered feature list produced in Phase 1;
* treat superfeature synthesis as a partitioning problem over a linear feature queue;
* propose an initial superfeature grouping that covers the full feature list based on the Superfeature Synthesis Rules below;
* explicitly justify superfeature boundaries in terms of MVP formation, functional slice definition, or extension semantics;
* ensure that each proposed superfeature forms a valid and meaningful `/speckit.specify` execution unit;
* verify that feature order is preserved exactly within and across superfeatures;
* ensure that every feature is assigned to exactly one superfeature;
* identify ambiguous or weak boundaries where grouping may be incorrect or unstable;
* refine superfeature boundaries when cohesion, scope, or execution clarity is compromised;
* ensure that the first superfeature forms a defensible MVP (if relevant, e.g., for a greenfield project) and that all subsequent superfeatures are valid extensions;
* ask targeted clarification questions when grouping decisions depend on unstated assumptions.

You MUST NOT:

* reorder features under any circumstances;
* split a feature across multiple superfeatures;
* group features solely for convenience;
* produce superfeatures that are too broad to serve as focused `/speckit.specify` inputs;
* produce superfeatures that are too narrow to represent meaningful functionality;
* leave gaps in the feature sequence;
* finalize superfeatures prematurely without evaluating boundary quality and execution suitability.

---

##### Superfeature Synthesis Rules

Each superfeature MUST:

* encapsulate a cohesive group of features that together define a meaningful, user-visible unit of functionality;
* include features in contiguous roadmap order;
* extend the system established by all prior superfeatures without requiring redefinition of previously delivered functionality;
* preserve all global definitions, conventions, and policies from the roadmap PREAMBLE;
* include all user stories corresponding to the included features in the canonical order.

---

Superfeature synthesis MUST strike a practical balance:

* the first superfeature in a greenfield roadmap MUST form a defensible MVP: minimal, but fully usable and testable as an end-to-end system slice;
* subsequent superfeatures MUST define cohesive slices of functionality, each meaningful in the context of prior superfeatures;
* superfeatures MUST NOT be fragmented so aggressively that closely related or structurally similar sequential features are separated without justification, causing unnecessary repetition of context, logic, or validation effort;
* each superfeature MUST be bounded in scope, sufficient for a single `/speckit.specify` execution, while avoiding arbitrary consolidation that dilutes focus.

---

Superfeature progression MUST preserve continuity:

* each superfeature MUST operate as a valid extension of the system produced by all prior superfeatures;
* the system state after each superfeature must remain internally consistent and testable.

---

Superfeature cohesion MUST be evaluated across included features:

* candidate features that are narrowly scoped and represent sequential refinements of the same capability, or
* candidate features that are tightly coupled, strongly parallel, or require substantially similar implementation, validation, or acceptance workflows

SHOULD be grouped together into a single superfeature when separate treatment would not meaningfully improve clarity, validation, or delivery confidence.

Reject or refine any superfeature that violates these constraints.

---

**Notes / Practical Guidance**

1. MVP superfeature (first in roadmap) must be minimal but viable, demonstrating end-to-end value with the smallest coherent subset of features.
2. Extension superfeatures must be defensible functional slices, strictly ordered after prior superfeatures.
3. Each superfeature is the unit of `/speckit.specify` execution. Its user story set corresponds to included features.
4. Ordering, completeness, and adherence to PREAMBLE policies are mandatory for all superfeatures.

---

#### Iteration Behavior

Both feature decomposition (Phase 1) and superfeature synthesis (Phase 2) are iterative refinement processes and MUST follow the same iteration protocol. 

For the current phase:

1. Propose a candidate structure:
    * Phase 1: feature list
    * Phase 2: superfeature grouping
2. Critically evaluate the structure against the applicable rules:
    * Phase 1: Feature Decomposition Rules
    * Phase 2: Superfeature Synthesis Rules
3. Identify:
    * ambiguity,
    * improper granularity,
    * weak cohesion,
    * invalid ordering,
    * unjustified separation or grouping
4. Ask targeted clarification questions where decisions cannot be made deterministically.
5. Refine the structure.

Repeat until the result is:

* correctly scoped:
    * minimal (Phase 1)
    * minimal but viable / coherent slice (Phase 2)
* well-structured:
    * well-separated features (Phase 1)
    * cohesive superfeatures (Phase 2)
* correctly ordered:
    * dependency-safe and value-prioritized (Phase 1)
    * strictly contiguous and progression-preserving (Phase 2)
* fully aligned with the applicable rule set.

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

