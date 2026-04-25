---
urls:
  - https://chatgpt.com/g/g-p-69e6210469388191b8880a8407594f1a-rpn-calculator/c/69e9987a-5974-83eb-a797-60c849059d3a
  - https://chatgpt.com/g/g-p-69e6210469388191b8880a8407594f1a-rpn-calculator/c/69eb74d5-0de4-83eb-8efc-16bad05c1955
  - https://chatgpt.com/g/g-p-69e6210469388191b8880a8407594f1a-rpn-calculator/c/69ebcc99-6ad4-83eb-aee3-f25f95b029fa
---

### Pre-specification Analysis

The LLM MUST assist in structured pre-specification analysis for a canonical GitHub Spec Kit workflow.

The goal is to:

1. iteratively decompose a target system into a sequence of minimal, self-sufficient user stories; and
2. synthesize those stories into a sequence of cohesive features, each forming an executable Spec Kit work packet;

and then produce a canonical `roadmap.md` capturing both levels.

Do NOT generate the roadmap until:

- the user story set satisfies the User Story Decomposition Rules; and
- the feature set satisfies the Feature Synthesis Rules.

Perform analysis in accordance with the Analysis Protocol and produce results according to the Report Template.

---

#### Motivation

This pre-specification analysis of the target system focuses on managing the complexity of individual runs of the SpecKit core development loop (`specify → plan → tasks → implement`). The user story decomposition workflow yields a list of user stories ordered first by implementation dependency requirements, then by value/functionality priority where dependencies permit, forming a user story development queue. Features are then formed by slicing this queue into cohesive, focused subsets that naturally form an MVP or compact, coherent functional slices.

Once a defensible MVP or coherent functional slice is defined, additional user stories MUST NOT be included in the same feature and MUST be deferred to subsequent features.

#### Analysis Protocol

You MUST:

- perform phased analysis of the described target system or project following the protocol below;
- generate a Markdown-structured report strictly following the "Report Template", including all:
    - required top-level sections;  
    - subtemplate-defined sections;  
    - applicable rules and constraints defined in this document;
- run every analysis session from scratch;
- ignore any prior similar analyses available from global or project context.

You MUST NOT:

- finalize the analysis prematurely;
- take advantage in this session of any similar analyses performed in other sessions.

All outputs produced under this framework MUST be internally consistent, non-duplicative, and reference-driven. Any rule that applies across multiple features MUST be defined in SSS and referenced, not restated.

---

##### Iteration Behavior

Both user story decomposition (Phase 1) and feature synthesis (Phase 2) are iterative refinement processes and MUST follow the same iteration protocol. 

For the current phase:

1. Propose a candidate structure:
    * user story decomposition (Phase 1)
    * feature grouping (Phase 2)
2. Critically evaluate the structure against the applicable rules:
    * User Story Decomposition Rules (Phase 1)
    * Feature Synthesis Rules (Phase 2)
3. Identify:
    * ambiguity,
    * improper granularity,
    * weak cohesion,
    * invalid ordering,
    * unjustified separation or grouping
4. Ask targeted clarification questions where decisions cannot be made deterministically.
5. Refine the structure.
6. Reject and rework the structure if it violates any mandatory rule or produces ambiguous or weak feature boundaries.

Repeat until the result is:

* correctly scoped:
    * minimal (Phase 1)
    * minimal but viable / coherent functional slice (Phase 2)
* well-structured:
    * well-separated user stories (Phase 1)
    * cohesive features (Phase 2)
* correctly ordered:
    * dependency-safe and value-prioritized (Phase 1)
    * strictly contiguous and progression-preserving (Phase 2)
* fully aligned with the applicable rule set.

Shared System Semantics (SSS) MUST be developed incrementally during Phase 1 and refined during Phase 2. After Phases 1 and 2 are considered complete, the LLM MUST perform a dedicated SSS validation and refinement pass:

- review the full accepted user story list and feature grouping;
- identify missing shared rules, implicit assumptions, or duplicated logic;
- promote cross-cutting rules into SSS;
- remove duplicated or conflicting definitions from user stories and features;
- revise SSS and affected user stories/features as necessary to ensure consistency, completeness, and absence of duplication;
- ensure all user stories and features can rely on SSS without redefining shared behavior.

---

##### Shared System Semantics (SSS)

The Shared System Semantics (SSS) is the authoritative home for global definitions, conventions, behavioral policies, invariants, and cross-cutting assumptions that apply to multiple features or constrain multiple superfeatures. Features and superfeatures MUST rely on the SSS by reference and MUST NOT restate, fork, override, or weaken SSS rules locally.

###### Construction Rules

The SSS MUST

- be developed as part of pre-specification analysis;
- capture all shared system-level definitions, conventions, and policies required for consistent feature and superfeature specification;
- be constructed incrementally during Phase 1 and refined during Phase 2;
- use stable section titles so that features and superfeatures can reference them without restating them.

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

Include the following categories when applicable to the system. Sections that are not applicable MUST be omitted.

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
7. Cross-Superfeature Continuity
    * shared assumptions that later superfeatures inherit from earlier completed system states
    * continuity expectations that preserve previously delivered behavior
    * no feature-specific sequencing instructions

---

###### Validation Rules

SSS MUST be validated against these rules:

- every section SHOULD be referenced by at least one feature or superfeature using templates
    - rule references: `SSS - [Section Title] - ([Rule Number])` (e.g., `SSS - Numeric Policy - (2)`);
    - section references: `SSS - [Section Title]` (e.g., `SSS - Numeric Policy`);
