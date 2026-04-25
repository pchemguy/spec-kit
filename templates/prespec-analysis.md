---
urls:
  - https://chatgpt.com/g/g-p-69e6210469388191b8880a8407594f1a-rpn-calculator/c/69e9987a-5974-83eb-a797-60c849059d3a
  - https://chatgpt.com/g/g-p-69e6210469388191b8880a8407594f1a-rpn-calculator/c/69eb74d5-0de4-83eb-8efc-16bad05c1955
  - https://chatgpt.com/g/g-p-69e6210469388191b8880a8407594f1a-rpn-calculator/c/69ebcc99-6ad4-83eb-aee3-f25f95b029fa
---

### Pre-specification Analysis

The LLM MUST assist in structured pre-specification analysis for a canonical GitHub Spec Kit workflow.

> [!NOTE] Terminology Note
>
> - Features produced by the present feature decomposition workflow correspond to SpecKit user stories.
> - Superfeatures are cohesive, focused groups of features forming units of work for the canonical SpecKit loop starting from `specify`.
> - Superfeatures correspond to SpecKit features - the primary focus of `specify.md`.
> - In this workflow, the term "feature" means a roadmap-level user-story candidate, not a SpecKit `/speckit.specify` feature. A "superfeature" is the SpecKit `/speckit.specify` feature.
 

The goal is to:

1. iteratively decompose a target system into a sequence of minimal, self-sufficient features; and
2. synthesize those features into a sequence of cohesive superfeatures forming executable Spec Kit work packets;

and then produce a canonical `roadmap.md` capturing both levels.

Do NOT generate the roadmap  until:

- the feature set satisfies the Feature Decomposition Rules; and
- the superfeature set satisfies the Superfeature Synthesis Rules.

Perform analysis in accordance with the Analysis Protocol and produce results according to the Report Template.

---

#### Motivation

Feature decomposition workflow is a "pre-specification" analysis of the target system focused on managing complexity of individual runs of the SpecKit core development loop (`specify → plan → tasks → implement`). The ultimate aim is to define a set of focused superfeatures for sequential execution by SpecKit and provide early user story decomposition. Each defined superfeature must represent either an MVP or a compact slice of functionality, relieving the SpecKit workflow from MVP/grouping/prioritization analysis/concerns.

The feature decomposition workflow MUST yield a list of features **ordered by implementation dependencies and value/functionality priority**, with the most important first, forming a feature development queue. Superfeatures are formed by slicing this queue into cohesive, focused subsets that naturally form an MVP or compact slice of functionality, while strictly preserving the order of included features and translating it into user story order.

Importantly, while the `spec-template.md` top HTML comment implies that each user story should represent "a viable MVP", the present approach rejects such coarse decomposition, requiring that the full user story set represents an MVP or compact slice of functionality, with higher feature granularity defined by the "Feature Decomposition Rules" below.

---

#### Analysis Protocol

You MUST:

- perform phased analysis of the target described system or project following the protocol below;
- generate a Markdown-structured report following the "Report Template" format, including
    - Top-Level Structure;
    - all subtemplates;
    - all defined rules;
- run every analysis session from scratch;
- ignore any prior similar analyses available from global or project context.

You MUST NOT:

- finalize the analysis prematurely;
- take advantage in this session of any similar analyses performed in other sessions.

---

##### Iteration Behavior

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
6. Reject and rework the structure if it violates any mandatory rule or produces ambiguous or weak superfeature boundaries.

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

To complete Shared System Semantics, you MUST:

- adapt iteration protocol above defined for phased analysis;
- review the full accepted output of Phases 1-2;
- identify gaps in shared rules, conventions, or policies;
- revise both Shared System Semantics and output of Phases 1-2 as necessary to 

Development of the "Shared System Semantics" section is performed in parallel to the phased analysis. After Phases 1-2 are considered accepted, you MUST:

- adapt the "Iteration Behavior" protocol (this section) to refinement of the "Shared System Semantics" section;
- review the full accepted output of Phases 1-2

---

##### Shared System Semantics (SSS)

The Shared System Semantics section of the report template contains cross-cutting, invariant rules, conventions, and policies that apply to multiple features and multiple superfeatures and MUST NOT be redefined locally.

###### Construction Rules

The SSS MUST

- be developed as part of pre-specification analysis;
- capture all shared system-level definitions, conventions, and policies required for consistent feature and superfeature specification;
- be constructed incrementally during Phase 1 and refined during Phase 2.

You MUST:

* identify all cross-cutting rules that affect multiple features;
* extract implicit assumptions from feature definitions and make them explicit;
* normalize terminology used across features into consistent definitions;
* define all shared behavioral invariants;
* ensure that SSS content is complete, minimal, and non-duplicative;
* ensure that all features and superfeatures can rely on SSS without redefining shared behavior.

You MUST NOT:

* include feature-specific behavior;
* include implementation details;
* duplicate acceptance scenarios from features;
* define rules that apply to only a single feature;
* leave critical system behavior implicit when it affects multiple features.

When a rule is identified that:

- affects more than one feature, or
- constrains behavior across superfeatures,

it MUST be promoted to SSS and removed from local feature definitions.

---

###### Essential Categories

SSS MUST define (when applicable)

1. Core Domain Definitions
    * fundamental entities
    * canonical terminology
2. State Model
    * what constitutes system state
    * which state components exist globally
    * persistence / reset expectations (if relevant)
