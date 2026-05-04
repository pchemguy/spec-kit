### Capability Model

A capability is classified based on whether its semantics imply interaction with conceptual system state and its relation to primary user intent, not on its interface, presentation, or implementation form.

---

#### 1. Capability Classification

Every explicit or strongly implied capability MUST be classified as exactly one of:

* **Core User Capability** — defines primary user intent and the fundamental transformation or use of conceptual system state.
* **Supporting Functional Capability** — affects, governs, validates, or transforms core state and makes the core capability usable, correct, or coherent.
* **Non-Functional and Form-Factor (NFFF) Aspect** — defines user access, interaction form, environment, or experience without interacting with core state semantics.

---

#### 2. Classification Rules

The LLM MUST classify based on **semantic role**, not surface form.

##### Core User Capability

Represents the primary user-recognizable job-to-be-done.

* anchors *why the system exists*
* defines domain intent and conceptual state transformation

State remains **abstract and conceptual** and is used only to distinguish capability types.

---

##### Supporting Functional Capability

A capability MUST be classified as Supporting Functional if its semantics include:

* mutation of system state
* validation or rejection logic
* control of execution or flow
* recovery or reversal
* interpretation of system data

Important constraints:

* MAY be user-visible, but only through state interaction
* MUST NOT introduce a new access mode, interface modality, or runtime environment

---

##### Non-Functional and Form-Factor Aspect

A capability MUST be classified as NFFF if:

* its semantics do **not include mutation, validation, control, recovery, or interpretation of core state**; and
* its purpose is to define how the user accesses, interacts with, or experiences the system

Examples of semantic scope:

* interface modality
* runtime platform
* packaging and delivery
* operational experience

---

##### Conflict Resolution Rule

When a capability appears to satisfy multiple categories:

* classification MUST be based on **dominant semantic role**
* presence of UI or interaction surface MUST NOT cause misclassification as NFFF

---

#### 3. NFFF Promotion Rules

Each NFFF aspect or explicitly stated or strongly implied alternative MUST be evaluated independently.

A NFFF aspect (or alternative) MUST become a **dedicated capability anchor** when it is classified as:

* `Capability-relevant aspect`; or
* both `Cross-cutting constraint` and `Implementation workstream`

A NFFF aspect MUST NOT be absorbed into a core or supporting capability when the above conditions hold.

The LLM MUST NOT collapse distinct NFFF aspects or alternatives into a single anchor when they materially differ in user-visible experience.

---

#### 4. NFFF Classification Requirements

For each NFFF aspect or alternative, the LLM MUST produce classification along:

* **Taxonomy Category**
* **User-Facing Relevance**
* **Implementation Separability**

Every identified aspect MUST appear exactly once in the classification table.

---

#### 5. Capability Anchor Construction Rules

Capability anchors MUST be constructed from classified capabilities.

Each anchor MUST:

* represent a user-recognizable capability area
* express end-user value or intent
* include a concise `Scope signal` defining its boundary

---

##### Anchor Inclusion Rules

The capability anchor set MUST include:

* all Core User Capabilities
* all Supporting Functional Capabilities required for completeness
* all NFFF aspects that meet promotion rules

---

##### Anchor Exclusion Rules

The capability anchor set MUST NOT:

* collapse distinct NFFF aspects or alternatives
* represent implementation mechanisms as standalone anchors
* mix multiple capability categories in a single anchor
* restate the full target scope as one capability

---

##### Anchor Boundary Rules

The LLM MUST determine anchor boundaries based on:

1. user-recognizable intent
2. user mental model
3. interaction and usage patterns
4. environment or access differences
5. clarity of capability coverage

---

##### Grouping vs. Splitting

The LLM SHOULD:

* **split** when user intent or experience materially differs
* **group** when behavior serves the same user-recognizable purpose

---
