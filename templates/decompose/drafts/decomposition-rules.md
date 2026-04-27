#### User Story Decomposition Rules

##### 1. Valid User Story Definition

Each user story MUST:

- be centered around a single user-initiated interaction or action;
- define a complete interaction cycle: user action → system response → observable outcome;
- produce value through successful execution of that interaction, not only through rejection, validation, or constraint enforcement;
- correspond to exactly one distinct user interaction type;  
- define behavior for one operation or one class of operations that share identical execution semantics;
- deliver standalone, interaction-driven user value, where the user intentionally performs an action to achieve a meaningful outcome;
- encapsulate a coherent unit of behavior with consistent logic, state, workflow, and architectural context;

Each user story MUST NOT:

- represent only error handling, validation, or rejection behavior without a primary user interaction;  
- exist solely to enforce constraints, invariants, or correctness policies;
- group multiple user actions that differ in operation semantics, even if they are structurally similar;
- combine unrelated or merely convenient future work;  
- depend on future user stories to become meaningful, complete, or correct.

---

##### 2. Scope, Granularity, and Cohesion

Each user story MUST be minimal in scope relative to its delivered value.

User story decomposition MUST strike a practical balance between:

- **granularity** — stories are small enough to support focused implementation, bounded context, and deterministic agent behavior; and
- **cohesion** — stories are not fragmented so aggressively that closely related, strongly parallel, or structurally similar behavior is split into multiple user stories whose implementation would substantially duplicate:
    - logic;
    - state handling;
    - workflow shape;
    - architectural structure; or
    - Spec Kit process overhead.

User story cohesion MUST be evaluated across candidate user stories.

The LLM MUST identify cases where:

- candidate stories represent sequential refinements of the same capability; or
- candidate stories are tightly coupled or strongly parallel; or
- candidate stories would require substantially similar:
    - implementation workflows;
    - validation workflows; and
    - execution structure and state interaction patterns.

Such stories SHOULD be combined into a single user story when separate treatment would not meaningfully improve:

- clarity;
- validation;
- delivery confidence.

User story decomposition MUST NOT:

- split a single coherent interaction or capability into multiple stories without clear justification;
- create multiple stories that differ only by minor variation in operation while sharing identical semantics;
- introduce artificial boundaries that increase duplication or coordination overhead without improving specification quality.

---

##### 3. Independence, Incremental Viability, and Continuity

Each user story MUST be self-sufficient, independently implementable, and independently testable.

Self-sufficiency and independence mean that a user story:

- can be implemented as a coherent increment on top of all previously accepted user stories;
- does not depend on any future user stories to become meaningful, complete, correct, or testable.

User story progression MUST preserve incremental system validity.

The ordered user story list MUST satisfy:

- after each user story is implemented, the resulting system MUST be:
    - internally consistent;
    - executable;
    - testable at that stage;
- each user story MUST
    - operate as a valid extension of the system produced by prior user stories;
    - introduce a complete interaction capability, not a partial fragment of one;
    - NOT defer essential parts of an interaction (e.g., execution, visibility, or state mutation) to a later story;
    - NOT introduce behavior that requires a later story to become usable or meaningful.

User story decomposition MUST NOT:

- create intermediate system states that are structurally valid but unusable;
- split a single interaction cycle across multiple user stories;
- rely on future user stories to “complete” the behavior introduced in the current story.

---

##### 4. Validation

For each candidate user story, you MUST verify:

- what explicit user action initiates this story;
- what successful outcome the user is trying to achieve;
- whether the story still makes sense if all other stories are removed.

If no clear user action exists, or the story only defines failure modes or constraints, it MUST be rejected or absorbed into SSS.

Classify each candidate as one of:  
  
- Interaction-driven behavior → valid user story;  
- Cross-cutting constraint → SSS;  
- Internal mechanism → invalid (must be merged or removed).  
  
Only the first category is allowed as user stories.

Reject or refine any user story that violates these constraints.

---