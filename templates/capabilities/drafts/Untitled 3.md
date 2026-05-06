### Validation

The LLM MUST validate the candidate capability set and extracted NFFF aspect classification using the **Validation Algorithm**.

If the Validation Algorithm detects any failure, the LLM MUST:

1. apply the applicable Revision Strategy;
2. revise the capability set;
3. restart validation from Step 1.

Validation results MUST be presented according to the **Report Template**.

The LLM MUST return only an output where:

- no validation failures remain;
- all Boundary Decisions are `Keep`;
- `Result` is `VALID`.

---

#### Validation Algorithm

The LLM MUST validate the capability set using the following ordered procedure.

Validation MUST proceed step-by-step.  
If any step fails, the LLM MUST:

1. record the failure;
2. classify it according to the defined **Failure Types**;
3. revise the capability set;
4. restart validation from Step 1.

The LLM MUST NOT skip steps or continue after a failure without revision.

---

##### Step 1 — Signal Grounding Validation

Verify that every capability is grounded in capability signals.

Checks:

- Every capability is supported by ≥1 capability signal.
- Every explicit or strongly implied capability signal is represented by ≥1 capability.

Failure Types:

- `UNSUPPORTED_CAPABILITY` — capability has no signal support.
- `UNMAPPED_SIGNAL` — signal not represented in any capability.

---

##### Step 2 — Capability Model Classification Validation

Verify correct and exclusive classification.

Checks:

- Every capability is assigned to exactly one category:
    - Core User Capability;
    - Supporting Functional Capability;
    - NFFF Aspect.
- No capability mixes category semantics.

Failure Types:

- `MULTI_CATEGORY_CAPABILITY`
- `UNCLASSIFIED_CAPABILITY`
- `CATEGORY_CONTAMINATION`

---

##### Step 3 — Core User Capability Integrity

Verify that core intent is correctly isolated.

Checks:

- At least one capability represents each identified Core User Capability.
- No capability classified as Core User Capability is subsumed by or includes:
    - solution form;
    - interaction model;
    - technology;
    - access mode;
    - delivery context;
    - supporting functional logic;
    - NFFF aspects.

Failure Types:

- `MISSING_CORE_CAPABILITY`
- `CORE_SUBSUMED`
- `CORE_CONTAMINATION`

---

##### Step 4 — NFFF Classification and Promotion Validation

Verify NFFF handling.

Checks:

- Every identified NFFF aspect appears exactly once in the classification table.
- Every table row is grounded in ≥1 capability signal.
- Every promotable NFFF aspect is represented by exactly one capability.
- No promoted NFFF aspect is embedded in non-NFFF capabilities.

Failure Types:

- `NFFF_MISSING`
- `NFFF_DUPLICATED`
- `NFFF_UNGROUNDED`
- `NFFF_NOT_PROMOTED`
- `NFFF_EMBEDDED`

---

##### Step 5 — Capability Construction Integrity

Verify structural validity of capabilities.

Checks:

- No capability represents only:
    - an implementation mechanism;
    - a task;
    - a processing step.
- Every capability has:
    - one user-recognizable purpose;
    - one dominant user intent;
    - a coherent scope signal.

Failure Types:

- `INVALID_CAPABILITY_TYPE`
- `NO_USER_INTENT`
- `INVALID_SCOPE_SIGNAL`

---

##### Step 6 — Splitting Validation

Verify that no capability is overloaded.

Checks:

- No capability contains candidate fragments that:
    - serve different user-recognizable purposes;
    - imply materially different user intents;
    - imply materially different access, environment, or runtime expectations.

Failure Types:

- `SHOULD_SPLIT`

---

##### Step 7 — Grouping Validation

Verify that no capabilities are artificially separated.

Checks:

- No pair of capabilities exists such that both:
    - serve the same user-recognizable purpose;
    - share the same user intent;
    - share the same access, environment, and runtime expectations.

Failure Types:

- `SHOULD_MERGE`

---

##### Step 8 — Boundary Consistency Validation

Verify global boundary integrity.

Checks:

- No capability scopes overlap.
- All capabilities are:
    - category-consistent;
    - semantically cohesive;
    - non-redundant.

Failure Types:

- `OVERLAPPING_CAPABILITIES`
- `REDUNDANT_CAPABILITIES`
- `INCOHERENT_BOUNDARY`

---

##### Step 9 — Boundary Decision Validation

For each capability, assign a Boundary Decision:

- `Keep`
- `Split`
- `Merge`
- `Revise`

Checks:

- `Keep` is valid only if:
    - no `SHOULD_SPLIT` condition applies;
    - no `SHOULD_MERGE` condition applies;
    - no structural failures apply.

Failure Types:

- `INVALID_KEEP_DECISION`

---

#### Revision Strategies

When a validation failure is detected, the LLM MUST apply the corresponding revision strategy.

Revisions MUST:

- be minimal and localized;
- preserve unaffected capabilities;
- maintain consistency with capability signals;
- NOT introduce new unsupported capabilities.

After applying a revision, the LLM MUST restart the **Validation Algorithm** from Step 1.

---

##### Failure Type → Revision Strategy

###### Signal Grounding

