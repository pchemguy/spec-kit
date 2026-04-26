Yes — the right move is to insert the audit as a **new Phase 2: Semantic Coverage Audit**, then renumber current **Feature Synthesis** to **Phase 3**.

The audit is not merely a post-check. It changes the roadmap quality materially: it enumerates every `Included Behavior` item, derives domain edge classes, checks each against SSS, and promotes missing cross-cutting semantics into SSS. That is exactly the gap visible in `roadmap-v1-audit.md`: the generated roadmap looked structurally valid, but the audit found under-specified SSS around numeric token syntax, stack capacity, input buffer semantics, floating-point comparison/display, undo branching, and desktop portability scope. 

Your current `prespec-analysis.md` already says SSS is developed during Phase 1 and refined during Phase 2, followed by a dedicated SSS validation pass. But that pass is too generic; it does not force exhaustive traversal of each user story’s `Included Behavior` items. 

Below is the insertion-ready design.

---

# Recommended Structural Change

Current flow:

```text
Phase 1 — Exploration and Decomposition
Phase 2 — Feature Synthesis
SSS validation/refinement pass
Roadmap output
```

Recommended flow:

```text
Phase 1 — Exploration and User Story Decomposition
Phase 2 — Semantic Coverage Audit and SSS Elaboration
Phase 3 — Feature Synthesis
Phase 4 — Final SSS / Roadmap Validation
Roadmap output
```

The important part: **Feature synthesis should not happen until the user story list has been semantically audited and SSS has been enriched.** Otherwise, features inherit an incomplete SSS and their Agent Override sections will propagate ambiguity into `/speckit.specify`.

---

# Patch 1 — Update Session Objectives

Replace this block in `SESSION OBJECTIVES`:

```markdown
- assist user in performing a structured pre-specification analysis for a canonical GitHub Spec Kit workflow, including:
    1. decomposing the system into minimal, self-sufficient user stories according to Phase 1 and the User Story Decomposition Rules;
    2. synthesizing a sequence of cohesive features from those stories according to Phase 2 and the Feature Synthesis Rules;
    3. developing shared rules according to Shared System Semantics;
    4. producing a canonical `roadmap.md` according to the Report Templates.
```

with:

```markdown
- assist user in performing a structured pre-specification analysis for a canonical GitHub Spec Kit workflow, including:
    1. decomposing the system into minimal, self-sufficient user stories according to Phase 1 and the User Story Decomposition Rules;
    2. auditing every user story’s included behavior for domain edge classes, semantic coverage, and missing shared rules according to Phase 2 and the Semantic Coverage Audit Rules;
    3. synthesizing a sequence of cohesive features from the audited user stories according to Phase 3 and the Feature Synthesis Rules;
    4. developing, validating, and refining shared rules according to Shared System Semantics;
    5. producing a canonical `roadmap.md` according to the Report Templates.
```

---

# Patch 2 — Update Phase Gating

Replace:

```markdown
### ⛔ PHASE GATING  
  
The LLM MUST NOT produce a roadmap until:  
  
- Phase 1 (user story decomposition) is validated;  
- Phase 2 (feature synthesis) is validated;  
- SSS is complete and consistent.  
  
Premature roadmap generation is INVALID.
```

with:

```markdown
### ⛔ PHASE GATING  
  
The LLM MUST NOT produce a roadmap until:  
  
- Phase 1 (user story decomposition) is validated;
- Phase 2 (semantic coverage audit and SSS elaboration) is completed and validated;
- Phase 3 (feature synthesis) is validated;
- final SSS validation confirms that shared semantics are complete, consistent, non-duplicative, and referenced by all applicable user stories and features.
  
Premature roadmap generation is INVALID.

The LLM MUST NOT begin feature synthesis until:

- every user story has a complete `Included Behavior` section;
- every item in every `Included Behavior` section has been audited for domain edge classes;
- all uncovered or partially covered cross-cutting edge classes have been either:
    - promoted into SSS;
    - resolved by revising the affected user story; or
    - explicitly deferred with justification.
```

