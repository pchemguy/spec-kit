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

#### 1. Edge Case Enumeration

The LLM MUST audit all user stories from the ordered user story list from Phase 1 following the list order:

1. For each user story, inspect every item listed under its `#### Included Behavior` individually.
2. For each `#### Included Behavior` item, perform comprehensive enumeration of edge cases for each applicable category/example from the Edge Case Taxonomy below.

**Edge Case Taxonomy**

- **Valid Input / Valid Action**: Ordinary valid cases and meaningful variants
    - empty / non-empty prior state,
    - single vs multi-value state, 
    - first vs later use, 
    - repeated use, 
    - minimum valid input,
    - maximum or practically large valid input,
    - ordinary representative examples,
    - duplicate or repeated values where relevant.
- **Invalid Input / Invalid Action**: Invalid cases
    - malformed input, 
    - missing input, 
    - insufficient state, 
    - domain-invalid values, 
    - forbidden operation combinations, 
    - non-finite results or otherwise invalid results, 
    - invalid actions after reset/undo/rejection/initialization/other state boundary.
- **Boundary / Numeric**: Numeric edge cases
    - zero, 
    - positive/negative zero, 
    - positive and negative values;
    - very small/large finite values, 
    - overflow/non-finite, 
    - underflow/subnormal, 
    - integer vs non-integer, 
    - precision/representation/comparison,
    - values that parse differently from how they display.
- **State & History**: State read/write and historical behavior
    - initial state, 
    - reset state, 
    - post-action state, 
    - post-rejection state, 
    - state after undo or other history traversal, 
    - empty vs non-empty history;
    - history boundaries, 
    - whether the action itself is recorded;
    - whether feedback/status is part of state or excluded from state.
- **Data Structures**: Data structures used in data model or state
    - stack overflow/underflow.
- **Display / Feedback / UX**: Visibility and user feedback
    - empty display, 
    - normal display, 
    - large/overflowing display, 
    - ordering and orientation, 
    - user-visible distinction between different states, 
    - accepted/rejected feedback, 
    - non-modal vs blocking, 
    - accessibility or keyboard interaction.
- **Cross-Context / Cross-Environment**: Environment, context, deployment, packaging
    - first/subsequent use, 
    - behavior after prior use,
    - portability or context-transfer requirements,
    - unsupported context or environment behavior,
    - persistence across sessions, 
    - context-specific affordances that must not alter semantics,
    - behavioral parity across supported contexts or environments.
- **Continuity**: Cross-story behavior
    - assumptions inherited from prior stories, 
    - behavior that must remain unchanged from prior stories,
    - state compatibility with prior completed behavior, 
    - whether the story extends, refines, or generalizes prior behavior, 
    - acceptance dependencies,
    - whether acceptance requires previous behavior to remain valid.

---

#### 2. Coverage Assessment and Proposed Resolution

1. Grade SSS coverage of each identified edge case on scale:
    * **Covered** — current SSS explicitly defines the required rule or invariant.
    * **Partial** — SSS implies behavior but leaves material ambiguity.
    * **Not Covered** — SSS does not define the rule.
2. Generate a detailed report listing

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
