##### Specify User Prompt

Create a feature specification for a system component implementing the following behavior:

###### Scope

This specification MUST cover the functionality defined by the following user stories:

- User Story N — [Feature Name]
- ...

The resulting specification MUST treat this set of user stories as a single coherent feature scope.

---

###### Global Definitions, Conventions, and Policies

The specification MUST strictly follow these definitions, conventions, and policies:

[Copy relevant content from PREAMBLE verbatim]

---

###### Behavioral Requirements

The specification MUST:

- define all user-visible behavior implied by the included user stories;
- define all state transitions and state interactions required by those behaviors;
- define all acceptance scenarios required to validate those behaviors;
- define all exception and rejection behavior consistent with the global policies;
- ensure that rejected operations do not alter system state unless explicitly specified otherwise;
- preserve consistency with all prior superfeatures (assumed implemented).

---

###### Constraints

The specification MUST:

- use exactly the provided user story set;
- NOT introduce new user stories;
- NOT merge, split, reorder, or rename user stories;
- treat this superfeature as a valid, self-contained extension of the system;
- assume all prior superfeatures are already implemented and correct.

---

###### Output Requirements

The output MUST:

- conform to the `spec-template.md`;
- include the canonical user story set in the *User Scenarios & Testing* section;
- fully elaborate each user story with acceptance scenarios;
- ensure internal consistency, completeness, and testability.

---
---

Good question. Right now `## PREAMBLE` is **structurally defined but procedurally undefined**, which means agents will either:

* under-specify it (missing critical rules), or
* over-specify it (dump everything there), or
* duplicate feature-level logic into it.

You need to treat PREAMBLE as a **first-class artifact with its own mini-protocol**, just like features and superfeatures.

---

# 🎯 What PREAMBLE actually is (formal definition)

You want to define it explicitly:

```md
## PREAMBLE — Global System Semantics
```

Then define:

> PREAMBLE contains **cross-cutting, invariant rules** that apply to **multiple features and multiple superfeatures** and MUST NOT be redefined locally.

That gives it a clear boundary.

---

# 🔴 Core Rule (this is the anchor)

```text
A rule belongs in PREAMBLE if and only if:

- it applies to more than one feature; and
- violating it would break system-wide consistency.
```

Everything else stays out.

---

# 🧱 Add a PREAMBLE Development Protocol

You need something like this:

---

## PREAMBLE Development Protocol

The PREAMBLE MUST be developed as part of pre-specification analysis and MUST capture all global system-level definitions, conventions, and policies required for consistent feature and superfeature specification.

---

### You MUST:

* identify all **cross-cutting rules** that affect multiple features;
* extract implicit assumptions from feature definitions and make them explicit;
* normalize terminology used across features into consistent definitions;
* define all **global behavioral invariants**;
* ensure that PREAMBLE content is **complete, minimal, and non-duplicative**;
* ensure that all features and superfeatures can rely on PREAMBLE without redefining shared behavior.

---

### You MUST NOT:

* include feature-specific behavior;
* include implementation details;
* duplicate acceptance scenarios from features;
* define rules that apply to only a single feature;
* leave critical system behavior implicit when it affects multiple features.

---

# 🧩 Required PREAMBLE Categories

To make this deterministic, you must force structure.

---

### PREAMBLE MUST define (when applicable):

###### 1. Core Domain Definitions

* fundamental entities (e.g., stack, operand, session, input)
* canonical terminology

---

###### 2. State Model

* what constitutes system state
* which state components exist globally
* persistence / reset expectations (if relevant)

---

###### 3. Global Behavioral Policies

Examples:

* numeric policy (valid values, rejection rules)
* operand order convention
* evaluation semantics
* precision / representation rules

---

###### 4. Error and Rejection Semantics

* when operations are rejected
* how rejection affects state (typically: no mutation)
* how errors are surfaced (e.g., status vs exception)

---

###### 5. Interaction and UX Conventions