---

# Patch 3 — Replace Iteration Behavior Opening

Your current iteration section says both Phase 1 and Phase 2 follow the same iteration protocol. That becomes wrong once Phase 2 is semantic audit. Replace:

```markdown
Both user story decomposition (Phase 1) and feature synthesis (Phase 2) are iterative refinement processes and MUST follow the same iteration protocol.
```

with:

```markdown
User story decomposition (Phase 1), semantic coverage auditing (Phase 2), and feature synthesis (Phase 3) are iterative refinement processes.

Phase 1 and Phase 3 follow the structural iteration protocol below.

Phase 2 follows the Semantic Coverage Audit protocol because it validates semantic completeness rather than proposing decomposition or feature boundaries.
```

Then update this part:

```markdown
For the current phase:

1. Propose a candidate structure:
    - user story decomposition (Phase 1)
    - feature grouping (Phase 2)
```

to:

```markdown
For Phase 1 and Phase 3:

1. Propose a candidate structure:
    - user story decomposition (Phase 1)
    - feature grouping (Phase 3)
```

And update references:

```markdown
- Feature Synthesis Rules (Phase 2)
```

to:

```markdown
- Feature Synthesis Rules (Phase 3)
```

---

# Patch 4 — Insert New Phase 2

Insert this **between current Phase 1 and current Phase 2**.

`````markdown
---

### 🔎 Phase 2 — Semantic Coverage Audit and SSS Elaboration

Using the finalized ordered user story list from Phase 1, perform a semantic coverage audit before feature synthesis.

The purpose of this phase is to ensure that every user-story-level behavior is semantically complete, that all domain edge classes are identified, and that all cross-cutting rules required for consistent downstream specifications are captured in Shared System Semantics (SSS).

This phase MUST be completed before feature synthesis begins.

You MUST:

- inspect every user story in order;
- inspect every item listed under that user story’s `#### Included Behavior`;
- enumerate domain-relevant edge cases, boundary classes, state classes, invalid classes, exceptional classes, and continuity classes for each included behavior item;
- assess whether each edge case or class is covered by existing SSS;
- classify SSS coverage for each edge case or class as:
    - Covered;
    - Partially Covered;
    - Not Covered;
    - Not Applicable;
- identify whether missing or partial coverage should be resolved by:
    - adding or revising an SSS rule;
    - revising the affected user story;
    - adding an exception scenario;
    - adding or revising state interaction declarations;
    - asking a clarification question;
    - explicitly deferring the decision with justification;
- promote all cross-cutting rules into SSS rather than duplicating them in user stories;
- remove or avoid local restatement of rules that belong in SSS;
- revise affected user stories so that they reference the relevant SSS sections or rules;
- verify that every user story remains interaction-driven and does not become a container for global policy.

You MUST NOT:

- proceed to feature synthesis while any material edge class remains uncovered without explicit justification;
- leave cross-cutting semantic rules embedded only in user stories;
- duplicate SSS rules inside user story descriptions, included behavior, acceptance scenarios, or exception scenarios;
- introduce implementation-specific details unless they are already required by project constraints or constitution;
- convert edge cases into separate user stories unless they define independent user-initiated, value-producing interactions.

---

#### Semantic Coverage Audit Rules

For each user story, perform the audit at the granularity of individual `Included Behavior` items.

For each included behavior item, enumerate applicable classes from the following taxonomy.

##### 1. Valid Input / Valid Action Classes

Identify ordinary valid cases and meaningful variants, including:

- empty vs non-empty prior state;
- single-value vs multi-value state;
- first use vs later use;
- repeated use;
- minimum valid input;
- maximum or practically large valid input;
- ordinary representative examples;
- duplicate or repeated values where relevant.

##### 2. Invalid Input / Invalid Action Classes

Identify invalid cases, including:

