<!--
Sync Impact Report
Version change: 1.7.0 -> 1.8.0
-->

# Project Constitution

## PREAMBLE: Project Context Initialization

This project follows a Specification-Driven Development paradigm and uses [GitHub Spec Kit](https://github.com/github/spec-kit/) as the primary framework for specification, planning, task decomposition, and agentic delivery.

The baseline project context is defined by the following documents:

- `constitution.md` — defines project invariants, workflow rules, and enforcement requirements.
- [`progress.md`](progress.md) — defines implemented feature history, reconstructed project state, and operational instructions.

The constitution MUST NOT be modified to reflect routine feature evolution. Such changes MUST be recorded in `progress.md` instead. Both documents MUST be loaded together for agent initialization.

### Mandatory Initialization Behavior

Any agent, tool, or contributor operating on this repository MUST:

1. Load and interpret `constitution.md` and `progress.md` before performing any analysis, planning, implementation, or review tasks.
2. Treat these documents as the authoritative source of:
    - `constitution.md`: project policies, constraints, and enforcement
    - `progress.md`: implemented system behavior and feature evolution
3. Use these documents to establish baseline understanding before consulting feature-level artifacts (specs, plans, tasks, or issues).

### Context Hierarchy

Project understanding MUST be constructed using the following precedence:

1. Constitution (`constitution.md`)
2. Project evolution context (`progress.md`)
3. Feature-level artifacts (specifications, plans, tasks, issues)
4. Conversation context

Conversation context MAY provide additional guidance, clarification, or intent, but MUST NOT override or contradict explicit information defined in repository artifacts.

### Bootstrap Behavior

- `progress.md` is a required project artifact and MUST exist alongside `constitution.md` within `.specify/memory/`.
- If `progress.md` is missing, the repository state MUST be treated as an invalid or incomplete.
- For greenfield or partially onboarded brownfield projects, `progress.md` MAY initially contain no feature entries.
- If `progress.md` exists but is inconsistent with repository artifacts, the inconsistency MUST be reported and resolved as part of the current work.

### Agent Onboarding Template  
  
The following block MAY be copied into agent prompts, AGENTS.md, or other execution environments to direct correct project context initialization.

```
<!-- AGENT_ONBOARDING_TEMPLATE_START -->  
## Agent Onboarding and Initialization Requirements

This project follows a Specification-Driven Development paradigm and uses [GitHub Spec Kit](https://github.com/github/spec-kit/) as the primary framework for specification, planning, task decomposition, and agentic delivery.

The baseline project context is defined by:

- [.specify/memory/constitution.md](.specify/memory/constitution.md)
- [.specify/memory/progress.md](.specify/memory/progress.md)

Before performing any work on this repository, agents MUST load and interpret these documents and treat them as the authoritative source of project policies, constraints, and implemented system state.
<!-- AGENT_ONBOARDING_TEMPLATE_END -->
```

## Core Principles

<!--
Note, in case of considerable changes, the principles should be order in a logical order. The current ordering is according to real workflow: context → spec → architecture → plan → implement → validate → document → record
-->

### I. Project Evolution Context Must Be Explicit And Machine-Readable

The repository MUST maintain a project-level context document alongside this `constitution.md` as:

- [progress.md](progress.md)

This document is the authoritative summary of implemented project evolution and feature history. It MUST:

- provide a chronological record of implemented features;
- map each feature to its specification and related delivery artifacts;
- summarize implemented behavior and scope at a level sufficient for agent context initialization; and
- enable deterministic reconstruction of project state without relying on conversation context;
- include operational instructions and a template for maintaining feature evolution records.

Every feature tracked through the governed agentic workflow MUST have exactly one canonical entry in `progress.md`. Existing feature entries MUST remain stable historical records except for factual corrections and allowed status transitions. `BROWNFIELD SUMMARY` entries MAY be used to establish baseline state when no prior governed history exists or when out-of-band changes must be reconciled.

Rationale: agentic workflows require a stable, compact, and explicit project state representation. Without a canonical evolution log, agents reconstruct context heuristically, leading to drift, inconsistency, and loss of traceability.

### II. Specification and Documentation are the Source of Truth

Implementation MUST be derived from explicit project artifacts rather than inferred from habit, preference, or undocumented assumptions. Specifications, plans, tasks, issue descriptions, and governing project documents MUST define:

- what is to be built,
- what is out of scope, and
- what evidence is required for completion.

When implementation, conversation context, and repository documents conflict, the approved project artifacts take precedence unless they are formally amended.

Rationale: this repository exists to support spec-driven and agent-driven development. That workflow fails when requirements are implicit, reconstructed ad hoc, or allowed to drift between artifacts.

### III. Architecture and Decomposition Must Preserve Separation of Concerns

System design and implementation MUST preserve clear separation of concerns at every relevant level of decomposition, including module boundaries, feature slices, service interfaces, data handling, runtime integration points, and cross-cutting concerns. Responsibilities SHOULD be assigned so that components remain understandable, independently testable, maintainable, and extensible without unnecessary coupling.

Feature-level decomposition MUST preserve separation of concerns at the product-delivery level. Accordingly, each feature MUST
deliver standalone, user-visible value and be minimal in scope, self-sufficient, and independently implementable and testable. A feature MUST NOT combine unrelated or merely convenient future work, and MUST NOT rely on later planned features to become meaningful.

Plans and implementation tasks MUST identify the intended decomposition for non-trivial changes and MUST avoid designs that mix unrelated responsibilities, hide boundaries, or force broad changes for localized behavior. Where a design introduces shared infrastructure, common abstractions, or reusable layers, that structure MUST be justified by immediate maintainability, testability, extensibility, or boundary-enforcement needs rather than speculative reuse or anticipated generality.

Rationale: strong separation of concerns reduces regression risk, improves local reasoning, enables more reliable testing, and allows the system to evolve in controlled increments.

### IV. Architecture and Environment Constraints Must be Respected

Implementation MUST preserve the repository's declared architectural boundaries, technology constraints, and development environment rules.

Accordingly:

- plans and tasks MUST name any platform, runtime, packaging, tooling, shell, or repository constraints that materially affect execution;
- agents and contributors MUST prefer the simplest approach compatible with the current architecture and supported environment, and MUST not introduce new dependencies, toolchains, or cross-boundary coupling without explicit justification; and
- repository-specific execution guidance takes precedence over contributor convenience defaults when shell, tooling, or environment choices affect correctness or determinism.

Rationale: agentic development is most reliable when architectural seams and execution constraints are explicit, stable, and enforced.

### V. Delivery Proceeds in Small, Testable Increments

Work MUST be decomposed into small, reviewable, and independently testable units.

To support that requirement:

- specifications MUST be structured so that user stories or feature slices can be implemented and validated incrementally;
- plans, task lists, and GitHub issues MUST avoid bundling unrelated work;
- implementation SHOULD prefer the smallest change that produces meaningful progress while preserving repository correctness.

Large or tightly coupled changes require explicit justification in the plan.

Rationale: small increments reduce agent error, simplify review, improve rollback safety, and enable more deterministic progress.

### VI. Test Suite Development is Inseparable from Code Development

Codebase development and test-suite development MUST proceed together. Every task that materially advances, modifies, or fixes production code MUST include the corresponding test work needed to verify the affected behavior, boundary, or regression risk. That test work MAY be performed immediately before the code change or immediately after it, but it MUST remain part of the same implementation unit and MUST not be deferred as optional follow-up work.

A feature is not complete when only the code path exists; completion requires the corresponding test path to exist as well.

The required test evidence MAY include unit, integration, contract, end-to-end, snapshot, replay, packaging, or other automated checks, depending on the affected boundary. Manual validation MAY supplement automated testing where appropriate, but it MUST not be used to justify leaving the automated test base behind when durable coverage is practical and warranted.

Tasks and implementation plans MUST identify the test development required for each code-advancing change, and reviewers MUST reject changes where production code evolves without a corresponding and proportionate evolution of the test suite.

Rationale: when tests evolve in lockstep with the codebase, the project preserves behavioral confidence, regression resistance, and change traceability. Treating test development as inseparable from code development prevents coverage debt from accumulating behind apparently complete feature work.

### VII. Scope, Decisions, and Exceptions Must be Traceable

Every non-trivial change MUST make its scope boundaries, design decisions, and exceptions explicit.

At minimum:

- specifications MUST distinguish required behavior from assumptions and non-goals;
- plans MUST identify material technical decisions, rejected alternatives when relevant, and any temporary deviations from the preferred approach; and
- exceptions MUST record the impacted principle, the business or technical reason, the owner, and the removal condition.

Rationale: traceability prevents scope creep, reduces hidden complexity, and makes agent-generated changes auditable and reviewable.

### VIII. Documentation is a First-Class, Verified Deliverable

Project documentation MUST be treated as a required, versioned, and validated deliverable of every feature, not as optional or post-hoc work.

Accordingly:

- plans MUST define required documentation artifacts, including:
    - user-facing documentation (e.g., README, usage instructions)
    - developer-facing documentation (e.g., DEVELOPMENT.md, architecture, extension points, development workflow)
    - supporting documentation (e.g., domain behavior, constraints, testing strategy, project evolution summary)
- task lists MUST include explicit documentation tasks that:
    - create or update all required documentation artifacts;
    - reflect the actual implemented system structure, behavior, and constraints; and
    - are sequenced after functional implementation but before final completion
- implementation MUST ensure that documentation:
    - is updated in the same feature scope as the code it describes;
    - does not contain speculative, outdated, or inferred behavior; and
    - is reviewed against the implemented code and test workflow and corrected for mismatches
- a feature MUST NOT be considered complete until:
    - all required documentation artifacts are present;
    - documentation is consistent with the implemented system; and
    - instructions for running, using, and extending the system are explicitly defined

Rationale: without enforced documentation, spec-driven development degrades into code-driven reconstruction. Verified documentation preserves system understanding, enables agent continuity, and prevents knowledge loss.

## Technology Platform

The approved technology platform for this repository is:

- **Project Type**: local desktop-packaged web application
- **Frontend**: React
- **Primary language**: TypeScript
- **Desktop packaging**: Electron
- **Persistence / backend storage**: SQLite
- **Architecture expectation**: browser-oriented application logic packaged for desktop use rather than a server-centric application

Implementation plans, tasks, issue generation, and code changes MUST preserve this platform definition unless it is explicitly amended. Agents and contributors MUST not substitute a different application model, language, desktop packaging approach, or persistence technology without a recorded decision and approval.

## Feature Roadmap

The repository MAY maintain a `roadmap.md` document alongside `constitution.md` and `progress.md`.

When present, `roadmap.md` MUST:

- define an ordered list of features, each beginning with a second-level heading `## [Feature Name]`;
- have unique feature names (`## [Feature Name]`) across the document;
- contain "### Specify User Prompt" subsection in each feature section that must represent a user prompt to be used with `/speckit.specify` without modification;
- record only features that satisfy the feature-level decomposition constraints defined by this constitution.

`/speckit.specify` MUST operate on exactly one feature.

That feature MUST be defined either:

- explicitly in the user prompt; or
- implicitly by selecting the earliest feature in `roadmap.md` having the `planned` status.

When `/speckit.specify` processes a feature from the `roadmap.md` file, the agent must include an additional metadata `Roadmap Feature: [Feature Name]`, where `[Feature Name]` must match exactly feature section title in `roadmap.md`.

When both an explicit feature definition and `roadmap.md` are provided, `/speckit.specify` MUST NOT proceed without explicit user confirmation of the selected feature.

When using feature from `roadmap.md`, 

`roadmap.md` represents planned intent and MUST NOT be treated as implemented state.

## Spec Kit Workflow Requirements

Specifications MUST define:

- prioritized, independently testable user stories or feature slices;
- functional requirements and explicit out-of-scope boundaries;
- non-functional requirements when relevant to architecture, reliability, UX, packaging, performance, or environment compatibility;
- measurable success criteria; and
- assumptions and dependencies that constrain implementation.

Implementation plans MUST translate the constitutional principles into concrete execution rules, including:

- constitution checks tied to the current feature;
- architectural constraints and approved technology choices;
- architectural decomposition and separation-of-concerns strategy;
- test strategy, required test development, and other required validation evidence;
- explicit sequencing rules consistent with the ordered feature and user-story decomposition;
- complexity tracking for justified deviations; and
- explicit sequencing that supports incremental implementation.

Task lists MUST:

- be organized to support independent execution and verification;
- separate foundational work from user-story work;
- reflect the intended architectural decomposition;
- order work according to the canonical user-story sequence defined by the feature specification;
- include the test-development, validation, documentation, and integration tasks required to prove completion;
- include work to update `progress.md`.

Implementation workflow MUST:

- verify that `tasks.md` includes a task requiring updating `progress.md` before feature completion;
- update `progress.md` before completion is considered final;
- update `roadmap.md`, if exists and was the source of the current feature, before completion is considered final;
- fail the workflow if `progress.md` cannot be updated to a consistent state.

Reviewers MUST:

- reject completion when `progress.md` is missing, omitted, or inconsistent with the implemented artifacts.

## Agentic Delivery Requirements

Prompts, templates, and issue-generation workflows MUST be written so that coding agents can act with bounded scope, deterministic guidance, and minimal ambiguity.

Tasks and issues MUST therefore be:

- small enough to implement safely in one focused pass;
- explicit about inputs, outputs, constraints, and dependencies; and
- clear about the acceptance checks, required test development, and validation evidence required for completion.

When tasks are converted into implementation issues, the issue set MUST preserve both the intended architectural decomposition and the phased delivery sequence. Task-to-issue workflows MUST not merge unrelated or independently testable tasks into a single implementation issue merely for administrative convenience.

Repository guidance MUST explicitly define shell selection, environment assumptions, architectural boundaries, and forbidden shortcuts where omission would cause agents to drift from the intended implementation path.

When repository or platform constraints materially affect delivery, the relevant documents, plans, and tasks MUST state those constraints directly rather than assuming they will be inferred during implementation.

## Delivery Workflow

Every feature MUST pass a constitution check during planning and again before final review.

The constitution check MUST confirm that the feature:

- is grounded in an explicit specification;
- is decomposed into small, testable increments;
- is sequenced according to the approved user-story decomposition and preserves the intended incremental delivery order;
- satisfies feature-level decomposition constraints, including:  
    - minimality;  
    - self-sufficiency;  
    - independent implementability and testability;  
    - standalone user-visible value;
- defines the test development and other evidence required for completion;
- records key decisions, assumptions, and exceptions;
- respects the repository's architecture and environment constraints;
- preserves clear architectural decomposition and separation of concerns.

Reviewers MUST reject changes that are too large to review confidently, advance the codebase without corresponding test development, lack required validation evidence, exceed approved scope without amendment, violate architectural or environment constraints, or break the approved staged sequence without a recorded justification.

Implementation SHOULD prefer the narrowest viable change that satisfies the approved specification and acceptance criteria.

## Governance

This constitution is the authoritative decision framework for specifications, plans, tasks, issues, implementation, review, and release readiness in this repository. All delivery artifacts MUST demonstrate compliance with these principles or document an approved exception.

### Runtime Agent Overrides

Spec Kit provides a layered execution and template authority model through core templates, extensions, presets, and project-local overrides. That hierarchy governs default command behavior, template resolution, and agent skill execution.

In addition, a user prompt MAY include a section titled exactly:

- `Agent Override`

When present, `Agent Override` MUST be treated as an explicit runtime override for the current command or workflow stage.

#### Scope and Precedence

An `Agent Override` section:

- MUST take precedence over agent-internal defaults, prompt scaffolding, template defaults, and resolved Spec Kit template/extension behavior for the current command;
- MUST be applied only to the workflow stage or artifact elements it explicitly addresses; and
- MUST NOT be interpreted as overriding the constitution or other already-approved governing artifacts unless the current command is explicitly intended to amend them.

Accordingly, the effective order of precedence for command execution is:

1. Constitution
2. Approved governing artifact(s) already in force for the current workflow stage
3. Explicit `Agent Override` section in the current user prompt
4. Resolved Spec Kit project-local override / preset / extension / core behavior
5. Agent default reasoning and local coding preference

#### Allowed Uses

`Agent Override` MAY be used to supply or constrain:

- user-story decomposition
- milestone or phase structure
- task decomposition
- artifact subsections or tables
- required wording, field values, or metadata
- command-stage execution behavior that would otherwise be inferred by the agent

If the override provides concrete structured content for a governed artifact element, the agent MUST use that content directly rather than regenerating or replacing it heuristically.

Examples include:

- a user-story table supplied to `/speckit.specify`
- a milestone table supplied to `/speckit.tasks` or `/speckit.taskstoissues`
- a required task block or documentation block supplied to `/speckit.tasks`
- a stage-specific execution rule supplied to `/speckit.implement`

#### Constraints

An `Agent Override` section:

- MUST be interpreted narrowly and only for the elements it explicitly addresses;
- MUST preserve consistency with the constitution and with any higher-precedence approved artifact already in force;
- MUST NOT be expanded into unrelated implicit overrides;
- MUST NOT be ignored merely because the agent has a preferred default decomposition or template pattern.

If an override conflicts with the constitution, the constitution prevails.

If an `Agent Override` section conflicts with an already-approved artifact that governs the same workflow stage, the approved artifact MUST remain authoritative. The override MUST NOT be applied implicitly in this case. If modification of the approved artifact is intended, this MUST be treated as an explicit amendment request. The agent MUST:

- preserve the existing artifact as the current source of truth; and
- surface the conflict and the required amendment clearly in the resulting work without silently applying the override.

#### Partial Overrides

If an `Agent Override` section supplies only part of the required content, the agent MUST:

- preserve the supplied content exactly for the addressed portion;
- generate only the unresolved remainder; and
- avoid re-deciding matters already fixed by the override.

### Artifact authority

Constitutional principles take precedence over feature-level preferences. The order of precedence is:

1. Constitution
2. Approved feature specification
3. Approved implementation plan
4. Approved task or issue definition
5. Local coding preference

### Compliance reviews

Compliance reviews occur during:

- feature specification,
- planning,
- task generation,
- implementation review, and
- pre-release validation.

### Versioning policy

- MAJOR: remove or fundamentally redefine a principle or governance rule in a backward-incompatible way;
- MINOR: add a principle, add a mandatory workflow section, or materially expand constitutional guidance; and
- PATCH: clarify wording, fix ambiguity, or make non-semantic editorial improvements.

**Version**: 1.8.0 | **Ratified**: 2026-04-10 | **Last Amended**: 2026-04-22