- unreferenced sections MUST be reviewed and either justified as global invariants or removed;
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

Self-sufficiency and independence mean that a feature can be implemented and validated as a coherent increment on top of all previously accepted features, without depending on future features to become meaningful, complete, correct, or testable.

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

Use the following top-level template and associated subtemplates.

##### Top-Level Template

```
# Roadmap | Roadmap: [Target Name]

## Notes *(if applicable)*

## Shared System Semantics (SSS)

## Features

### Feature F[N] — [Feature Name]

## Superfeatures

### Superfeature SF[N] — [Superfeature Name]

```

- Repeat Feature and Superfeature subsections as needed.

---

##### Shared System Semantics Subtemplate

```
## Shared System Semantics (SSS)

The following rules define shared system semantics and apply to all features and superfeatures.

Features and superfeatures MAY further constrain these rules but MUST NOT contradict, weaken, or bypass them.

---

### [Policy / Convention Name]

**Definition**: Short statement describing the purpose and scope of this rule group.

1. Normative rule statement
2. Normative rule statement
3. Normative rule statement

Optional:

N+1. Clarifying rule
N+2. Constraint
N+3. Boundary condition

---

### [Policy / Convention Name]

**Definition**: Short statement describing the purpose and scope of this rule group.

1. Normative rule statement
2. Normative rule statement

Optional:

N+1. Explicit evaluation rules
N+2. Ordering rules

- Examples (only when needed to disambiguate semantics)

---

### [Interaction / Evaluation Convention]

**Definition**: Defines how operations are interpreted or executed.

1. Rule defining evaluation order / semantics
2. Rule defining operand roles or interaction structure
3. Rule defining interpretation constraints

Optional:

- Examples illustrating interpretation

---

### [Error / Rejection Policy]

**Definition**: Defines how invalid actions are handled.

1. Conditions under which actions are rejected
2. Guarantee that rejected actions do not mutate state
3. Requirement for user-visible feedback
4. Constraints on feedback mechanism (e.g., non-modal, non-blocking)

---

### [State Model Assumptions]

**Definition**: Defines the conceptual structure of system state.

The system state MAY include, as applicable:

1. State component
2. State component
3. State component

Features MUST explicitly declare:

- which state components they read;
- which state components they mutate;
- which state components they preserve;
- which state components they reset.

---

### [State Mutation Policy]

**Definition**: Defines global guarantees about state transitions.

1. State changes occur only through accepted operations.
2. Rejected operations MUST NOT mutate any state.
3. State transitions MUST be atomic from the user perspective.

---

### [History / Reversibility Policy]

**Definition**: Defines undo/redo or historical behavior.

1. Rule defining what actions are recorded
2. Rule defining what actions are reversible
3. Explicit exclusions (e.g., rejected actions, resets)

---

### [Interaction and UX Conventions]

**Definition**: Defines user interaction and feedback expectations.

1. Input model assumptions
2. Feedback model (status, warnings, etc.)
3. Visibility constraints

---

### [Cross-Environment Consistency Constraint]

**Definition**: Defines invariants across deployment environments.

1. Behavior that MUST remain identical across environments
2. Explicit list of preserved semantics

---

```

###### Usage Rules

1. Inclusion Rule
    A section MUST be included only if:
    * it applies to more than one feature, or
    * it defines a shared/global invariant
    Otherwise → it belongs in a feature.
2. Coverage Rule
    - applicable shared semantic concerns MUST be captured in an existing or newly named SSS section
    - sections or items that do not apply MUST be excluded
3. Atomic Rule Style
    Each rule statement MUST:
    * express a single enforceable rule
    * be testable or checkable
    * avoid vague language ("should", "generally", etc.)
4. No Feature Leakage
    The SSS MUST NOT:
    * describe specific feature workflows
    * include acceptance scenarios
    * include UI layout or implementation details
    * encode feature sequencing
5. No Duplication Rule
    - Features and superfeatures MUST reference applicable SSS sections or rules in accordance with SSS Validation Rules when those rules materially affect behavior.
6. Naming Rule
    Each section name MUST:
    * reflect a distinct semantic concern
    * be reusable across systems (e.g., “Numeric Policy” → “Data Validity Policy” in another domain)
7. List Format Rule
    Sections MUST use:
    - numbered lists for rules to support specific references
    - bulleted lists for examples (no specific references)

---

##### Features Subtemplate

```
### Feature F[N] — [Feature Name]

Status: planned | in-progress | complete

#### Description

Short statement of intent and user-visible value.

#### Shared System Semantics References  
  
List applicable Shared System Semantics sections and rules:  
  
- SSS - [Section Title]  
- SSS - [Section Title] - (Rule Number)

#### Scope

#### Included Behavior

#### State Interaction

- Reads:  
- Mutates:  
- Preserves:  
- Resets:  
- Must Not Affect:

#### Acceptance Scenarios

1. **Given** [initial state], **When** [action], **Then** [expected outcome]
2. ...

#### Exception Scenarios

Rejected scenarios and their expected behavior (must align with global policies).

---

```

---

##### Superfeatures Subtemplate

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

The specification MUST inherit the roadmap SSS as authoritative shared context.

Applicable SSS definitions, conventions, policies, and rules for this superfeature:

- SSS - [Section Title 1]
- SSS - [Section Title 2] - (Rule Number)
- ...

Do not redefine, weaken, duplicate, or localize these rules inside the feature specification. Use them as inherited constraints when writing requirements, acceptance scenarios, edge cases, exceptions, assumptions, and success criteria.

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
