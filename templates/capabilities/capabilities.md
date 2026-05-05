---
url: https://chatgpt.com/c/69f5cb12-6d14-83eb-ab10-a57b41b1aa71
---

## Capability Decomposition

Capability decomposition is an early analysis activity that converts a target description into a compact, user-centric map of the target scope.

A central component of capability decomposition is **capability classification**, which provides a **structural reasoning model** that the LLM MUST use to:

- distinguish primary user intent from supporting behavior and system form;
- guide identification and scoping of capabilities;
- prevent conflation of domain logic, supporting functionality, and form-factor concerns;
- steer boundary decisions during capability grouping and splitting.

This classification is also foundational for downstream analysis, but its primary role is to **shape decomposition itself**.

The LLM MUST decompose the target scope into a set of high-level user-centric capability anchors.

The **target description** is the input text or contextual material provided for analysis.

A **target scope** is the described product, system, feature area, extension, change, or project evolution being analyzed. It MAY represent a complete new system or a bounded change to an existing system.

A **capability anchor** is a coarse functional area that reflects something an end user would recognize, intentionally use, access, rely on, or care about. Capability anchors create a concise user-centric map of the target scope. They identify the major kinds of value, access, behavior, or experience described or strongly implied by the target description.

As part of capability decomposition, the LLM MUST identify each explicit or strongly implied capability and classify it according to the **Capability Model**.

A capability is classified based on whether its semantics imply interaction with conceptual system state and its relation to primary user intent, not on its interface, presentation, or implementation form.

Each capability MUST be assigned to exactly one of:

- **Core User Capability** — defines primary user intent and core state semantics.
- **Supporting Functional Capability** — affects, governs, validates, or transforms core state and provides functionality required to make the core user capability usable, complete, or coherent.
- **Non-Functional and Form-Factor (NFFF) Aspect** — defines user access, interaction form, environment, or experience without affecting core state.

---

### Protocol

The LLM MUST execute this module in order:

1. Interpret the target description and identify the target scope.
2. Perform capability decomposition as a **coupled activity**:
    - identify explicit or strongly implied capabilities;
    - classify each capability according to the **Capability Model**;
    - construct candidate capability anchors by grouping compatible capabilities.
    
    During this step, the LLM MUST:
    1) Identify the **Core User Capability** or capabilities.
    2) For each additional (non-core) capability, determine its dominant semantic role based on whether its primary purpose is::
        - If it implies mutation, validation, control, recovery, or interpretation of core state → **Supporting Functional Capability**.
        - Otherwise → **Non-Functional and Form-Factor (NFFF) Aspect**.
    3) Apply classification **during grouping**, ensuring:
        - each capability is assigned to exactly one category;
        - no capability anchor mixes categories.
    4) Classify and promote NFFF Aspects according to **NFFF Promotion Requirements**.
    5) Produce the **Non-Functional and Form-Factor Aspect Classification** table.
3. Apply the **Core Capability Test** and **Grouping vs. Splitting Test** to every candidate capability anchor.
4. Perform candidate set **Validation** and revise it until all validation checks pass.
5. Return the Capability Decomposition Report according to **Report Template**.

If a material ambiguity prevents valid report output, the LLM MUST ask a targeted clarification question instead of returning the Capability Decomposition Report.

---

### Purpose

The capability anchor set MUST establish a compact, user-centric, and structurally coherent representation of the target scope.

The LLM MUST produce a capability anchor set that:

- captures the **primary user intent** through dedicated Core User Capability anchors;
- includes all **supporting functional capabilities** required to make the core capability usable, correct, and coherent;
- represents all **Non-Functional and Form-Factor (NFFF) aspects** that meet promotion requirements;
- preserves clear separation between capability categories defined by the Capability Model;
- reflects how users **access, use, and experience** the system, not how it is implemented.

The capability anchor set MUST make explicit:

- major user-recognizable capability areas;
- the user value or functional intent of each capability anchor;
- natural capability boundaries implied by:
    - user intent;
    - interaction patterns;
    - access and environment differences;
- usability, access, launch, delivery, environment, and runtime concerns that materially affect user experience.

The capability anchor set MUST ensure that:

- combining anchors would obscure distinct user intent or experience;
- splitting anchors would not produce low-value or overly narrow capability fragments;
- all meaningful capabilities from the target description are represented without redundancy or conflation.

---

### Capability Model

#### Core User Capability

##### Definition and Role

Represents primary user intent and the fundamental transformation or use of conceptual system state required to fulfill it.

A Core User Capability:

* anchors *why the system exists*;
* defines domain intent and core state semantics;
* drives decomposition of domain behavior.

