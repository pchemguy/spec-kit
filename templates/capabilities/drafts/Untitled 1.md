| Functional Capability                 | Taxonomy Category                 | User-Facing Relevance | Implementation Separability | Extracted Keywords                                                                          |
| ------------------------------------- | --------------------------------- | --------------------- | --------------------------- | ------------------------------------------------------------------------------------------- |
| [Explicit or strongly implied aspect] | [One or more taxonomy categories] | [Classification]      | [Classification]            | [Verbatim or minimally normalized keywords or short phrases directly supporting the aspect] |

#### Grounding and Keyword Traceability

All structured outputs produced during capability decomposition that include an `Extracted Keywords` column MUST be grounded in the target description.

The `Extracted Keywords` column serves as a traceability mechanism linking each row to source signals in the target description.

The `Extracted Keywords` column MUST:

- contain only keywords or short phrases derived directly from the target description;
- use verbatim text where possible, with minimal normalization;
- include only terms that directly support identification of the corresponding row;
- be concise and non-redundant.

The `Extracted Keywords` column MUST NOT:

- introduce inferred terminology not grounded in the target description;
- restate the full row description;
- include explanations, sentences, or interpretations;
- mix unrelated concepts.

Each keyword or phrase SHOULD be assigned to the most appropriate row only, avoiding duplication across multiple rows unless it genuinely supports multiple distinct items.

When multiple tables include `Extracted Keywords`, the LLM MUST ensure consistent grounding across all tables.



A **capability** is a coarse, user-recognizable functional area that reflects something an end user would intentionally use, access, rely on, or care about.

Capabilities form a concise, user-centric map of the target scope. They represent the major kinds of value, access, behavior, or experience described or strongly implied by the target description.

Capabilities are constructed by grouping compatible signals inferred from the target description into cohesive, user-facing areas.

A **capability signal** is an exact keyword, term, or short phrase from the target description that provides source evidence for identifying, classifying, or scoping a capability.
A **capability** is a coarse, user-recognizable functional area inferred from one or more capability signals and reflecting something an end user would intentionally use, access, rely on, or care about.

A **capability** is a coarse functional or form-factor area that reflects something an end user would recognize, intentionally use, access, rely on, or care about. Capabilities form a concise user-centric map of the target scope. They identify the major kinds of value, access, behavior, or experience described or strongly implied by the target description.

A **capability** is a coarse, user-recognizable area of value, behavior, access, or experience inferred from one or more capability signals and reflecting something an end user would intentionally use, access, rely on, or care about.

A **capability** is a coarse, user-recognizable functional or form-factor area inferred from one or more capability signals that reflects something an end user would intentionally use, access, rely on, or care about.




A **capability** is a coarse functional or form-factor area that reflects something an end user would recognize, intentionally use, access, rely on, or care about. Capabilities form a concise user-centric map of the target scope. They identify the major kinds of value, access, behavior, or experience described or strongly implied by the target description.

A **capability signal** is an exact keyword, term, or short phrase from the target description that provides source evidence for identifying, classifying, or scoping a capability.

Each capability is inferred from one or more capability signals

The LLM MUST NOT expand capability semantics beyond what is reasonably supported by the associated capability signals and their immediate context.



----
---


## Capability Decomposition

Capability decomposition is an early analysis activity that converts a target description into a compact, user-centric map of the target scope.

A central component of capability decomposition is capability classification, which provides a structural reasoning model that the LLM MUST use to:

- distinguish primary user intent from supporting behavior and system form;
- guide identification and scoping of capabilities;
- prevent conflation of domain logic, supporting functionality, and form-factor concerns;
- steer boundary decisions during capability scoping.

This classification is also foundational for downstream analysis, but its primary role is to shape decomposition itself.

---

### Core Concepts

The **target description** is the input text or contextual material provided for analysis.

A **target scope** is the product, system, feature area, extension, change, or project evolution inferred from the target description. It MAY represent a complete new system or a bounded change to an existing system.

A **capability signal** is an exact keyword, term, or short phrase from the target description that provides source evidence for identifying, classifying, or scoping a capability.

A **capability** is a coarse functional or form-factor area that reflects something an end user would recognize, intentionally use, access, rely on, or care about. Capabilities form a concise, user-centric map of the target scope and identify the major kinds of value, access, behavior, or experience described or strongly implied by the target description.

The LLM MUST:

- use capability signals as the grounding basis for all capability identification and classification;
- ensure that every capability is fully supported by one or more capability signals;
- infer capability semantics from the meaning and immediate context of associated capability signals.

The LLM MUST NOT expand capability semantics beyond what is reasonably supported by the associated capability signals and their immediate context.

As part of capability decomposition, the LLM MUST identify explicit or strongly implied capabilities from the target description and classify them according to the **Capability Model**. The Capability Model classifies capabilities by their relation to primary user intent and by whether their semantics imply interaction with conceptual system state. It MUST NOT classify capabilities based on interface, presentation, or implementation form. The Capability Model requires each capability to be assigned to exactly one of:

- **Core User Capability** — defines primary user intent and core state semantics.
- **Supporting Functional Capability** — affects, governs, validates, or transforms core state and provides functionality required to make the core user capability usable, complete, or coherent.
- **Non-Functional and Form-Factor (NFFF) Aspect** — defines user access, interaction form, environment, or experience without affecting core state.

---

