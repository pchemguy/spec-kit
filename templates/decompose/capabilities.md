---
url: https://chatgpt.com/c/69f5cb12-6d14-83eb-ab10-a57b41b1aa71
---

## Capability Decomposition

The LLM MUST decompose the target scope into a set of high-level user-centric capability anchors.

The **target description** is the input text or contextual material provided for analysis.

A **target scope** is the described product, system, feature area, extension, change, or project evolution being analyzed. It MAY represent a complete new system or a bounded change to an existing system.

A **capability anchor** is a coarse functional area that reflects something an end user would recognize, intentionally use, access, rely on, or care about.

Capability anchors create a concise user-centric map of the target scope. They identify the major kinds of value, access, behavior, or experience described or strongly implied by the target description.

---

### Protocol

The LLM MUST execute this module in order:

1. Interpret the target description and identify the target scope.
2. Identify the core user capability or capabilities represented by the target scope according to **Core User Capability**.
3. Inspect the **Non-Functional and Form-Factor Aspect Taxonomy** and extract every explicit or strongly implied **Non-Functional and Form-Factor Aspect** from the target description.
4. Classify each extracted aspect by:
    - **Taxonomy Category** — one or more matching categories from the taxonomy;
    - **User-Facing Relevance** — `Capability-relevant aspect` / `Cross-cutting constraint` / `Not user-facing or not materially relevant`;
    - **Implementation Separability** — `Implementation workstream` / `Implementation constraint` / `No distinct implementation implication`.
5. Produce the **Non-Functional and Form-Factor Aspect Classification** table.
6. Decompose the target scope into a candidate capability anchor set based on the **Rules**.
7. Apply the **Core Capability Test** and **Grouping vs. Splitting Test** to every candidate capability anchor.
8. Perform candidate set **Validation** and revise it until all validation checks pass.
9. Return the Capability Decomposition Report according to **Capability Decomposition Report Template**.

If a material ambiguity prevents valid report output, the LLM MUST ask a targeted clarification question instead of returning the Capability Decomposition Report.

---

### Purpose

The capability anchor set MUST establish a compact, user-facing representation of the target scope.

The LLM MUST identify:

- major user-recognizable capability areas;
- the user value or functional intent of each capability;
- specified or strongly implied usability, access, launch, delivery, environment, or runtime concerns;
- natural capability boundaries indicated by the target description;
- capability areas that are distinct enough that combining them would obscure user intent.

---

### Rules

#### Core User Capability

The LLM MUST identify the core user capability or capabilities represented by the target scope before naming capability anchors.

A **core user capability** is a primary user-recognizable need or job-to-be-done that the target scope satisfies, independent of the specific domain form, interaction model, architectural approach, delivery mechanism, or implementation solution used to satisfy it.

When the target description expresses a specific solution form, the LLM MUST distinguish:

- the user need being satisfied;
- the domain or interaction form through which the need is satisfied;
- supporting correction, recovery, visibility, access, or environment capabilities;
- delivery or runtime concerns that affect user access or experience.

The capability anchor set MUST include one or more dedicated capability anchors for core user capabilities included in the target description. The LLM MUST NOT allow a domain-specific solution form, interaction convention, technology choice, packaging approach, or delivery context to subsume the core user capability. 

**Core Capability Test**

For each proposed capability anchor, the LLM MUST ask:

> Is this anchor defined around the user need being satisfied, or around a specific way of satisfying it?

If the anchor is named/scoped for a specific solution form, interaction model, technology, access context, or delivery mechanism, the LLM MUST verify that the underlying core user capability is represented by a separate dedicated anchor or that no distinct core user capability is being hidden.

If a core user capability is hidden inside a solution-form anchor, the LLM MUST split or revise the anchor set.

---

#### Decomposition

The LLM MUST derive each capability anchor, including its capability name and `Scope signal`, from:

- the target description and explicitly stated behavior;
- strongly implied user-facing behavior;
- specified usability, access, launch, delivery, environment, or runtime requirements;
- structural completeness expectations implied by the kind of target scope being described.

Each capability anchor MUST:

- use a concise user-facing name;
- state the end-user value or functional intent in the anchor summary;
- include a `Scope signal` that briefly identifies what belongs under the capability;
- avoid implementation-first naming unless the target requirement is itself about user access, launch, delivery, runtime, or environment experience;
- avoid low-level action names when a broader user-recognizable capability name is available.

The `Scope signal` MUST be brief and boundary-oriented.

The `Scope signal` MUST NOT contain:

- implementation steps;
- validation scenarios;
- acceptance criteria;
- task details;
- sequencing;
- exhaustive behavior lists.

The capability anchor set MUST:

- cover all meaningful user-facing capabilities described or strongly implied by the target description;
- cover specified or strongly implied usability, access, launch, delivery, environment, and runtime aspects;
- include dedicated anchors for core user capabilities that represent the primary user needs or jobs-to-be-done in the target scope;
- distinguish core user capabilities from domain forms, interaction models, supporting behaviors, access contexts, and delivery mechanisms;
- describe cohesive user-facing capability areas;
- translate architectural, deployment, or delivery requirements into user-centric terms based on how the user accesses, launches, uses, or experiences the target scope;
- prefer end-user value and functional intent over implementation structure;
- remain coarse enough to avoid becoming a list of isolated actions;
- remain specific enough to make capability coverage and boundaries inspectable.

The capability anchor set MUST NOT:

- copy the target description as a single broad capability;
- let a domain-specific solution form or interaction model absorb the underlying user need it serves;
- hide primary user needs inside broad umbrella anchors named after a solution form, technology choice, access context, or delivery mechanism;
- collapse unrelated capabilities merely because they were mentioned together;
- split capabilities into isolated operations or low-level actions;
- group potentially related capabilities when the target description indicates distinct user intent, access mode, environment, or delivery expectation;
- describe internal mechanisms unless they directly affect user-visible access, behavior, or experience;
- include implementation plans, validation scenarios, acceptance criteria, task lists, or sequencing.

---

#### Non-Functional and Form-Factor Aspects (NFFF Aspects)

The LLM MUST inspect all **Non-Functional and Form-Factor Aspect Taxonomy** categories before deciding whether any such aspect belongs in the capability anchor set.

For each explicit or strongly implied non-functional or form-factor aspect, the LLM MUST classify it on two axes:

1. **User-facing relevance**
    - **Capability-relevant aspect** — the aspect materially changes user-visible value, access, control, recovery, trust, portability, environment, ownership, or experience.
    - **Cross-cutting constraint** — the aspect constrains one or more capability anchors but is not itself a distinct user-recognizable capability area.
    - **Not user-facing or not materially relevant** — the aspect does not materially affect capability decomposition.
2. **Implementation separability**
    - **Implementation workstream** — the aspect implies separable build, packaging, integration, deployment, or delivery work.
    - **Implementation constraint** — the aspect constrains implementation choices, compatibility, validation, or quality expectations but does not imply a separable workstream.
    - **No distinct implementation implication** — the aspect does not materially affect implementation structure.

A NFFF aspect MUST become a dedicated capability anchor when it is classified as `Capability-relevant aspect`.

A NFFF aspect SHOULD become a dedicated capability anchor when it is classified as both:

- `Cross-cutting constraint`; and
- `Implementation workstream`.

A NFFF aspect MUST NOT be absorbed into a core user capability anchor when it materially affects product form, access, launch, delivery, interface modality, interaction style, runtime environment, portability, persistence, identity, integration, recovery, observability, privacy, ownership, or operational use.

The LLM MUST prefer explicit NFFF aspect capability anchors over hiding NFFF aspect concerns inside broad domain capability anchors.

---

##### Classification Rules

Every extracted Non-Functional and Form-Factor Aspect MUST appear exactly once in the **Non-Functional and Form-Factor Aspect Classification** table.

Each `Taxonomy Category` cell MAY contain multiple categories when the aspect spans more than one category.

The classification table MUST NOT include aspects that are neither explicit nor strongly implied by the target description.

If no explicit or strongly implied Non-Functional and Form-Factor Aspects are found, the classification table MUST contain one row with `None identified` in the `Aspect` column and `N/A` in the remaining columns.

---

##### Non-Functional and Form-Factor Aspect Taxonomy

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

#### Capability Boundary

When deciding whether to split or group capability anchors, the LLM MUST evaluate the following factors in order:

1. **User-recognizable intent** — whether users would understand the behaviors as serving the same purpose.
2. **User mental model** — whether users would naturally categorize the behaviors together when deciding what they want to do.
3. **User-facing experience** — whether the behaviors are used, accessed, discovered, or experienced as part of the same broad activity.
4. **Domain cohesion** — whether the behaviors belong to the same conceptual capability area.
5. **Access or environment distinction** — whether the target scope indicates materially different access, launch, delivery, runtime, or environment expectations.
6. **Coverage clarity** — whether the anchor makes the target scope easier to inspect for omitted, overloaded, or conflated capability areas.

The LLM SHOULD split capability anchors when:

- users would perceive the areas as different reasons to use the target scope;
- behaviors imply materially different user intents, even if they are domain-related;
- behaviors belong to different user mental models or expertise levels;
- behaviors have materially different discoverability, access, or usage-frequency expectations;
- the target scope signals distinct access, launch, delivery, environment, or runtime expectations;
- one capability represents baseline use and another represents a materially different extension of use;
- grouping would hide a meaningful capability boundary;
- grouping would create a vague umbrella that merely restates the target scope.

