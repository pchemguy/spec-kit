---
urls:
  - https://chatgpt.com/c/69f5cb12-6d14-83eb-ab10-a57b41b1aa71
  - https://chatgpt.com/c/69fc9b6e-eb4c-8390-8d38-7f1b836e7d56
---

## Capability Decomposition

### Core Concepts

Capability decomposition is an early analysis activity that converts a **target description** into a compact, user-centric map of the **target scope**. This analysis begins by extracting **capability signals** from the target description. Capability signals are then used to infer, classify, and refine capability candidates according to the **Capability Construction Rules** and **Capability Model**. Capability signals are also used to provide grounding and traceability for the final reported capabilities. After validation of the capability candidates, the final **capability set** is reported.

The **target description** is the input text or contextual material provided for analysis.

A **target scope** is the product, system, feature area, extension, change, or project evolution inferred from the target description. It MAY represent a complete new system or a bounded change to an existing system.

A **capability** is a coarse functional or form-factor area that reflects something an end user would recognize, intentionally use, access, rely on, or care about. Capabilities form a concise, user-centric map of the target scope and identify the major kinds of value, access, behavior, or experience described or strongly implied by the target description.

A **capability signal** is an exact keyword, term, or short phrase from the target description that provides source evidence for identifying, classifying, or scoping a capability.

A **capability candidate** is a provisional capability inferred from one or more capability signals during decomposition. Capability candidates are identified, classified, grouped, split, revised, or rejected before the final capability set is returned.

---

### Procedure

The LLM MUST execute this module in the following order, applying the **Capability Model** and **Capability Construction Rules** throughout:

1. Interpret the target description and identify the target scope.
2. Extract capability signals from the target description according to **Capability Signal Grounding**.
3. Generate and refine the capability set:  
    1. infer capability candidates from capability signals;  
    2. identify Core User Capability candidates;  
    3. determine the dominant semantic role of each non-core capability candidate;  
    4. classify capability candidates according to the Capability Model;  
    5. evaluate and promote NFFF Aspects according to the NFFF Evaluation Pipeline;  
    6. group, split, revise, or reject capability candidates to construct the final capability set;  
    7. maintain capability signal grounding and traceability throughout refinement.
4. Validate the capability decomposition according to the **Validation** section.
5. Generate the **Capability Decomposition Report** according to the **Report Template**.  
6. Validate the generated report according to **Report Output Validation**.  
7. Return only the validated final **Capability Decomposition Report**.

If a material ambiguity prevents valid output, the LLM MUST ask a targeted clarification question instead of returning the report.

---

### Capability Model

**Capability Model classification** is a central component of capability decomposition. It provides a structural reasoning model that the LLM MUST use to:

- distinguish primary user intent from supporting behavior and system form;
- guide identification and scoping of capabilities;
- prevent conflation of domain logic, supporting functionality, and form-factor concerns;
- steer boundary decisions during capability scoping.

The Capability Model classifies capabilities by their relation to primary user intent and by whether their semantics imply interaction with conceptual system state. It MUST NOT classify capabilities based on interface, presentation, or implementation form.

---

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
* distinguish Supporting Functional Capabilities from NFFF Capabilities.

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

A Supporting Functional Capability MUST NOT be classified as a NFFF Capability solely because it has a user-visible interface or interaction surface.

The LLM MUST classify based on capability semantics, not surface form:

* **Supporting Functional Capability** — the capability's primary purpose is to operate on, validate, control, recover, interpret, or transform system data, execution, or conceptual core state, even if it has a user-visible interface.
* **NFFF Capability** — the capability's primary purpose is to define how the user accesses, interacts with, or experiences the system, and it does not operate on core-state semantics.

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

#### Non-Functional and Form-Factor Capability (NFFF Capability)

##### Definition and Role

A **NFFF aspect** is a user-facing form, access path, interaction mode, interface modality, runtime environment, delivery characteristic, or operational experience identified during capability decomposition.

NFFF aspects do not operate on conceptual core state. They describe how the user accesses, interacts with, receives, runs, or experiences the target scope.

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

An **NFFF Capability** is a dedicated capability constructed from one or more promotable NFFF aspects according to the **NFFF Evaluation Pipeline**.

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