- malformed input;
- missing input;
- insufficient state;
- domain-invalid values;
- forbidden operation combinations;
- non-finite or otherwise invalid results;
- invalid action after reset, undo, rejection, initialization, or other state boundary.

##### 3. Boundary and Numeric Classes

When the system involves numeric behavior, identify relevant numeric edge classes, including:

- zero;
- positive zero and negative zero, if floating-point semantics are relevant;
- positive and negative values;
- very small finite values;
- very large finite values;
- overflow to non-finite values;
- underflow to zero or subnormal values;
- integer vs non-integer values;
- precision, representation, and comparison expectations;
- values that parse differently from how they display.

##### 4. State and History Classes

When the behavior reads, mutates, preserves, resets, or records state, identify:

- initial state;
- reset state;
- state after accepted action;
- state after rejected action;
- state after undo or other history traversal;
- empty vs non-empty history;
- history boundaries;
- whether the action itself is recorded;
- whether feedback/status is part of state or excluded from state.

##### 5. Display / Feedback / UX Classes

When behavior affects visibility or feedback, identify:

- empty display;
- normal display;
- large or overflowing display;
- ordering and orientation;
- user-visible distinction between different states;
- feedback after accepted action;
- feedback after rejected action;
- non-modal vs blocking feedback constraints;
- accessibility or keyboard interaction if already in scope.

##### 6. Cross-Environment / Packaging Classes

When behavior spans environments or packaging modes, identify:

- first launch;
- subsequent launch;
- launch after previous use;
- portable operation requirements;
- unsupported environment behavior;
- persistence or non-persistence across launches;
- environment-specific affordances that must not alter semantics;
- behavioral parity between environments.

##### 7. Continuity Classes

For every user story after the first, identify:

- assumptions inherited from prior user stories;
- behavior that must remain unchanged from prior stories;
- state compatibility with prior features;
- whether the story extends, refines, or generalizes prior behavior;
- whether acceptance requires previous behavior to remain valid.

---

#### Coverage Assessment Rules

For each enumerated edge case or class, determine coverage using the following definitions.

- **Covered**: Existing SSS explicitly defines the required rule or invariant.
- **Partially Covered**: Existing SSS implies the behavior but leaves material ambiguity.
- **Not Covered**: Existing SSS does not define the behavior or relevant constraint.
- **Not Applicable**: The edge class does not apply to the target system or current user story, and the reason is clear.

A class marked **Partially Covered** or **Not Covered** MUST produce one of the following actions:

1. Add a new SSS rule.
2. Revise an existing SSS rule.
3. Revise the affected user story.
4. Add or revise an exception scenario.
5. Add or revise state interaction declarations.
6. Ask a clarification question.
7. Defer explicitly with justification.

If the same uncovered or partially covered class appears in more than one user story, it MUST be resolved through SSS unless it is intentionally story-specific.

---

#### SSS Elaboration Rules

When Phase 2 identifies missing shared semantics, the LLM MUST revise SSS using stable, reusable sections and numbered normative rules.

The LLM SHOULD create or revise SSS sections for recurring concerns such as:

- input token validity;
- value representation and comparison;
- state capacity and visibility;
- operation domain rules;
- rejected action behavior;
- reset and initialization boundaries;
- history and undo branching;
- display and feedback semantics;
- cross-environment consistency;
- desktop packaging boundaries;
- persistence or non-persistence expectations.

The LLM MUST ensure that new or revised SSS rules:

- are atomic;
- are enforceable;
- are testable or checkable;
- avoid implementation details unless required by project constraints;
- are referenced by every applicable user story;
- do not duplicate acceptance scenarios;
- do not encode user-story sequencing except as cross-feature continuity assumptions.

---

#### User Story Revision Rules After Audit

After SSS elaboration, the LLM MUST revise user stories as needed.

For each affected user story, the LLM MUST:

- update `Shared System Semantics References` to include newly applicable SSS sections or rule numbers;
- remove local restatements of rules promoted to SSS;
- keep `Scope` focused on the user-story boundary;
- keep `Included Behavior` focused on execution semantics of the user interaction;
- update `State Interaction` if the audit reveals missing reads, mutations, preservation, resets, or exclusions;
- update `Acceptance Scenarios` only where the primary successful behavior needs clearer observable outcomes;
- update `Exception Scenarios` where rejected behavior is story-specific and not fully covered by SSS.

The LLM MUST NOT expand user stories into global policy containers.

---

#### Required Phase 2 Audit Output During Iteration

During Phase 2, before asking the user to accept the audited user story set, the LLM MUST produce an audit report using the following structure.

````markdown
## Phase 2 Semantic Coverage Audit

### Audit Summary

- User stories audited:
- Included behavior items audited:
- Edge classes identified:
- Covered:
- Partially covered:
- Not covered:
- SSS sections added:
- SSS sections revised:
- User stories revised:
- Clarifications required:

### User Story Audit

#### US[N] — [User Story Name]

##### Included Behavior: [Behavior Item]

| Edge case / class | Coverage | Assessment | Required resolution |
| ----------------- | -------- | ---------- | ------------------- |
| [Case/class]      | Covered / Partially Covered / Not Covered / Not Applicable | [Assessment] | [Resolution] |

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
```

The Phase 2 audit output is an intermediate analysis artifact. It MUST NOT replace the final `roadmap.md` output. The final roadmap MUST still follow the Roadmap Template exactly.

---

#### Phase 2 Completion Criteria

Phase 2 is complete only when:

- every user story has been audited;
- every `Included Behavior` item has been audited;
- every material edge class is classified;
- every Partially Covered or Not Covered class has a resolution;
- all accepted SSS changes have been integrated into the roadmap SSS;
- all affected user stories have been updated to reference the revised SSS;
- all unresolved decisions have either been clarified with the user or explicitly deferred with justification;
- the user has accepted the audited user story set and revised SSS.

If these criteria are not satisfied, feature synthesis MUST NOT begin.

---
`````

---

# Patch 5 — Rename Current Phase 2 to Phase 3

Change:

```markdown
### 🧱 Phase 2 — Feature Synthesis
```

to:

```markdown
### 🧱 Phase 3 — Feature Synthesis
```

Then change:

```markdown
Using the finalized user story list, iteratively synthesize features as cohesive SpecKit work packets.
```

to:

```markdown
Using the finalized, semantically audited user story list and revised SSS from Phase 2, iteratively synthesize features as cohesive SpecKit work packets.
```

Also change:

```markdown
- operate strictly on the finalized, ordered user story list produced in Phase 1;
```

to:

```markdown
- operate strictly on the finalized, ordered, semantically audited user story list produced by Phases 1 and 2;
```

And add this bullet under `You MUST`:

```markdown
- ensure each feature’s Agent Override references all SSS sections and rules required by the Phase 2 audit;
```

---

# Patch 6 — Add Final Phase 4

Insert this after Feature Synthesis and before Report Templates.

```markdown
---

### 🧾 Phase 4 — Final SSS and Roadmap Validation

After feature synthesis is complete and before producing the final roadmap, perform a final validation pass across:

- SSS;
- user stories;
- feature grouping;
- feature Specify User Prompts;
- feature Agent Override sections.

You MUST verify:

1. Every user story is valid under the User Story Decomposition Rules.
2. Every user story was covered by Phase 2 semantic audit.
3. Every `Included Behavior` item has either:
    - sufficient SSS coverage;
    - story-local acceptance or exception coverage;
    - explicit justified deferral.
4. Every SSS section is referenced by at least one applicable user story or feature, unless justified as a global invariant.
5. No user story duplicates SSS rules.
6. No feature Specify User Prompt duplicates SSS rules unnecessarily.
7. Every feature Agent Override includes all applicable SSS sections and rule references.
8. Every feature includes only contiguous user stories from the accepted user story order.
9. Every user story is assigned to exactly one feature.
10. The first feature forms a defensible MVP when the target system is greenfield.
11. Later features are valid extensions of prior features.
12. The final roadmap follows the Roadmap Template and subtemplates exactly.

If any validation item fails, the LLM MUST revise the relevant prior phase output before producing the final roadmap.

---
```