State semantics and transformations define the conceptual domain model, not its implementation.

The target description may or may not constrain domain modeling. During capability decomposition, **state MUST remain conceptual** and MUST be used only to:

* identify what constitutes core user capability; and
* distinguish Supporting Functional Capabilities from NFFF Aspects.

---

##### Identification Rules

The LLM MUST identify the core user capability or capabilities represented by the target scope before constructing the capability anchor set.

A core user capability is a primary user-recognizable need or job-to-be-done satisfied by the target scope, independent of:

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

The capability anchor set MUST include one or more dedicated capability anchors representing each core user capability, without fragmentation of the core intent.

The LLM MUST NOT allow:

* a domain-specific solution form;
* an interaction convention;
* a technology choice;
* a packaging approach;
* a delivery context;

to subsume or obscure the core user capability.

---

##### Core Capability Test

For each proposed capability anchor, the LLM MUST ask:

> Is this anchor defined around the user need being satisfied, or around a specific way of satisfying it?

If the anchor is named or scoped in terms of a specific solution form, interaction model, technology, access context, or delivery mechanism, the LLM MUST verify that:

* the underlying core user capability is represented by a separate dedicated anchor; or
* no distinct core user capability is being hidden.

If a core user capability is hidden inside a solution-form anchor, the LLM MUST split or revise the anchor set.

---


#### Supporting Functional Capability

##### Definition and Role

Represents core-state-affecting, core-state-governing, or core-state-focused functionality required to make the core user capability usable, correct, and coherent.

A Supporting Functional Capability:

* MAY affect user-visible behavior, but only through core-state interaction, not through form, access, or environment;
* ensures observability, correctness, completeness, and recoverability;
* remains within the same user mental model;
* operates on or governs core state;
* includes control, validation, and recovery logic;
* MUST NOT introduce a new form, access mode, or environment.

---

##### Classification Rules

A Supporting Functional Capability MUST NOT be classified as a NFFF Aspect solely because it has a user-visible interface or interaction surface.

The LLM MUST classify based on capability semantics, not surface form:

* **Supporting Functional Capability** — the capability's primary purpose is to operate on, validate, control, recover, or interpret system data or execution, even if it has a user interface.
* **NFFF Aspect** — the capability's primary purpose is to define how the user accesses, interacts with, or experiences the system, and it does not operate on core-state semantics.

When both appear present, the LLM MUST classify based on the dominant semantic role.

---

#### Non-Functional and Form-Factor Aspects (NFFF Aspects)

##### Definition and Role

Represents a distinct user-facing form, access path, interface modality, runtime environment, or operational experience whose semantics do not operate on core state.

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

##### NFFF Evaluation Pipeline

The LLM MUST perform the following steps for NFFF aspects:

1. **Identification**
    The LLM MUST inspect all **NFFF Aspect Taxonomy** categories and identify every explicit or strongly implied:
    - NFFF aspect;
    - alternative of the same NFFF aspect.
    
2. **Classification**
    For each identified NFFF aspect or alternative, the LLM MUST classify it on two axes:
    - **User-facing relevance**
        - **Capability-relevant aspect** — materially affects user-visible value or experience.
        - **Cross-cutting constraint** — constrains capability anchors without forming a distinct user-recognizable capability area.
        - **Not user-facing or not materially relevant** — does not materially affect capability decomposition.
    - **Implementation separability**
        - **Implementation workstream** — implies separable build, packaging, integration, deployment, or delivery work.
        - **Implementation constraint** — constrains implementation without implying a separable workstream.
        - **No distinct implementation implication** — does not materially affect implementation structure.
    
3. **Promotion**
    A NFFF aspect or alternative MUST become a dedicated capability anchor when it is classified as:
    - `Capability-relevant aspect`; or
    - both `Cross-cutting constraint` and `Implementation workstream`.
    A NFFF aspect or alternative MUST NOT be absorbed into a core user capability anchor when it meets either condition.
    
4. **Separation Constraints**
    The LLM MUST evaluate independently:
    - each distinct NFFF aspect;
    - each alternative of the same aspect.

    The LLM MUST NOT collapse distinct NFFF aspects or alternatives into a single generic capability anchor merely because they:
    - belong to the same taxonomy category;
    - support the same core user capability; or
    - share implementation components.
    
    The LLM MUST prefer explicit NFFF capability anchors over embedding NFFF concerns inside domain capability anchors.
    
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

Capability anchors are formed by grouping or splitting classified capabilities into cohesive, user-recognizable areas.

The LLM MUST determine capability boundaries using the following ordered evaluation factors:

