---
url: https://chatgpt.com/c/69f5cb12-6d14-83eb-ab10-a57b41b1aa71
---

## Capability Decomposition

Capability decomposition is an early analysis activity that converts a **target description** into a compact, user-centric map of the **target scope**.

This analysis begins by extracting **capability signals** from the target description. Capability signals are then used to infer, classify, and refine **capability candidates** according to **Capability Construction** rules and **Capability Model** classification. Capability signals are also used to provide grounding and traceability for the final reported capabilities. After validation of the capability candidates, the final **capability set** is reported.

---
### Core Concepts

The **target description** is the input text or contextual material provided for analysis.

A **target scope** is the product, system, feature area, extension, change, or project evolution inferred from the target description. It MAY represent a complete new system or a bounded change to an existing system.

A **capability** is a coarse functional or form-factor area that reflects something an end user would recognize, intentionally use, access, rely on, or care about. Capabilities form a concise, user-centric map of the target scope and identify the major kinds of value, access, behavior, or experience described or strongly implied by the target description.

A **capability signal** is an exact keyword, term, or short phrase from the target description that provides source evidence for identifying, classifying, or scoping a capability.

The LLM MUST:

- use capability signals as the grounding basis for capability identification and classification;
- ensure that every capability is fully supported by one or more capability signals;
- infer capability semantics from the meaning and immediate context of associated capability signals.

The LLM MUST NOT expand capability semantics beyond what is reasonably supported by the associated capability signals and their immediate context.

A **capability candidate** is a provisional capability inferred from one or more capability signals during decomposition. Capability candidates are identified, classified, grouped, split, revised, or rejected before the final capability set is returned.

**Capability Model classification** is a central component of capability decomposition. It provides a structural reasoning model that the LLM MUST use to:

- distinguish primary user intent from supporting behavior and system form;
- guide identification and scoping of capabilities;
- prevent conflation of domain logic, supporting functionality, and form-factor concerns;
- steer boundary decisions during capability scoping.

The Capability Model classifies capabilities by their relation to primary user intent and by whether their semantics imply interaction with conceptual system state. It MUST NOT classify capabilities based on interface, presentation, or implementation form.

Each capability MUST be assigned exactly one Capability Model class:

- **Core User Capability** — defines primary user intent and core state semantics.
- **Supporting Functional Capability** — affects, governs, validates, or transforms core state and provides functionality required to make the core user capability usable, complete, or coherent.
- **Non-Functional and Form-Factor (NFFF) Aspect** — defines user access, interaction form, environment, or experience without affecting core state.

Capability Model classification is foundational for downstream analysis, but its primary role is to shape decomposition itself.

---

### Protocol

The LLM MUST execute this module in the following order:

1. Interpret the target description and identify the target scope.
2. Extract capability signals from the target description.
    The LLM MUST identify exact keywords, terms, and short phrases that:
    - indicate:
        - what user-recognizable functionality, experience, or jobs-to-be-done the target scope provides;
        - how users access, interact with, or experience the target scope;
        - what supporting or governing behavior is required to make the target scope usable, correct, or coherent;
    - provide evidence for:
        - Core User Capabilities;
        - Supporting Functional Capabilities;
        - Non-Functional and Form-Factor (NFFF) Aspects;
        - capability boundaries;
        - user access, interaction, environment, runtime, or delivery characteristics.
3. Generate the capability set.
    1. infer capability candidates from capability signals, ensuring:
        - capability candidates are inferred as user-recognizable capability areas rather than implementation details, internal components, or low-level actions;
        - every capability signal is assigned to the single most appropriate capability candidate unless the signal genuinely supports multiple distinct capability semantics;
        - every capability candidate is associated with one or more capability signals;
    2. identify Core User Capability candidates;
    3. determine the dominant semantic role of each non-core capability candidate:
        - Supporting Functional Capability — if the candidate primarily implies mutation, validation, control, recovery, or interpretation of core state;
        - NFFF Aspect — otherwise;
    4. classify capability candidates according to the **Capability Model**, ensuring:
        - each capability belongs to exactly one Capability Model class;
        - no capability mixes class semantics;
    5. evaluate and promote NFFF Aspects according to the **NFFF Evaluation Pipeline**;
    6. group, split, revise, or reject capability candidates to construct the final capability set, while maintaining capability signal grounding and traceability throughout refinement, ensuring that capability signal assignments:
        - are updated whenever capability candidates are revised and remain preserved throughout refinement;
        - remain semantically consistent with the current capability candidate definition and scope;
        - support traceability from every final reported capability and classification to one or more capability signals.