The LLM MUST perform the following steps for identified NFFF aspects:

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
    A identified NFFF aspect or alternative MUST become a dedicated NFFF Capability when it is classified as:
    
    - `Capability-relevant aspect`; or
    - both `Cross-cutting constraint` and `Implementation workstream`.
    
    A promotable NFFF aspect or alternative MUST NOT be absorbed into a Core User Capability or Supporting Functional Capability.
    
4. **Separation Constraints**  
    The LLM MUST evaluate independently:
    
    - each distinct NFFF aspect;
    - each alternative of the same aspect.
    
    The LLM MUST NOT collapse distinct NFFF aspects or alternatives into a single generic NFFF Capability merely because they:
    
    - belong to the same taxonomy category;
    - support the same core user capability;
    - share implementation components.
    
    The LLM MUST prefer explicit NFFF Capabilities over embedding NFFF concerns inside domain capabilities.
    
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

### Capability Construction Rules

Capabilities are constructed as part of capability decomposition by identifying capability candidates, applying classification constraints, and grouping or splitting candidates into cohesive user-recognizable areas.

Capability identification, classification, and grouping are interdependent activities:

- capability candidates are inferred from capability signals extracted from the target description;
- classification according to the Capability Model is applied as candidates are inferred and scoped;
- capabilities are formed by grouping, splitting, revising, or rejecting capability candidates.

The LLM MUST use classification to govern capability construction, not as a separate pre- or post-processing step.

---

#### Capability Signal Grounding

Capability construction MUST be grounded in capability signals extracted from the target description.

The LLM MUST:

- use capability signals as the grounding basis for capability identification and classification;
- ensure that every capability is grounded in one or more capability signals;
- infer capability semantics from the meaning and immediate context of associated capability signals.

Capability signals MUST:

- use verbatim text (exact keywords, terms, and short phrases) with minimal normalization, including:
    - word form normalization;
    - `[collective term] ([extracted keywords])`, if appropriate;
- directly support capability identification, classification, or scoping decisions;
- provide evidence used during grouping and splitting evaluation;
- indicate:
    - what user-recognizable functionality, experience, or jobs-to-be-done the target scope provides;
    - how users access, interact with, or experience the target scope;
    - what supporting or governing behavior is required to make the target scope usable, correct, or coherent;
- remain concise and non-redundant.

Capability signals MUST NOT:

- introduce inferred terminology not grounded in the target description;
- restate full capability descriptions;
- include explanations, interpretations, or sentences;
- combine unrelated source concepts.

The LLM MUST NOT expand capability semantics beyond what is reasonably supported by the associated capability signals and their immediate context.

---

#### Construction Principles

Each capability MUST:

- use a concise, user-facing name;
- express a clear end-user value or functional intent;
- include a `Scope boundary` that defines what belongs within the capability boundary;
- reflect user-recognizable value, behavior, access, or experience, not implementation structure.

The LLM MUST construct capabilities from:

- capability signals in the target description;
- explicitly stated behavior;
- strongly implied user-facing behavior;
- usability, access, launch, delivery, environment, and runtime concerns;
- structural completeness expectations implied by the target scope.

The LLM MUST NOT:

- allow solution forms, technologies, or delivery contexts to subsume core user intent;
- collapse distinct capabilities due to shared implementation;
- include internal mechanisms unless they directly affect user-visible behavior or experience;
- include implementation plans, validation scenarios, or task-level detail.

---

#### Boundary and Cohesion

Capability boundaries are determined by grouping or splitting capability candidates into cohesive, user-recognizable capability areas.

For each proposed capability, the LLM MUST ask:

> Would a typical target user reasonably expect the included behaviors to belong together as one recognizable capability area?

The LLM MUST evaluate capability boundaries using the following factors:

1. **User-recognizable intent** — whether the included behaviors serve the same user purpose.
2. **User mental model** — whether users would naturally categorize the behaviors together.
3. **User-facing experience** — whether behaviors are used or experienced as part of the same activity.
4. **Access and discoverability** — whether behaviors are accessed, discovered, or invoked in materially different ways.
5. **Environment or runtime context** — whether behaviors imply materially different access, launch, delivery, or runtime expectations.
6. **Usage profile** — whether behaviors differ materially in interaction frequency, required expertise, or baseline-versus-advanced use.
7. **Coverage clarity** — whether grouping improves or obscures understanding of the target scope.

The LLM SHOULD split a candidate capability when it:

- combines materially different user intents, mental models, or usage experiences;
- combines materially different access, environment, or runtime expectations;
- obscures meaningful extensions of use behind baseline functionality;
- lacks clear user-recognizable cohesion;
- forms a vague or overly broad umbrella capability.

The LLM SHOULD group multiple capability candidates into a single capability when the resulting capability would:

- preserve one coherent user-recognizable purpose;
- reflect one broader user intent;
- align with natural user interaction and discoverability patterns;
- avoid artificial fragmentation;
- avoid mirroring implementation structure instead of user value.

The LLM MUST maintain capability signal grounding and traceability throughout refinement, ensuring that capability signal associations:

- are updated whenever capability candidates are revised and remain preserved throughout refinement;
- remain semantically consistent with the current capability candidate or final capability definition and scope;
- support traceability from every final reported capability and classification to one or more capability signals.

The LLM MUST NOT:

- group capabilities when doing so would obscure distinct user intent, experience, access patterns, or environment distinctions;
- split capabilities when doing so would produce low-value, artificial, or overly narrow fragments.

Capability boundaries MUST be determined primarily by:

- user expectations;
- interaction patterns;
- discoverability;
- cognitive grouping;
- capability-level cohesion;

NOT primarily by formal domain taxonomy or implementation structure.

The LLM MUST enforce Capability Model consistency during capability construction:

- capability candidates belonging to different Capability Model classes MUST NOT be grouped;
- Core User Capabilities MUST NOT absorb Supporting Functional Capabilities or NFFF Capabilities;
- NFFF Capabilities MUST remain distinct from functional capabilities.

Each resulting capability MUST remain:

- semantically cohesive;
- category-consistent;
- user-recognizable as a distinct capability area.

---

#### Capability Set Integrity

The capability set MUST establish a compact, user-centric, and structurally coherent representation of the target scope.

The capability set MUST:

- cover all meaningful capabilities described or strongly implied by the target description;
- preserve coherent separation between Capability Model classes across the overall capability set;
- represent cohesive, non-overlapping, and inspectable capability areas;
- balance granularity across the capability set:
    - not fragmented into isolated low-level actions;
    - not collapsed into overly broad umbrella capabilities.

The capability set MUST NOT:

- omit meaningful user-recognizable capabilities strongly implied by the target description;
- contain redundant or conflated capabilities.

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

### Validation

The LLM MUST validate the candidate capability set against the declarative rules defined in this module.

Validation MUST proceed in the following order. If any validation step fails, the LLM MUST:

1. apply the correction defined for that step;
2. revise only the affected capability, capability boundary, or signal association where possible;
3. preserve unaffected valid decisions;
4. restart validation from Step 1.

The LLM MUST NOT generate the final report until all validation steps pass.

---

#### Step 1 — Signal Grounding and Traceability

Check that:

- every capability is grounded in one or more capability signals;
- every material capability signal used during decomposition is represented by, or traceably associated with, one or more capabilities;
- capability signal associations remain semantically consistent with the final capability names, classifications, and scope boundaries.

If validation fails:

- remove or revise any capability that is not grounded in capability signals;
- associate unmapped capability signals with the most appropriate existing capability when this preserves cohesion;
- introduce a new capability only when an unmapped signal represents a distinct user-recognizable capability that cannot be coherently absorbed;
- update stale or inconsistent signal associations to match the revised capability definition and scope.

---

#### Step 2 — Capability Model Classification

Check that:

- every capability is assigned exactly one Capability Model class;
- no capability mixes Core User Capability, Supporting Functional Capability, or NFFF Capability semantics;
- each classification reflects the capability’s dominant semantic role.

If validation fails:

- assign any unclassified capability to the correct class;
- split any mixed capability into separate category-consistent capabilities;
- revise any contaminated scope boundary so that each capability reflects one dominant semantic role.

---

#### Step 3 — Core User Capability Integrity

Check that:

- each identified Core User Capability is represented by one or more dedicated capabilities;
- no Core User Capability is obscured by solution form, interaction model, technology, access mode, delivery context, or supporting logic;
- no Core User Capability absorbs Supporting Functional Capability or NFFF Capability semantics.

If validation fails:

- introduce or revise a dedicated Core User Capability grounded in capability signals;
- extract supporting logic, form-factor concerns, access concerns, or delivery concerns into separate capabilities when required;
- revise the Core User Capability name and scope boundary around primary user intent.