3. Global Behavioral Policies
   Examples:
    * numeric policy (valid values, rejection rules)
    * normal state mutation conventions
    * evaluation semantics
    * precision / representation rules
4. Error and Rejection Semantics
    * when operations are rejected
    * how rejection affects state (typically: no mutation)
    * how errors are surfaced (e.g., status vs exception)
5. Interaction and UX Conventions
    * input model assumptions
    * feedback model (e.g., warnings vs blocking)
    * visibility requirements
6. Cross-Feature Constraints
    * rules that constrain multiple features
    * invariants that must always hold
7. Superfeature-Level Assumptions
    * assumptions about prior superfeatures
    * continuity expectations

---

###### Validation

SSS MUST be validated such that:

- every rule is referenced by at least one feature or superfeature;
- no rule is duplicated in feature or superfeature definitions;
- all shared assumptions required for consistent specification are explicitly defined;
- no feature depends on an unstated shared rule.

---

##### Phase 1 — Exploration and Decomposition

First, analyze the target system and guide the user through decomposition.

You MUST:

- identify major capabilities of the system;
- propose an initial feature breakdown;
- explicitly evaluate each proposed feature against the decomposition rules below;
- identify ambiguities, coupling, or oversized features;
- suggest splits or refinements where needed;
- ask targeted clarification questions when decomposition is uncertain;

You MUST NOT:

- assume unclear requirements without validation;
- group multiple capabilities into a single feature without justification;

---

###### Feature Decomposition Rules

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

##### Phase 2 — Superfeature Synthesis

Using the finalized feature list, iteratively synthesize superfeatures as cohesive SpecKit work packets.

Your goal is to partition the ordered feature list into a sequence of superfeatures that define a valid execution plan for the SpecKit core workflow (`specify → plan → tasks → implement`), where each superfeature produces a coherent, bounded, and actionable specification input.

Superfeature synthesis operates by introducing explicit boundaries ("cuts") in the ordered feature list. Each cut defines the end of one superfeature and the beginning of the next. All synthesis decisions MUST be expressed as placement, removal, or adjustment of these boundaries.

You MUST:

* operate strictly on the finalized, ordered feature list produced in Phase 1;
* treat superfeature synthesis as a partitioning problem over a linear feature queue;
* propose an initial superfeature grouping that covers the full feature list based on the Superfeature Synthesis Rules below;
* explicitly justify superfeature boundaries in terms of MVP formation, functional slice definition, or extension semantics;
* ensure that each proposed superfeature defines a coherent, self-contained specification scope suitable for a single `/speckit.specify` execution;
* verify that feature order is preserved exactly within and across superfeatures;
* ensure that every feature is assigned to exactly one superfeature;
* identify ambiguous or weak boundaries where grouping may be incorrect or unstable;
* refine superfeature boundaries when cohesion, scope, or execution clarity is compromised;
* ensure that
    * the first superfeature forms a defensible MVP when the system is introduced from an empty or initial state
    * all subsequent superfeatures are valid extensions;
* ask targeted clarification questions when grouping decisions depend on unstated assumptions.

You MUST NOT:

* reorder features under any circumstances;
* split a feature across multiple superfeatures;
* group features without a clear justification in terms of MVP formation, functional cohesion, or extension semantics;
* produce superfeatures that are too broad to serve as focused `/speckit.specify` inputs;
* produce superfeatures that are too narrow to represent meaningful functionality;
* leave gaps in the feature sequence;
* finalize superfeatures prematurely without evaluating boundary quality and execution suitability.

A valid superfeature set is complete only when:

- all features are assigned to exactly one superfeature;
- all superfeatures are contiguous slices of the feature list;
- the first superfeature forms a defensible MVP (when applicable);
- each subsequent superfeature is a valid extension slice;
- all superfeatures satisfy the Superfeature Synthesis Rules;
- the full sequence defines a valid execution plan for the SpecKit workflow.

---

###### Superfeature Synthesis Rules

Each superfeature MUST:

* encapsulate a cohesive group of features that together define a meaningful, user-visible unit of functionality;
* include features in contiguous roadmap order;
* extend the system established by all prior superfeatures without requiring redefinition of previously delivered functionality;
* preserve all shared definitions, conventions, and policies from the roadmap SSS;
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
4. Ordering, completeness, and adherence to Shared System Semantics policies are mandatory for all superfeatures.

---

#### Report Template

##### Top-Level Structure

```
# Roadmap | Roadmap: [Target Name]

## Notes *(if applicable)*

## Shared System Semantics (SSS)

## Features

## Superfeatures

```

Notes:

- Other `##` sections MUST be populated following respective subtemplates and any defined usage rules below.

---

##### Shared System Semantics

---

##### Feature Subtemplate

```
### Feature F[N] — [Feature Name]

Status: planned | in-progress | complete

#### Description

Short statement of intent and user-visible value.

#### Scope

#### Included Behavior

#### State Interaction

- Reads:
- Mutates:
- Must Not Affect:

#### Acceptance Scenarios

1. **Given** [initial state], **When** [action], **Then** [expected outcome]
2. ...

#### Exception Scenarios

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

####### Shared Definitions, Conventions, and Policies

Include all relevant definitions, conventions, and policies from SSS that apply to this superfeature.

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
| ... | ...                                     |

---

```

---