4. Validate the capability set and NFFF aspect classification according to the **Validation** section.
5. Return the final **Capability Decomposition Report** according to the **Report Template**, including:
    - the completed **Capabilities** section containing the final capability set;
    - the completed **Non-Functional and Form-Factor Aspect Classification** table.

If a material ambiguity prevents valid output, the LLM MUST ask a targeted clarification question instead of returning the report.

---

### Capability Model

#### Core User Capability

##### Definition and Role

Represents primary user intent and the fundamental use or transformation of conceptual system state required to fulfill it.

A Core User Capability:

* anchors *why the system exists*;
* defines domain intent and core state semantics;
* drives decomposition of domain behavior.

State semantics and transformations define the conceptual domain model, not its implementation.

During capability decomposition, state MUST remain conceptual and MUST be used only to:

* identify what constitutes a Core User Capability; and
* distinguish Supporting Functional Capabilities from NFFF Aspects.

---

##### Identification Rules

The LLM MUST identify the Core User Capability or capabilities represented by the target scope before constructing the capability set.

A Core User Capability is a primary user-recognizable need or job-to-be-done satisfied by the target scope, independent of:

* domain form;
* interaction model;
* architectural approach;
* delivery mechanism;
* implementation solution.

When the target description expresses a specific solution form, the LLM MUST distinguish:

* the user need being satisfied;
* the domain or interaction form through which the need is satisfied;
* supporting correction, recovery, visibility, access, or environment capabilities;
* delivery or runtime concerns that affect user access or experience.

---

##### Constraints

The capability set MUST include one or more capabilities representing each Core User Capability, without fragmentation of the core intent.

The LLM MUST NOT allow:

* a domain-specific solution form;
* an interaction convention;
* a technology choice;
* a packaging approach;
* a delivery context;

to subsume or obscure the Core User Capability.

---

##### Core Capability Test

For each proposed capability, the LLM MUST ask:

> Is this capability defined around the user need being satisfied, or around a specific way of satisfying it?

If a capability is named or scoped in terms of a specific solution form, interaction model, technology, access context, or delivery mechanism, the LLM MUST verify that:

* the underlying Core User Capability is represented by a separate capability; or
* no distinct Core User Capability is being hidden.

If a Core User Capability is hidden inside a solution-form capability, the LLM MUST split or revise the capability set.

---

#### Supporting Functional Capability

##### Definition and Role

Represents functionality that affects, governs, validates, interprets, controls, recovers, or transforms conceptual core state in order to make the Core User Capability usable, correct, and coherent.

A Supporting Functional Capability:

* MAY affect user-visible behavior, but only through interaction with core state;
* ensures observability, correctness, completeness, and recoverability;
* remains within the same user mental model as the Core User Capability;
* operates on or governs core state;
* includes control, validation, interpretation, and recovery logic;
* MUST NOT introduce a distinct form, access mode, interface modality, runtime environment, or operational experience.

---

##### Classification Rules

A Supporting Functional Capability MUST NOT be classified as a NFFF Aspect solely because it has a user-visible interface or interaction surface.

The LLM MUST classify based on capability semantics, not surface form:

* **Supporting Functional Capability** — the capability's primary purpose is to operate on, validate, control, recover, interpret, or transform system data, execution, or conceptual core state, even if it has a user-visible interface.
* **NFFF Aspect** — the capability's primary purpose is to define how the user accesses, interacts with, or experiences the system, and it does not operate on core-state semantics.

