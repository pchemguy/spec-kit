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