1. **User-recognizable intent** — whether users perceive the behaviors as serving the same purpose.
2. **User mental model** — whether users would naturally categorize the behaviors together.
3. **User-facing experience** — whether behaviors are used and experienced as part of the same activity.
4. **Domain cohesion** — whether behaviors belong to the same conceptual capability area.
5. **Access or environment distinction** — whether behaviors imply different access, launch, or runtime contexts.
6. **Coverage clarity** — whether grouping improves or obscures understanding of system capabilities.

For each proposed capability anchor, the LLM MUST ask:

> Would a typical target user reasonably expect the included behaviors to belong together as one recognizable capability area?

The LLM MUST test whether the capability anchor differs materially from adjacent or related capability anchors in:

- user intent;
- user mental model;
- access or discoverability;
- interaction frequency;
- required expertise;
- environment or runtime context;
- baseline versus advanced use.

If one or more material differences exist inside a proposed capability anchor, the LLM SHOULD split the anchor unless doing so would create low-value, overly narrow anchors.

If a proposed capability anchor is too narrow to represent a meaningful user-facing capability area, the LLM SHOULD merge it with the nearest cohesive anchor.

---

##### Mandatory Constraints

The LLM MUST NOT:

- group capabilities that belong to different **Capability Model categories**:
    - Core User Capability
    - Supporting Functional Capability
    - NFFF Aspect (when promoted)
- merge a **Core User Capability** with:
    - Supporting Functional Capabilities; or
    - NFFF Capability Anchors
- absorb a promoted **NFFF Aspect** into a domain capability anchor

Each capability anchor MUST remain:

- category-consistent;
- semantically cohesive;
- user-recognizable as a distinct capability area.

---

##### Splitting Guidance

The LLM SHOULD split capability anchors when:

- users would perceive the behaviors as different reasons to use the system;
- behaviors imply materially different user intents;
- behaviors belong to different mental models or expertise levels;
- behaviors have different discoverability or usage patterns;
- behaviors imply different access, environment, or runtime expectations;
- one represents baseline use and another represents an extension;
- grouping would obscure a meaningful boundary;
- grouping would create a vague umbrella that restates the target scope.

---

##### Grouping Guidance

The LLM SHOULD group capabilities when:

- behaviors serve the same user-recognizable purpose;
- users would naturally access and use them as one capability area;
- separation would mirror implementation structure rather than user value;
- separation would create overly narrow or low-value anchors;
- differences are variations within the same broader user intent.

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

### Capability Anchor Construction

Capability anchors are constructed as part of the decomposition process by identifying and grouping capabilities while applying classification constraints.

Capability identification, classification, and grouping are **interdependent activities**:

- capabilities are identified from the target description;
- classification according to the Capability Model is applied as capabilities are inferred;
- capability anchors are formed by grouping compatible capabilities into cohesive areas.

The LLM MUST use classification to **govern anchor construction**, not as a separate pre- or post-processing step.

---

#### Construction Principles

Each capability anchor MUST:

- use a concise, user-facing name;
- express a clear end-user value or functional intent;
- include a `Scope signal` that defines what belongs within the capability boundary;
- group capabilities that are semantically cohesive and category-consistent under the Capability Model;
- reflect user-recognizable functionality, not implementation structure.

The LLM MUST derive anchors from:

- explicitly stated behavior;
- strongly implied user-facing behavior;
- usability, access, launch, delivery, environment, and runtime concerns;
- structural completeness expectations implied by the target scope.

---

#### Scope Signal Requirements

The `Scope signal` MUST:

- be brief and boundary-oriented;
- clarify inclusion boundaries without enumerating behaviors.

The `Scope signal` MUST NOT contain:

- implementation steps;
- validation or acceptance logic;
- task sequencing;
- exhaustive behavior lists.

---

#### Coverage and Structure Requirements

The capability anchor set MUST:

- cover all meaningful user-facing capabilities described or strongly implied by the target description;
- include dedicated anchors for Core User Capabilities;
- include dedicated anchors for NFFF Aspects that meet promotion requirements;
- preserve clear separation between capability categories;
- represent cohesive, inspectable capability areas;
- translate system concerns into user-centric terms of access, usage, and experience;
- balance granularity:
    - not fragmented into low-level actions;
    - not collapsed into overly broad umbrellas.

---

#### Prohibited Constructions

The LLM MUST NOT:

- construct anchors independently of classification;
- restate the entire target scope as a single capability;
- allow solution forms, technologies, or delivery contexts to subsume core user intent;
- collapse distinct capabilities due to shared implementation;
- split capabilities into isolated low-level actions;
- include internal mechanisms unless they directly affect user-visible behavior or experience;
- include implementation plans, validation scenarios, or task-level detail.