When both appear present, the LLM MUST classify based on the dominant semantic role.

---

##### Constraints

A Supporting Functional Capability MUST:

- remain semantically subordinate to at least one Core User Capability;
- operate on or govern conceptual core state;
- remain within the same broader user mental model as the associated Core User Capability.

A Supporting Functional Capability MUST NOT:

- define a distinct user-facing form, runtime environment, or delivery context;
- subsume a Core User Capability;
- absorb a promotable NFFF Aspect.

---

#### Non-Functional and Form-Factor Aspects (NFFF Aspects)

##### Definition and Role

Represents a distinct user-facing form, access path, interface modality, runtime environment, or operational experience whose semantics do not operate on conceptual core state.

An NFFF Aspect:

* defines *how the user accesses, interacts with, or experiences the system*;
* represents form, environment, interface, delivery, and operational context;
* is orthogonal to domain logic and core-state behavior.

An NFFF Aspect MUST NOT:

- mutate the core state;
- define core-state transition rules;
- participate in domain evaluation semantics;
- govern acceptance, rejection, or transformation of core data.

All such behavior belongs to Supporting Functional Capabilities.

---

##### Identification Requirement

The LLM MUST explicitly identify all NFFF aspects and their alternatives from the target description using **capability signals**.

Identification MUST be grounded in:

- explicit statements in the target description; or
- strongly implied user-facing access, interaction, environment, or delivery characteristics.

The LLM MUST:

- inspect all **NFFF Aspect Taxonomy** categories;
- identify each distinct NFFF aspect;
- identify each explicitly stated or strongly implied alternative of the same aspect.

The LLM MUST NOT:

- invent NFFF aspects not supported by capability signals;
- omit any aspect that is explicit or strongly implied.

---

##### NFFF Evaluation Pipeline

The LLM MUST perform the following steps for NFFF aspects:

1. **Identification**  
    Identify every explicit or strongly implied:
    - NFFF aspect;
    - alternative of the same NFFF aspect.
    
2. **Classification**  
    For each identified NFFF aspect or alternative, classify it on two axes:
    
    - **User-facing relevance**
        - **Capability-relevant aspect** — materially affects user-visible value or experience.
        - **Cross-cutting constraint** — constrains capabilities without forming a distinct user-recognizable capability area.
        - **Not user-facing or not materially relevant** — does not materially affect capability decomposition.
    - **Implementation separability**
        - **Implementation workstream** — implies separable build, packaging, integration, deployment, or delivery work.
        - **Implementation constraint** — constrains implementation without implying a separable workstream.
        - **No distinct implementation implication** — does not materially affect implementation structure.
    
3. **Promotion**  
    A NFFF aspect or alternative MUST become a dedicated capability when it is classified as:
    
    - `Capability-relevant aspect`; or
    - both `Cross-cutting constraint` and `Implementation workstream`.
    
    A NFFF aspect or alternative MUST NOT be absorbed into a Core User Capability or Supporting Functional Capability when it meets either condition.
    
4. **Separation Constraints**  
    The LLM MUST evaluate independently:
    
    - each distinct NFFF aspect;
    - each alternative of the same aspect.
    
    The LLM MUST NOT collapse distinct NFFF aspects or alternatives into a single generic capability merely because they:
    
    - belong to the same taxonomy category;
    - support the same core user capability;
    - share implementation components.
    
    The LLM MUST prefer explicit NFFF capabilities over embedding NFFF concerns inside domain capabilities.
    
5. **Classification Table Requirements**  
    Every identified NFFF aspect or alternative MUST appear exactly once in the **Non-Functional and Form-Factor Aspect Classification** table.
    
    Each `Taxonomy Category` cell MAY contain multiple categories when applicable.
    
    The table MUST NOT include aspects or alternatives that are neither explicit nor strongly implied by the target description.
    
    If no NFFF aspects are identified, the table MUST contain one row with:
    
    - `Aspect`: `None identified`
    - all other columns: `N/A`