- `UNSUPPORTED_CAPABILITY`  
    → Remove the capability OR replace it with one derived from valid capability signals.
- `UNMAPPED_SIGNAL`  
    → Introduce a new capability candidate OR extend an existing capability to cover the signal.

---

###### Classification

- `UNCLASSIFIED_CAPABILITY`  
    → Assign the capability to exactly one Capability Model category based on dominant semantics.
- `MULTI_CATEGORY_CAPABILITY`  
    → Split the capability into separate capabilities, each with a single category.
- `CATEGORY_CONTAMINATION`  
    → Remove or extract conflicting semantics into separate capabilities.

---

###### Core Capability Integrity

- `MISSING_CORE_CAPABILITY`  
    → Introduce a dedicated Core User Capability derived from relevant capability signals.
- `CORE_SUBSUMED`  
    → Extract the core user intent into a separate Core User Capability.
- `CORE_CONTAMINATION`  
    → Remove non-core elements (form, access, NFFF, supporting logic) from the Core capability.

---

###### NFFF Handling

- `NFFF_MISSING`  
    → Add the missing NFFF aspect to the classification table.
- `NFFF_DUPLICATED`  
    → Consolidate duplicate rows into a single entry.
- `NFFF_UNGROUNDED`  
    → Remove the NFFF aspect OR replace it with one supported by capability signals.
- `NFFF_NOT_PROMOTED`  
    → Introduce a dedicated capability for the NFFF aspect.
- `NFFF_EMBEDDED`  
    → Extract the NFFF aspect into a separate capability.

---

###### Capability Construction

- `INVALID_CAPABILITY_TYPE`  
    → Replace the capability with a user-recognizable capability OR remove it.
- `NO_USER_INTENT`  
    → Redefine the capability around a clear user-recognizable purpose.
- `INVALID_SCOPE_SIGNAL`  
    → Rewrite the scope signal to be boundary-oriented and non-procedural.

---

###### Splitting

- `SHOULD_SPLIT`  
    → Divide the capability into multiple capabilities based on:
    - user intent;
    - access/environment differences;
    - mental model separation.

---

###### Grouping

- `SHOULD_MERGE`  
    → Merge the capabilities into a single capability that:
    - represents a unified user-recognizable purpose;
    - preserves all valid capability signals.

---

###### Boundary Consistency

- `OVERLAPPING_CAPABILITIES`  
    → Adjust scope signals OR split capabilities to eliminate overlap.
- `REDUNDANT_CAPABILITIES`  
    → Merge or remove duplicate capabilities.
- `INCOHERENT_BOUNDARY`  
    → Re-scope or split capabilities to restore cohesion.

---

###### Boundary Decision

- `INVALID_KEEP_DECISION`  
    → Replace `Keep` with the correct action (`Split`, `Merge`, or `Revise`) and apply the corresponding revision.

---

##### Revision Priority Rules

When multiple validation failures are present, the LLM MUST resolve them in the following strict priority order:

1. **Signal Grounding**
    Resolve:
    - `UNSUPPORTED_CAPABILITY`
    - `UNMAPPED_SIGNAL`
2. **Capability Model Classification**
    Resolve:
    - `UNCLASSIFIED_CAPABILITY`
    - `MULTI_CATEGORY_CAPABILITY`
    - `CATEGORY_CONTAMINATION`
3. **Core User Capability Integrity**
    Resolve:
    - `MISSING_CORE_CAPABILITY`
    - `CORE_SUBSUMED`
    - `CORE_CONTAMINATION`
4. **NFFF Classification and Promotion**
    Resolve:
    - `NFFF_MISSING`
    - `NFFF_DUPLICATED`
    - `NFFF_UNGROUNDED`
    - `NFFF_NOT_PROMOTED`
    - `NFFF_EMBEDDED`
5. **Capability Construction Integrity**
    Resolve:
    - `INVALID_CAPABILITY_TYPE`
    - `NO_USER_INTENT`
    - `INVALID_SCOPE_SIGNAL`
6. **Splitting and Grouping**
    Resolve:
    - `SHOULD_SPLIT`
    - `SHOULD_MERGE`
7. **Boundary Consistency**
    Resolve:
    - `OVERLAPPING_CAPABILITIES`
    - `REDUNDANT_CAPABILITIES`
    - `INCOHERENT_BOUNDARY`
8. **Boundary Decision**
    Resolve:
    - `INVALID_KEEP_DECISION`

---

###### Priority Enforcement Rules

The LLM MUST:

- resolve all failures at the current priority level before proceeding to the next level;
- restart the **Validation Algorithm** after applying any revision;
- NOT attempt to resolve lower-priority failures before higher-priority failures are cleared.

---

###### Rationale (Non-Normative)

Higher-priority failures affect the validity of lower-level reasoning:

- incorrect grounding invalidates classification;
- incorrect classification invalidates grouping;
- incorrect grouping invalidates boundary evaluation.

Therefore, resolving failures out of order produces unstable or incorrect capability sets.

---

### Completion Condition

Validation is complete only when:

- no failures are detected in any step;
- all Boundary Decisions are `Keep`;
- the final result is marked `VALID`.