---

# Patch 7 — Update Final Pre-Output Validation

Replace current validation list:

```markdown
1. The output top-level structure follows Roadmap Template - Top-Level Structure.
2. Every User Story follows the full subtemplate.
3. Every Feature follows the full subtemplate.
4. EVERY Feature contains Agent Override.
5. EVERY Agent Override contains ALL subsections.
6. No section is summarized or omitted.
```

with:

```markdown
1. Phase 1 user story decomposition was completed and validated.
2. Phase 2 semantic coverage audit was completed and validated.
3. All accepted Phase 2 SSS changes were integrated.
4. Phase 3 feature synthesis was completed and validated.
5. Phase 4 final SSS and roadmap validation passed.
6. The output top-level structure follows Roadmap Template - Top-Level Structure.
7. Every User Story follows the full subtemplate.
8. Every User Story contains complete `Included Behavior`.
9. Every User Story references all applicable SSS sections or rules.
10. Every Feature follows the full subtemplate.
11. EVERY Feature contains Agent Override.
12. EVERY Agent Override contains ALL required subsections.
13. EVERY Feature Agent Override references all applicable SSS sections or rules.
14. No section is summarized or omitted.
15. No roadmap-local rule duplicates an SSS rule.
```

---

# Important Fix: Agent Override Subsection Naming

Your enforcement block currently says Agent Override must contain:

```text
1. Shared Definitions, Conventions, and Policies
2. User Story Decomposition Constraints
3. User Story Decomposition (table)
```

But the actual Feature Subtemplate only has:

```text
###### Shared Definitions, Conventions, and Policies
###### User Story Decomposition
```

with the constraints and table inside `User Story Decomposition`.

So either change the enforcement block to match the template, or split the template into three headings. I recommend **matching the template**, because three headings are overkill.

Replace:

```markdown
Inside **Agent Override**, the LLM MUST include ALL subsections:

1. Shared Definitions, Conventions, and Policies
2. User Story Decomposition Constraints
3. User Story Decomposition (table)
```

with:

```markdown
Inside **Agent Override**, the LLM MUST include ALL required subsections:

1. Shared Definitions, Conventions, and Policies
2. User Story Decomposition

Inside `User Story Decomposition`, the LLM MUST include both:

- the canonical decomposition constraints;
- the canonical user story table.
```

This matters because `roadmap-v1.md` already follows the two-subsection pattern, with constraints and table inside `User Story Decomposition`. 

---

# Why This Works

The failure pattern is not that the roadmap ignored the template. It produced a structurally strong roadmap with SSS, user stories, features, and Agent Override blocks. The problem is that SSS was **semantically too shallow** for downstream `/speckit.specify`. For example, US2’s `Included Behavior` says “Accept numeric input / Push operand to stack / Display stack,” but the audit showed missing coverage for lexical numeric forms, negative literals, scientific notation, stack capacity, scrollability, top-of-stack orientation, and display format. 

Likewise, the audit found operator-domain holes: signed zero, underflow, reciprocal of `-0`, `pow(0, 0)`, negative bases with non-integer powers, and centralized `ln`/`sqrt` domain rules. 

And undo/desktop packaging had important unstated semantics: undo after undo, whether undo itself is recorded, whether status feedback is part of undoable state, launch after prior desktop use, portable operation, persistence, and desktop affordance boundaries. 

So the new Phase 2 should be treated as a **semantic hardening gate** between user-story decomposition and feature synthesis. It prevents weak SSS from being baked into feature prompts and Agent Overrides.