* input model assumptions
* feedback model (e.g., warnings vs blocking)
* visibility requirements (e.g., stack display)

---

###### 6. Cross-Feature Constraints

* rules that constrain multiple features
* invariants that must always hold

---

###### 7. Superfeature-Level Assumptions

* assumptions about prior superfeatures
* continuity expectations

---

# 🔧 Add a Construction Rule (important)

Agents need to know *when* to create or extend PREAMBLE.

Add this:

```md
PREAMBLE MUST be constructed incrementally during Phase 1 and refined during Phase 2.

When a rule is identified that:
- affects more than one feature, or
- constrains behavior across superfeatures,

it MUST be promoted to PREAMBLE and removed from local feature definitions.
```

---

# 🔁 Add a Validation Rule

This prevents garbage accumulation:

```md
PREAMBLE MUST be validated such that:

- every rule is referenced by at least one feature or superfeature;
- no rule is duplicated in feature or superfeature definitions;
- all global assumptions required for consistent specification are explicitly defined;
- no feature depends on an unstated global rule.
```

---

# 🔴 Add a Hard Constraint (this is critical)

```md
All superfeatures MUST include relevant PREAMBLE content verbatim in the Agent Override section.

No superfeature may redefine or contradict PREAMBLE.
```

---

# 🧠 Resulting Mental Model

You now have:

```text
PREAMBLE = global invariants

Feature = local behavior

Superfeature = execution slice

Specify Prompt = generation contract
```

Each layer is cleanly separated.

---

# ⚠️ Common Failure Modes This Fixes

Without this, you will get:

* operand order defined in one feature but not others ❌
* inconsistent rejection semantics ❌
* undo behavior diverging ❌
* duplicated logic across specs ❌

With this, you enforce:

* **single source of truth**
* **zero duplication**
* **deterministic prompt construction**

---

# If You Want Next Step

The last piece to fully lock this system is:

👉 **deterministic PREAMBLE extraction into Agent Override**

Right now it says “include relevant content” — which is vague.

We can make it:

* rule-based,
* minimal,
* and non-redundant.

That’s the final step to make this system **fully mechanical**.

---
---

# Enforce Output Format

## 1. **Hard Output Contract** (this is the key missing piece)

Right now your prompt *describes* templates. That’s not enough.
You must **promote them to an enforceable contract with failure conditions**.

Add this block near the top:

```markdown
## STRICT OUTPUT CONTRACT (MANDATORY)

The LLM MUST produce output that is a fully expanded, literal instantiation of the Report Template and all Subtemplates.

The LLM MUST:

- include EVERY section defined in the template;
- include ALL required subsections, even if repetitive;
- NOT omit any subsection for brevity;
- NOT compress, summarize, or collapse structure;
- NOT replace sections with prose descriptions;
- NOT merge sections;
- NOT skip Agent Override blocks;
- fully expand Feature Subtemplate for EVERY feature;
- fully expand User Story Subtemplate for EVERY story.

The LLM MUST treat templates as a schema, not guidance.

If any required section is missing, the output is INVALID.

The LLM MUST self-verify compliance before returning the answer.
```

This alone eliminates ~80% of the compression problem.

---

## 2. **“No Compression” rule**

Right now it's implicit. Make it explicit and aggressive:

```markdown
## NO COMPRESSION RULE

The LLM MUST NOT:

- collapse repeated sections;
- omit “obvious” or “redundant” subsections;
- shorten Feature or User Story blocks;
- inline or summarize Agent Override sections;
- replace structured sections with narrative text.

Even if content appears repetitive, it MUST be rendered in full.
```

---

## 3. **Feature Subtemplate Enforcement Clause**

You specifically failed on `Agent Override`, so enforce it locally:

```markdown
## FEATURE TEMPLATE ENFORCEMENT

For EACH Feature, the LLM MUST include:

- Metadata
- Specify User Prompt
- Agent Override

Inside Agent Override, the LLM MUST include ALL subsections:

- Shared Definitions, Conventions, and Policies
- User Story Decomposition Constraints
- User Story Decomposition (table)

Omission of ANY subsection is a violation.
```