The LLM SHOULD group capability anchors when:

- the behaviors serve the same user-recognizable purpose;
- users would naturally look for, learn, access, or use the behaviors as one capability area;
- separation would merely mirror implementation structure or formal domain taxonomy;
- separation would create anchors that are too narrow to represent meaningful user-facing capability areas;
- the distinction is only a variation within the same broader user intent.

The separation is not based only on formal domain taxonomy. It is based on user expectations, interaction patterns, discoverability, cognitive category, and capability-level cohesion.

The LLM MAY use common product conventions for the target domain as evidence when evaluating user mental model, discoverability, and capability boundaries.

---

### Grouping vs. Splitting Test

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

### Capability Decomposition Report Template

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

### Capability Anchor Validation Result

- ✅ Target-description coverage checked.
- ✅ Core user capabilities are represented by dedicated capability anchors.  
- ✅ Domain forms, interaction models, technology choices, access contexts, packaging approaches, and delivery contexts do not subsume core user capabilities.  
- ✅ Non-functional and form-factor aspects were classified before inclusion.  
- ✅ Every explicit or strongly implied non-functional and form-factor aspect appears exactly once in the classification table.  
- ✅ The classification table excludes aspects that are neither explicit nor strongly implied by the target description.  
- ✅ Cross-cutting constraints are not represented as standalone capability anchors unless they define distinct user-facing value.  
- ✅ Implementation workstreams are not represented as standalone capability anchors unless they create distinct user-facing access, interaction, operating, control, recovery, visibility, trust, ownership, or experience value.  
- ✅ User-facing capability areas checked.  
- ✅ Usability, access, launch, delivery, environment, and runtime aspects checked where applicable.  
- ✅ No capability anchor merely restates the full target scope.  
- ✅ No capability anchor is merely an implementation mechanism.  
- ✅ No unrelated capabilities are grouped without justification.  
- ✅ No capability anchor is split into isolated low-level actions.  
- ✅ Grouping vs. Splitting Test applied to every capability anchor.  
- ✅ Capability boundaries are clear, inspectable, and non-overlapping.

#### Capability Boundary Test Result

| Capability Anchor | Core Capability Assessment | Grouping/Splitting Assessment | Boundary Decision | Justification |
| ----------------- | -------------------------- | ----------------------------- | ----------------- | ------------- |
| [Capability Name] | [Assessment of whether the anchor represents a user need or a solution form, and whether any core capability is hidden] | [Assessment of whether the anchor is properly scoped] | Keep | [Brief justification based on user intent, mental model, access/discoverability, expertise, environment, or coverage clarity] |

Result: Valid
```

The LLM MUST NOT include any section not shown in the Capability Decomposition Report Template.

---

### Validation

The LLM MUST apply validation checks defined in `Capability Anchor Validation Result`, `Non-Functional and Form-Factor Aspect Classification`, `Core Capability Test`, and `Grouping vs. Splitting Test` to the candidate capability anchor set and extracted aspect classification.

During execution of the `Core Capability Test` and `Grouping vs. Splitting Test`, the `Boundary Decision` MUST use one of the following values:

- `Keep` — the capability anchor is valid for the final report.
- `Split` — the capability anchor is too broad and MUST be decomposed into separate anchors.
- `Merge` — the capability anchor is too narrow and MUST be combined with another anchor.
- `Revise` — the capability anchor name, value statement, or scope signal MUST be corrected.

If any validation item fails, or if any `Boundary Decision` is `Split`, `Merge`, or `Revise`, the LLM MUST revise the capability anchor set and rerun validation before returning the output.

Validation results MUST be presented according to **Capability Decomposition Report Template**. Boundary test results MUST be presented as the `Capability Boundary Test Result` table of the template.

The LLM MUST return only an output where:

- every checklist item is marked `✅`;
- every `Boundary Decision` is `Keep`;
- `Result` is `Valid`.

---

### Completion Criteria

This analysis is complete only when:

- a capability anchor set has been produced;
- all meaningful user-facing capabilities from the target description are represented;
- core user capabilities are represented by dedicated capability anchors;
- every explicit or strongly implied non-functional and form-factor aspect appears exactly once in the classification table;
- the classification table excludes aspects that are neither explicit nor strongly implied by the target description;
- domain forms, interaction models, technology choices, access contexts, packaging approaches, and delivery contexts do not subsume core user capabilities;
- specified or strongly implied usability, access, launch, delivery, environment, and runtime aspects are represented where applicable;
- every capability anchor is user-centric and functionally cohesive;
- no capability anchor merely restates the full target scope;
- no capability anchor is merely an implementation mechanism;
- no capability anchor is split into isolated low-level actions;
- capability boundaries are clear, inspectable, and non-overlapping;
- the validation result is passing.