---

#### Construction Objective

The resulting capability anchor set MUST form a:

- compact;
- user-centric;
- structurally consistent;

representation of the target scope suitable for boundary evaluation and downstream analysis.

---

### Report Template

The LLM MUST return only the following output structure:

```markdown
## Capability Anchors

- **[Capability Name]** — [End-user value / functional intent].  
  Scope signal: [Brief statement of what kinds of behavior, access, experience, or environment concern this capability includes].

- **[Capability Name]** — [End-user value / functional intent].  
  Scope signal: [Brief statement of what kinds of behavior, access, experience, or environment concern this capability includes].

### Non-Functional and Form-Factor Aspect Classification

| Aspect | Taxonomy Category | User-Facing Relevance | Implementation Separability |
| ------ | ----------------- | --------------------- | ---------------------------- |
| [Explicit or strongly implied aspect] | [One or more taxonomy categories] | [Classification] | [Classification] |

### Capability Anchor Validation Results

- [Status] [Validation Checklist item]
- [Status] [Validation Checklist item]

#### Capability Boundary Test Result

| Capability Anchor | Capability Source Assessment | Grouping/Splitting Assessment | Boundary Decision | Justification |
| ----------------- | ---------------------------- | ----------------------------- | ----------------- | ------------- |
| [Capability Name] | [Assessment of whether the anchor represents a core user capability or a classified NFFF aspect/alternative, and whether any core user capability or promoted NFFF aspect is hidden] | [Assessment of whether the anchor is properly scoped] | Keep | [Brief justification based on user intent, mental model, access/discoverability, expertise, environment, or coverage clarity] |

Result: VALID
```

The items in **Capability Anchor Validation Results** MUST correspond exactly, in order and content, to the items defined in the **Validation Checklist** section.  
  
The LLM MUST:  
  
- evaluate each checklist item from the **Validation Checklist**;  
- render each `[Validation Checklist item]` here with its evaluated `[Status]` (`✅` or `❌`);  
- NOT introduce new items;  
- NOT omit any items;  
- NOT rephrase checklist items.

---

### Validation

The LLM MUST apply validation checks defined in **Validation Checklist**, **Core Capability Test**, and **Grouping vs. Splitting Test** to the candidate capability anchor set and extracted aspect classification.

During execution of the `Core Capability Test` and `Grouping vs. Splitting Test`, the `Boundary Decision` MUST use one of the following values:

- `Keep` — the capability anchor is valid for the final report.
- `Split` — the capability anchor is too broad and MUST be decomposed into separate anchors.
- `Merge` — the capability anchor is too narrow and MUST be combined with another anchor.
- `Revise` — the capability anchor name, value statement, or scope signal MUST be corrected.

If any validation item fails, or if any `Boundary Decision` is `Split`, `Merge`, or `Revise`, the LLM MUST revise the capability anchor set and rerun validation before returning the output.

Validation results MUST be presented according to **Report Template**. Boundary test results MUST be presented as the `Capability Boundary Test Result` table of the template.

The LLM MUST return only an output where:

- every checklist item is marked `✅`;
- every `Boundary Decision` is `Keep`;
- `Result` is `Valid`.

---

#### Validation Checklist

- (✅/❌) Target-description coverage checked.
- (✅/❌) Core user capabilities are represented by dedicated capability anchors.
- (✅/❌) Non-functional and form-factor (NFFF) aspects were classified per NFFF Classification Rules before inclusion.
- (✅/❌) Core user capabilities are not subsumed by solution forms or NFFF aspects.
- (✅/❌) Every explicit or strongly implied NFFF aspect or alternative appears exactly once in the classification table.
- (✅/❌) The classification table does not include aspects or alternatives that are neither explicit nor strongly implied by the target description.
- (✅/❌) NFFF aspects and alternatives that meet NFFF Promotion Requirements are represented as separate capability anchors and are not absorbed into core user capability anchors.
- (✅/❌) Every capability anchor corresponds either to a core user capability or to a classified NFFF aspect or alternative.
- (✅/❌) No capability anchor is merely an implementation mechanism.
- (✅/❌) No unrelated capabilities are grouped without justification.
- (✅/❌) No capability anchor is split into isolated low-level actions.
- (✅/❌) Grouping vs. Splitting Test applied to every capability anchor.
- (✅/❌) Capability boundaries are clear, inspectable, and non-overlapping.

---

### Completion Criteria

This analysis is complete only when:

- a capability anchor set has been produced;  
- the **Validation Checklist** is fully satisfied;  
- the validation result is `Valid`.
