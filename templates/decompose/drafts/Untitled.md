### 🔎 Phase 2 — Semantic Coverage Audit and SSS Elaboration

Using the finalized, ordered user story list and preliminary SSS from Phase 1, perform a semantic coverage audit before feature synthesis. This phase ensures that all user-story-level behaviors are semantically complete, domain edge classes are enumerated, and cross-cutting rules are captured in Shared System Semantics (SSS).

Phase 2 MUST be **completed before Phase 3 — Feature Synthesis** begins.

---

#### 1. Edge Case Enumeration

The LLM MUST audit all user stories from the ordered user story list from Phase 1 following the list order:

1. For each user story, inspect every item listed under its `#### Included Behavior` individually.
2. For each `#### Included Behavior` item, perform comprehensive enumeration of edge cases for each applicable category/example from the Edge Case Taxonomy.

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

1. For each each identified edge case, grade its SSS coverage on the scale:
    * **Covered** — current SSS explicitly defines the required rule or invariant.
    * **Partial** — SSS implies behavior but leaves material ambiguity.
    * **Not Covered** — SSS does not define the rule.
2. Propose resolutions for **Partial / Not Covered** cases by:
    - Adding a new SSS rule.
    - Revising an existing SSS rule.
    - Revising the affected user story.
    - Adding or revising an exception scenario.
    - Adding or revising state interaction declarations.
    - Asking a clarification question.
    - Deferring explicitly with justification.
    - Following the rules:
        * Repeated gaps across multiple stories are be resolved via SSS unless intentionally story-specific.
        * Deferred items document the unresolved case, reason for safe deferral, and where resolution will occur.
3. Produce a **Semantic Coverage Audit and Resolution Report** using Semantic Coverage Audit and Resolution Template.

---

#### 3. Revise User Story List and Preliminary SSS from Phase 1
##### SSS Elaboration

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

##### User Story Revision

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

#### 4. Completion Criteria

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

#### 5. Semantic Coverage Audit and Resolution

`````markdown
## Phase 2 Semantic Coverage Audit

### Audit Summary

### User Story Audit

#### US[N] — [User Story Name]

##### Included Behavior: [Behavior Item]

| Edge case | Coverage | Assessment | Required resolution |
| --------- | -------- | ---------- | ------------------- |
| [Case]    | Covered / Partial / Not Covered | [Assessment] | [Resolution] |

##### Required SSS Changes

- Add / revise / none: [SSS section or rule]

##### Required User Story Changes

- [Change or none]

---

### Proposed SSS Changes

#### Add SSS Section: [Section Name]

```markdown
### [Section Name]

**Definition**: [Definition]

1. [Normative rule]
2. [Normative rule]
3. [Normative rule]
```

#### Revise SSS Section: [Section Name]

```markdown
### [Section Name]

**Definition**: [Definition]

1. [Revised normative rule]
2. [Revised normative rule]
3. [Revised normative rule]
```

### Clarification Questions

1. [Question, if needed]
2. [Question, if needed]

### Phase 2 Validation Result

* [ ] Every user story was audited.
* [ ] Every `Included Behavior` item was audited.
* [ ] Every uncovered cross-cutting rule was promoted to SSS or explicitly deferred.
* [ ] Every affected user story references the appropriate SSS sections or rules.
* [ ] No user story duplicates SSS rules.
* [ ] No material semantic ambiguity remains before feature synthesis.
`````

The audit report is an intermediate analysis artifact. It MUST NOT replace the final `roadmap.md`.