---

## 4. **Pre-Output Checklist (self-verification loop)**

Force the model to check itself before answering:

```markdown
## PRE-OUTPUT VALIDATION (MANDATORY)

Before returning the answer, the LLM MUST verify:

1. Every top-level section exists.
2. Every User Story follows the full subtemplate.
3. Every Feature follows the full subtemplate.
4. EVERY Feature contains an Agent Override section.
5. EVERY Agent Override contains ALL required subsections.
6. No section is summarized or omitted.

If any check fails, the LLM MUST fix the output before returning it.
```

This is critical—it prevents silent omissions.

---

## 5. **“Do Not Optimize for Brevity” directive**

The model *will* try to be helpful otherwise.

```markdown
## RESPONSE STYLE CONSTRAINT

This task prioritizes structural correctness over brevity.

The LLM MUST:

- prefer completeness over conciseness;
- produce verbose, fully expanded structured output;
- avoid any attempt to “improve readability” by reducing structure.
```

---

## 6. **Failure Mode Instruction**

```markdown
## FAILURE MODE

If the LLM cannot fit the full output within limits, it MUST:

- stop before truncation;
- explicitly state that output would exceed limits;
- ask to continue in multiple parts.

The LLM MUST NOT silently truncate or compress content.
```

---

## 7. Minimal Patch You Actually Need

If you don’t want to rewrite everything, this minimal addition is sufficient:

```markdown
## CRITICAL ENFORCEMENT

Templates are STRICT and MUST be followed literally.

DO NOT:
- omit any section
- compress repeated structures
- skip Agent Override
- summarize template content

DO:
- expand ALL sections fully
- repeat full structure for EACH feature

Output is INVALID if any template section is missing.
```

---
---

# ❌⚠️🔒🎯🚫🧩✅🔧🔁🧠📦🧱📄🧨🚀🔴

---

# AI Spec Kit Session Onboarding

#### ⚠️ Session Context Initialization Notice

This context defines session behavior only. It MUST be used to interpret all subsequent prompts.

The LLM MUST:

* operate strictly within the Spec Kit workflow defined below;
* assist in user story decomposition, feature synthesis, and roadmap generation;
* treat templates as **strict schemas**, not guidance.

---

##### 🔒 STRICT OUTPUT CONTRACT (MANDATORY)

The LLM MUST produce output that is a **fully expanded, literal instantiation** of all templates and subtemplates in Report Templates.

The LLM MUST:

* include **EVERY section** defined in the templates;
* include **ALL required subsections**, even if repetitive;
* fully expand **User Story Subtemplate** for EVERY story;
* fully expand **Feature Subtemplate** for EVERY feature;
* include **Agent Override sections for EVERY feature**;
* include **ALL nested Agent Override subsections**;
* preserve **exact structure and hierarchy**.

The LLM MUST treat templates as a **schema**, not guidance, and strictly follow Usage Rules.

The LLM MUST NOT:

* replace structured sections with prose;
* skip Agent Override;
* partially fill templates.

If any required section is missing → **OUTPUT IS INVALID**.
If any applicable conditionally required section is missing → **OUTPUT IS INVALID**.

---

##### 🎯 DO NOT OPTIMIZE FOR BREVITY - RESPONSE STYLE CONSTRAINT

This task prioritizes **structural correctness over brevity**.

The LLM MUST:

* prefer completeness over conciseness;
- produce verbose, fully expanded structured output;
- avoid any attempt to “improve readability” by reducing structure.

---
##### 🚫 NO COMPRESSION RULE

The LLM MUST NOT:

* omit sections “for brevity”;
* summarize template content;
* merge multiple sections into one;
* compress repetitive structures or sections;
* remove “redundant” subsections;
* shorten Feature or User Story blocks;
* inline or summarize Agent Override sections;
* reduce structural verbosity.

