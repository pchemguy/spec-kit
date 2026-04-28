### 🔎 Phase 2 — Semantic Coverage Audit and SSS Elaboration

Using the finalized, ordered user story list and preliminary SSS from Phase 1, perform a semantic coverage audit before feature synthesis. This phase ensures that all user-story-level behaviors are semantically complete, domain edge classes are enumerated, and cross-cutting rules are captured in Shared System Semantics (SSS).

Phase 2 MUST be **completed before Phase 3 — Feature Synthesis** begins.

---

#### 1. Audit Execution

The LLM MUST audit each user story in order. For each user story:

* Inspect every item listed under its `#### Included Behavior` individually.
* Enumerate applicable edge cases/classes using the Audit Taxonomy for each item.
* For each identified edge case/class:
    * Assess coverage against the current SSS.
    * Classify coverage using the Coverage Assessment Rules.
    * Resolve gaps using the Resolution Rules.
* Promote cross-cutting rules to SSS instead of duplicating them in user stories.
* Revise affected user stories to reference all relevant SSS sections.
* Verify that each user story remains interaction-driven and does not become a container for global policy.

The LLM MUST repeat this **audit-and-revision process** until all Phase 2 Completion Criteria are satisfied.

---

#### 2. Audit Taxonomy

For each included behavior item, the LLM MUST classify edge classes according to the following table. Only relevant classes apply; mark others as **Not Applicable**.

| Class                             | Description                                  | Examples / Notes                                                                                                                                                            |
| --------------------------------- | -------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Valid Input / Valid Action        | Ordinary valid cases and meaningful variants | empty / non-empty prior state, single vs multi-value, first vs later use, repeated use, minimum/maximum valid input                                                         |
| Invalid Input / Invalid Action    | Invalid cases                                | malformed input, missing input, insufficient state, domain-invalid values, forbidden operation combinations, non-finite results, invalid actions after reset/undo/rejection |
| Boundary / Numeric                | Numeric edge classes                         | zero, positive/negative zero, very small/large finite values, overflow/non-finite, underflow/subnormal, integer vs non-integer, precision & comparison                      |
| State & History                   | State read/write and historical behavior     | initial state, reset state, post-action state, post-rejection state, undo, history boundaries, feedback inclusion/exclusion                                                 |
| Display / Feedback / UX           | Visibility and user feedback                 | empty, normal, large/overflowing display, ordering, orientation, accepted/rejected feedback, non-modal vs blocking, accessibility                                           |
| Cross-Context / Cross-Environment | Environment, context, deployment, packaging  | first/subsequent use, persistence across sessions, unsupported environments, portability, behavioral parity                                                                 |
| Continuity                        | Cross-story behavior                         | assumptions inherited from prior stories, state compatibility, extension/refinement/generalization, acceptance dependencies                                                 |

---

#### 3. Coverage Assessment & Gap Resolution

For each enumerated edge class:

* Classify coverage:
    * **Covered** — current SSS explicitly defines the required rule or invariant.
    * **Partially Covered** — SSS implies behavior but leaves material ambiguity.
    * **Not Covered** — SSS does not define the rule.
    * **Not Applicable** — class is irrelevant to this system or story.
* Resolve **Partially Covered / Not Covered** classes by:
    - Adding a new SSS rule.
    - Revising an existing SSS rule.
    - Revising the affected user story.
    - Adding or revising an exception scenario.
    - Adding or revising state interaction declarations.
    - Asking a clarification question.
    - Deferring explicitly with justification.
* Repeated gaps across multiple stories MUST be resolved via SSS unless intentionally story-specific.
* Deferred items MUST document the unresolved class, reason for safe deferral, and where resolution will occur.

---

#### 4. SSS Elaboration

* Revise SSS using stable, numbered rules.
* Create/revise sections for recurring concerns, e.g.:
    * input validity, parsing, normalization
    * value representation and comparison
    * state structure, capacity, visibility
    * operation/action domain rules
    * rejected action behavior
    * initialization, reset, baseline boundaries
    * history, reversibility, recovery
    * display, feedback, observability
    * cross-context / cross-environment consistency
    * deployment, packaging, runtime environment
    * persistence expectations
* Each new/revised SSS rule MUST be atomic, enforceable, checkable, referenced by all applicable user stories, and avoid duplication of Acceptance Scenarios.
* SSS MUST NOT encode user-story sequencing except as cross-feature continuity.

---

#### 5. User Story Revision

For each audited story:

* Ensure `Acceptance Scenarios` cover primary successful behaviors in `Included Behavior`.
* Ensure `Exception Scenarios` cover rejected or invalid behaviors identified during audit.
* Align exception scenarios with SSS rules (rejection, no-mutation, feedback, state transitions).
* Acceptance scenarios should reference SSS where applicable, not duplicate it.
* Update:
    * `Shared System Semantics References`
    * `Scope` (focus on user-story boundary)
    * `Included Behavior` (reflect accepted behaviors)
    * `State Interaction` (reads, mutates, preserves, resets, must not affect)
* LLM MUST NOT expand stories into global policy containers.

---

#### 6. Output Artifact

* Produce a **Semantic Coverage Audit Report** using Semantic Coverage Audit Template.
* The report is an **intermediate artifact**; it does **not** replace the final `roadmap.md`.

---

#### 7. Completion Criteria

Phase 2 is complete only when:

* Every user story has been audited.
* Every `Included Behavior` item has been audited.
* Every accepted user story contains:
    * Acceptance Scenarios derived from Included Behavior
    * Exception Scenarios derived from audit and SSS rules
* Every material edge class is classified.
* Every Partially Covered / Not Covered class has a resolution.
* All accepted SSS changes are integrated into the current SSS.
* All affected user stories reference the revised SSS.
* No cross-cutting rule remains duplicated in user stories.
* All unresolved decisions are clarified or explicitly deferred.
* The user has accepted the audited user story set and revised SSS.

 If these criteria are not satisfied, **Phase 3 — Feature Synthesis MUST NOT begin**.

---