---

#### Step 4 — NFFF Aspect Promotion

Check that:

- every promotable NFFF aspect is represented by a dedicated NFFF capability;
- no promoted NFFF Aspect is embedded inside a Core User Capability or Supporting Functional Capability.

If validation fails:

- promote qualifying NFFF aspects into dedicated capabilities;
- extract embedded promoted NFFF aspects from non-NFFF capabilities.

---

#### Step 5 — Capability Construction Integrity

Check that:

- every capability represents a user-recognizable capability area, not an implementation mechanism, internal component, task, processing step, validation scenario, or low-level action;
- every capability has one dominant user intent;
- every capability has a concise user-facing name;
- every `Scope boundary` is brief, boundary-oriented, and non-procedural.

If validation fails:

- remove or revise invalid capabilities into user-recognizable capability areas;
- rewrite names around end-user value or functional intent;
- rewrite scope boundaries to clarify inclusion boundaries without enumerating behaviors or implementation steps.

---

#### Step 6 — Splitting and Grouping

Check that:

- no capability combines materially different user intents, mental models, usage experiences, access patterns, environment or runtime contexts, expertise levels, or usage profiles;
- no pair of capabilities is artificially separated when they serve the same user-recognizable purpose and share the same access, environment, and runtime expectations;
- every capability is cohesive, non-redundant, and category-consistent.

If validation fails:

- split overloaded capabilities along the material difference that caused the failure;
- merge artificially separated capabilities when doing so preserves category consistency and user-recognizable cohesion;
- revise overlapping scope boundaries to eliminate redundancy or ambiguity;
- preserve all valid capability signals during any split or merge.

---

#### Step 7 — Final Validity Check

Check that:

- all capabilities satisfy the Capability Model;
- all promoted NFFF aspects are represented correctly;
- all capability boundaries are cohesive and non-overlapping;
- all signal associations remain valid after the final revision.

If validation fails:

- apply the smallest correction needed;
- restart validation from Step 1.

---

#### Completion Condition

Validation is complete only when:

- all validation steps pass;
- all capabilities satisfy the declarative rules defined in this module.

---

### Report Template

The LLM MUST return only the following output structure:

```markdown
## Capability Decomposition Report

### Capability List

- **[Capability Name]** — [End-user value / functional intent].
  Scope boundary: [Brief boundary-oriented scope statement].
  Classification: [Capability Model Class].
  Extracted Keywords: [One or more capability signals].

### Capability Table

| Capability | Classification | Scope boundary | Extracted Keywords |
| ---------- | -------------- | -------------- | ------------------ |
| [Capability Name] | [Capability Model Class] | [Brief boundary-oriented scope statement] | [One or more capability signals] |

### Non-Functional and Form-Factor Aspect Classification

| Aspect | Taxonomy Category | User-Facing Relevance | Implementation Separability | Extracted Keywords |
| ------ | ----------------- | --------------------- | --------------------------- | ------------------ |
| [Explicit or strongly implied aspect] | [One or more taxonomy categories] | [Classification] | [Classification] | [One or more capability signals] |
```

The LLM MUST validate generated output according to **Report Output Validation**;

---

#### Report Output Validation

Validates that the generated report completely and consistently materializes the validated capability set according to the Report Template.

Check that:

- **Capability List** and **Capability Table** include every capability included in the final capability set, including NFFF capabilities;
- Capability List and Capability Table contain consistent capability names, classifications, scope boundaries, and extracted keywords;
- every `Extracted Keywords` field contains one or more capability signals;
- capability signal usage and interpretation remain semantically consistent across all outputs;
- every explicit or strongly implied NFFF aspect or alternative appears exactly once in the NFFF classification table;
- the table contains the required `None identified` row when no NFFF aspects are identified;
- the final output conforms to the Report Template.

Resolve:

- add missing capability items;
- complete missing `Extracted Keywords` fields;
- update stale or inconsistent signal associations to match the revised capability definition and scope;
- add missing NFFF aspects or alternatives to the classification table;
- remove unsupported NFFF table rows;
- consolidate duplicated NFFF rows;
- revise final output to match the Report Template.

If validation fails:

- apply the smallest correction needed;
- restart report validation checks;
- repeat `validate -> correct` workflow until all checks pass.

Validation is complete only when:

- all report validation checks pass.

---