Even if content is repetitive, it MUST be rendered in full.

---

##### 🧩 FEATURE SUBTEMPLATE ENFORCEMENT

For EACH Feature, the LLM MUST include:

* Metadata
* Specify User Prompt
* Agent Override

Inside **Agent Override**, the LLM MUST include ALL subsections:

1. Shared Definitions, Conventions, and Policies
2. User Story Decomposition Constraints
3. User Story Decomposition (table)

Omission of ANY subsection is a **hard violation**.

---

##### ✅ PRE-OUTPUT VALIDATION (MANDATORY)

Before returning output, the LLM MUST verify:

1. The top-level sections exist
2. Every User Story follows the full subtemplate
3. Every Feature follows the full subtemplate
4. EVERY Feature contains Agent Override
5. EVERY Agent Override contains ALL subsections
6. No section is summarized or omitted

If any check fails → the LLM MUST fix the output before returning.

---

##### ⚠️ FAILURE MODE

If the LLM cannot fit the full output within limits, it MUST:

* stop BEFORE truncation;
* explicitly state that output would exceed limits and continuation is required;
- ask to continue in multiple parts;
* continue in additional messages.

The LLM MUST NOT silently truncate or compress content.

---

##### 🔧 Operating Objectives

The LLM MUST assist in:

1. decomposing the system into minimal, self-sufficient user stories;
2. synthesizing features from those stories;
3. producing a canonical `roadmap.md`.

---

##### 🔁 Iteration Protocol (MANDATORY)

For BOTH phases:

1. Propose candidate structure
2. Critically evaluate against rules
3. Identify issues:
    * ambiguity
    * incorrect granularity
    * weak cohesion
    * invalid ordering
4. Ask clarification questions if needed
5. Refine structure
6. Repeat until valid

The LLM MUST NOT finalize prematurely.

---

##### 🧠 Shared System Semantics (SSS)

SSS is the authoritative source of:

* global definitions
* behavioral rules
* invariants
* cross-cutting constraints

###### Rules

The LLM MUST:

* extract cross-cutting logic into SSS;
* eliminate duplication across stories/features;
* ensure all shared assumptions are explicit;
* enforce SSS references in stories and features.

The LLM MUST NOT:

* include user-story-specific behavior in SSS;
* duplicate SSS rules in stories/features;
* leave cross-cutting behavior implicit.

---

##### 📦 Phase 1 — User Story Decomposition

Each user story MUST:

* represent ONE user-initiated interaction;
* produce meaningful user value;
* define full interaction cycle;
* be independently implementable and testable;
* NOT represent only validation or constraints;
* NOT depend on future stories.

The LLM MUST classify each candidate:

* Interaction-driven → valid story
* Cross-cutting → SSS
* Internal → invalid

---

##### 🧱 Phase 2 — Feature Synthesis

Features are **contiguous partitions** of the story queue.

Each feature MUST:

* form a cohesive functional slice;
* preserve story order;
* be suitable for one `/speckit.specify` run;
* extend prior system state.

The first feature MUST be a **true MVP**.

---

##### 📄 Report Template (STRICT)

The LLM MUST produce EXACTLY:

```
# Roadmap | Roadmap: [Target Name]

## Notes *(if applicable)*

## Shared System Semantics (SSS)

## User Stories

### User Story US[N] — [Name]

(full subtemplate)

## Features

### Feature F[N] — [Name]

(full subtemplate INCLUDING Agent Override)
```

---

##### 🧨 CRITICAL ENFORCEMENT SUMMARY

* Templates are **STRICT SCHEMA**
* Missing section = **INVALID OUTPUT**
* Agent Override is **MANDATORY**
* NO compression under any circumstances
* MUST self-validate before returning

---

## 🚀 Result

With this version:

* the model cannot “optimize” structure away;
* Agent Override becomes **non-optional**;
* output becomes deterministic and Spec Kit–compatible;
* failures become **detectable instead of silent**.

---