---

##### NFFF Aspect Taxonomy

1. **Product Form** — application, library, service, tool, extension, workflow, configuration artifact.
2. **Runtime Platform** — browser, desktop, mobile, terminal, server, embedded, cloud, local.
3. **Access Model** — direct launch, integrated access, programmatic access, automated access.
4. **User Interface Modality** — GUI, CLI, TUI, conversational UI, voice UI, API, no direct UI.
5. **Interaction Style** — form-based, command-driven, menu-driven, editor-like, dashboard, direct manipulation, batch, real-time.
6. **Packaging and Delivery** — hosted, installable, portable, package-manager, source-based, containerized, plugin/extension bundle.
7. **Portability** — cross-platform runtime, portable execution, data portability, configuration portability.
8. **Deployment and Hosting** — local-only, SaaS, self-hosted, on-premises, cloud, hybrid, air-gapped, multi-tenant.
9. **Connectivity and Offline Behavior** — online-only, offline-first, offline-capable, sync, degraded mode, local-only.
10. **Persistence and State** — ephemeral, local persistence, remote persistence, hybrid sync, user-managed files, autosave/versioned state.
11. **Identity and Access Control** — no auth, local identity, account login, SSO, roles, permissions, API keys.
12. **Integration Surface** — files, clipboard, URLs, APIs, webhooks, SDKs, plugins, OS/browser/IDE integration.
13. **Execution Mode** — interactive, batch, streaming, scheduled, event-driven, background, asynchronous.
14. **Reliability and Recovery** — undo, redo, retry, rollback, autosave, crash recovery, backup/restore, safe failure.
15. **Observability and Feedback** — status, progress, warnings, errors, logs, audit trails, notifications, previews.
16. **Performance and Resource Constraints** — latency, throughput, startup time, large-data handling, memory/CPU/battery limits.
17. **Accessibility and Internationalization** — keyboard access, screen reader support, contrast, localization, locale/timezone behavior.
18. **Configuration and Customization** — preferences, profiles, themes, shortcuts, config files, policies, presets.
19. **Privacy and Data Ownership** — local-only data, cloud data, exportability, encryption, retention, third-party processing.
20. **Target Audience and Operational Ownership** — consumer, power user, developer, admin, operator, organization, public/internal/enterprise.

---

#### Capability Boundary and Cohesion

##### Boundary and Grouping Rules

Capabilities are formed by grouping or splitting capability candidates into cohesive, user-recognizable areas.

The LLM MUST determine capability boundaries using the following ordered evaluation factors:

1. **User-recognizable intent** — whether users perceive the behaviors as serving the same purpose.
2. **User mental model** — whether users would naturally categorize the behaviors together.
3. **User-facing experience** — whether behaviors are used and experienced as part of the same activity.
4. **Domain cohesion** — whether behaviors belong to the same conceptual capability area.
5. **Access or environment distinction** — whether behaviors imply different access, launch, or runtime contexts.
6. **Coverage clarity** — whether grouping improves or obscures understanding of system capabilities.

---

##### Mandatory Constraints

The LLM MUST enforce Capability Model consistency during grouping of capability candidates:

- capability candidates belonging to different Capability Model classes MUST NOT be grouped:
    - Core User Capability
    - Supporting Functional Capability
    - NFFF Aspect (when promoted)
- a **Core User Capability** MUST NOT be merged with:
    - Supporting Functional Capability candidates; or
    - NFFF capability candidates
- a promoted **NFFF Aspect** MUST NOT be absorbed into a domain capability

Each resulting capability MUST remain:

- category-consistent;
- semantically cohesive;
- user-recognizable as a distinct capability area.

---

##### Boundary Test

For each proposed capability, the LLM MUST ask:

> Would a typical target user reasonably expect the included behaviors to belong together as one recognizable capability area?

The LLM MUST evaluate whether the proposed capability differs materially from adjacent or related capabilities in:

