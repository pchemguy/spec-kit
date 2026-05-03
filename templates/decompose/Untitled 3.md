I need taxonomy of key non-functional aspects, such as platform, portability, packaging/delivery, ui, lib vs app, what else? Each of these, in turn, may have various alternative, such as browser based web app vs desktop packaged, ui could include cli, tui, gui (and each may in turn have alternatives). The objective is to make more explicit related instructions (later).


#### Core User Capability

**Definition**

Represents primary user intent and defines the core state and its transformations.

**Role**

* anchors *why the system exists*
* defines domain intent and core state semantics
* drives decomposition of domain behavior

**Important**

Sate semantics and its transformations are key implementation details belonging to data/domain modeling. Target description may or may not provide any desired constrains on domain modeling. Throughout the capability decomposition process, target **state** should generally remain abstract/conceptual and primarily used for identifying supporting functional capabilities as distinct from NFFF aspects.

---

#### 2. NFFF Capability Anchors (Form / Access / Experience Layer)

**Definition**
Represents a distinct user-facing form, access path, interface modality, runtime environment, or operational experience that does **not affect or govern core state**.

**Role**

* defines *how the user accesses, interacts with, or experiences the system*
* represents form, environment, interface, delivery, and operational context
* is orthogonal to domain logic and core-state behavior

**Important properties**

* may be parallel (e.g., CLI vs GUI vs Web)
* may represent alternatives (each becomes its own anchor)
* may introduce independent implementation workstreams
* MUST NOT alter, validate, or govern core state

**Examples**

* “Use calculator via browser-based GUI”
* “Use calculator via CLI interface”
* “Run calculator as portable desktop application”

---

#### 3. Supporting Functional Capability

**Definition**

Represents core-state-affecting, core-state-governing, or core-state-focused functionality required to make the core user capability usable, correct, and coherent.

**Role**

* ensures observability, correctness, completeness, and recoverability
* remains within the same user mental model

**Important properties**

* operates on or governs core state
* includes control, validation, and recovery logic
* MUST NOT introduce a new form, access mode, or environment

**Examples**

* “Enter and edit numeric operands”
* “Manage stack state”
* “Apply arithmetic operations”
* “Validate and reject invalid operations”
* “Undo and reset state”

---

#### The Separation Rule (Critical)

Every capability anchor MUST fall into exactly one of:

| Type                             | Decided by                              |
| -------------------------------- | --------------------------------------- |
| Core User Capability             | primary user intent and state semantics |
| NFFF Capability                  | no interaction with core state          |
| Supporting Functional Capability | affects or governs core state           |

---

#### Decision Logic (What bucket does something go into?)

##### Step 1 — Does it define primary user intent and core state?

→ YES → **Core User Capability**

---

##### Step 2 — Does it affect or govern core state?

→ YES → **Supporting Functional Capability**

---

##### Step 3 — Otherwise

→ **NFFF Capability Anchor**

---