- user intent;
- user mental model;
- access or discoverability;
- interaction frequency;
- required expertise;
- environment or runtime context;
- baseline versus advanced use.

During boundary evaluation, each capability MUST receive one `Boundary Decision`:

- `Keep` — the capability is valid for the final output.
- `Split` — the capability is too broad and MUST be decomposed into separate capabilities.
- `Merge` — the capability is too narrow and MUST be combined with another capability.
- `Revise` — the capability name, value statement, or scope boundary MUST be corrected.

If one or more material differences exist within a proposed capability, the LLM SHOULD assign `Split` unless doing so would produce low-value or overly narrow capabilities.

If a proposed capability is too narrow to represent a meaningful user-facing area, the LLM SHOULD assign `Merge`.

If the capability name, value statement, or scope boundary is inaccurate or unclear, the LLM SHOULD assign `Revise`.

The LLM MUST assign `Keep` only when no `Split`, `Merge`, or `Revise` condition applies.

---

##### Splitting Guidance

The LLM SHOULD split a capability candidate when the resulting fragmented capabilities would:

- serve different user-recognizable purposes;
- imply materially different user intents, access, environment, or runtime expectations;
- be understood by users through different mental models or expertise levels;
- be discovered, accessed, or used by users in materially different ways;
- separate baseline use from a meaningful extension of use;
- clarify an otherwise obscured capability boundary;
- replace a vague umbrella candidate with clearer, more cohesive separate capabilities.

---

##### Grouping Guidance

The LLM SHOULD group capability candidates when the grouped capability would:

- serve one user-recognizable purpose;
- support the same broader user intent;
- be naturally accessed and used by users as one capability area;
- treat differences among candidates as variations within the same broader use case;
- avoid mirroring implementation structure instead of user value;
- avoid artificial capability boundaries;
- avoid overly narrow or low-value capabilities.

---

##### Boundary Evaluation Principle

Capability boundaries are determined by:

- user expectations;
- interaction patterns;
- discoverability;
- cognitive grouping;
- capability-level cohesion;

NOT by formal domain taxonomy or implementation structure alone.

The LLM MAY use established product conventions within the target domain as supporting evidence.

---

### Capability Construction

Capabilities are constructed as part of capability decomposition by identifying capability candidates, applying classification constraints, and grouping or splitting candidates into cohesive user-recognizable areas.

Capability identification, classification, and grouping are interdependent activities:

- capability candidates are inferred from capability signals extracted from the target description;
- classification according to the Capability Model is applied as candidates are inferred and scoped;
- capabilities are formed by grouping, splitting, revising, or rejecting capability candidates.

The LLM MUST use classification to govern capability construction, not as a separate pre- or post-processing step.

---

#### Capability Signal Constraints

Capability construction MUST be grounded in capability signals extracted from the target description.

Capability signals MUST:

- use verbatim text where possible, with minimal normalization;
- directly support capability identification, classification, or scoping decisions;
- provide evidence used during grouping and splitting evaluation;
- remain concise and non-redundant.

Capability signals MUST NOT:

- introduce inferred terminology not grounded in the target description;
- restate full capability descriptions;
- include explanations, interpretations, or sentences;
- combine unrelated source concepts.

The LLM MUST NOT construct capability semantics that exceed what is reasonably supported by the associated capability signals and their immediate context.

---

#### Construction Principles

Each capability MUST:

- use a concise, user-facing name;
- express a clear end-user value or functional intent;
- include a `Scope boundary` that defines what belongs within the capability boundary;
- be semantically cohesive and category-consistent under the Capability Model;
- reflect user-recognizable value, behavior, access, or experience, not implementation structure.

The LLM MUST construct capabilities from:

- capability signals in the target description;
- explicitly stated behavior;
- strongly implied user-facing behavior;
- usability, access, launch, delivery, environment, and runtime concerns;
- structural completeness expectations implied by the target scope.

---

#### Scope Boundary Requirements

The `Scope boundary` MUST:

- be brief and boundary-oriented;
- clarify inclusion boundaries without enumerating behaviors.

The `Scope boundary` MUST NOT contain:

- implementation steps;
- validation or acceptance logic;
- task sequencing;
- exhaustive behavior lists.

---

#### Capability Set Requirements

The capability set MUST establish a compact, user-centric, and structurally coherent representation of the target scope.

The capability set MUST:

- capture primary user intent through dedicated Core User Capabilities;
- include Supporting Functional Capabilities required to make the core capability usable, correct, and coherent;
- represent NFFF Aspects that meet promotion requirements;
- preserve clear separation between Capability Model classes;
- reflect how users access, use, and experience the system, not how it is implemented;
- make explicit the user value or functional intent of each capability;
- expose natural capability boundaries implied by user intent, interaction patterns, access differences, and environment differences;
- represent usability, access, launch, delivery, environment, and runtime concerns that materially affect user experience;
- represent all meaningful capabilities from the target description without redundancy or conflation.

The capability set MUST NOT:

- combine capabilities when doing so would obscure distinct user intent or experience;
- split capabilities when doing so would produce low-value or overly narrow fragments.
 ---
 
#### Coverage and Structure Requirements

The capability set MUST:

- cover all meaningful capabilities described or strongly implied by the target description;
- include dedicated capabilities for Core User Capabilities;
- include dedicated capabilities for NFFF Aspects that meet promotion requirements;
- preserve clear separation between Capability Model classes;
- represent cohesive, inspectable capability areas;
- translate system concerns into user-centric terms of access, usage, and experience;
- balance granularity:
    - not fragmented into low-level actions;
    - not collapsed into overly broad umbrellas.

---

#### Prohibited Constructions

The LLM MUST NOT:

- construct capabilities independently of classification;
- restate the entire target scope as a single capability;
- allow solution forms, technologies, or delivery contexts to subsume core user intent;
- collapse distinct capabilities due to shared implementation;
- split capabilities into isolated low-level actions;
- include internal mechanisms unless they directly affect user-visible behavior or experience;
- include implementation plans, validation scenarios, or task-level detail.

---

#### Construction Objective

The resulting capability set MUST form a:

- compact;
- user-centric;
- structurally consistent;

representation of the target scope suitable for boundary evaluation and downstream analysis.

---

### Report Template

The LLM MUST return only the following output structure:

```markdown
## Capability Decomposition Report

### Capabilities

- **[Capability Name]** — [End-user value / functional intent].  
  Scope boundary: [Brief boundary-oriented scope statement].  
  Classification: [Capability Model Class].  
  Extracted Keywords: [One or more capability signals].

- **[Capability Name]** — [End-user value / functional intent].  
  Scope boundary: [Brief boundary-oriented scope statement].  
  Classification: [Capability Model Class].  
  Extracted Keywords: [One or more capability signals].

### Non-Functional and Form-Factor Aspect Classification

| Aspect | Taxonomy Category | User-Facing Relevance | Implementation Separability |
| ------- | ----------------- | --------------------- | --------------------------- |
| [Explicit or strongly implied aspect] | [One or more taxonomy categories] | [Classification] | [Classification] |
```

- Any `Extracted Keywords` field MUST contain one or more capability signals.  
- `Extracted Keywords` fields provide grounding and traceability between reported items and the target description.  
- Capability signal usage and interpretation MUST remain semantically consistent across all outputs.

---

### Validation

The LLM MUST validate the candidate capability set and extracted NFFF aspect classification using the **Validation Algorithm**.

If the Validation Algorithm detects any failure, the LLM MUST:

1. apply the applicable Revision Strategy;
2. revise the capability set;
3. restart validation from Step 1.

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
- Every table row corresponds to an NFFF aspect capability grounded in ≥1 capability signal.
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
    - a coherent scope boundary.

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
    → Rewrite the scope boundary to be boundary-oriented and non-procedural.

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
    → Adjust scope boundaries OR split capabilities to eliminate overlap.
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

---
