---
url: https://chatgpt.com/g/g-p-69e6210469388191b8880a8407594f1a-rpn-calculator/c/69ee3260-0510-83eb-a940-ecee9b09259f
---
# Included Behavior

Yes. The immediate weakness in `roadmap-v1.md` is that `Included Behavior` is being populated as a **loose capability list**, not as a **semantically auditable execution contract**.

That is why the later audit was valuable: it exposed that short bullets like “Accept numeric input”, “Display stack”, “Division”, “Undo accepted operator application”, and “Launch as portable desktop application” were too coarse to force early SSS creation. In the generated roadmap, US1 and US2 use terse behavior bullets such as “Clear stack”, “Clear input buffer”, “Accept numeric input”, “Push operand to stack”, and “Display stack”; those are structurally valid but under-specified as audit anchors.  The audit then had to infer missing edge classes such as numeric token syntax, input buffer lifecycle, stack capacity, stack display overflow, top-of-stack orientation, and numeric display format. 

The patch should therefore make `Included Behavior` creation much stricter.

---

## Diagnosis: What Went Wrong in `Included Behavior`

### 1. Bullets describe capabilities, not execution semantics

Example:

```markdown
* Accept numeric input
* Push operand to stack
* Display stack
```

These are not wrong, but they are too abstract. They do not force the model to decide whether `Accept numeric input` includes token parsing, partial input, negative literals, scientific notation, empty input, `-0`, whitespace, or display normalization. The audit had to discover those missing semantic classes after the fact. 

### 2. Some bullets mix operation families too broadly

US3 says:

```markdown
* Binary operations: add, sub
* Unary operations: neg, abs
```

This is compact, but it hides behavior variation. `add` and `sub` share arity but differ in operand-order sensitivity; `neg` and `abs` share arity but differ in signed-zero and magnitude behavior. The audit found these details only later, especially signed zero and floating-point artifacts. 

### 3. Some bullets are too implementation-like or environment-like

US8 includes:

```markdown
* Launch the calculator as a portable desktop application.
* Provide a desktop-packaged user experience without redefining calculator behavior.
```

Those are valid story-level behaviors, but they do not force the prompt to define “portable”, launch mode, first launch vs later launch, persistence, unsupported OS behavior, or permitted desktop affordances. The audit identified these as missing desktop packaging boundary semantics. 

### 4. Cross-cutting policies leak into exception scenarios instead of SSS

For US5 and US6, operator domain rules appear partly in `Exception Scenarios`, such as `sqrt` negative input and `ln` zero/negative input.  The audit correctly says these should be centralized in a real-valued operator domain policy rather than scattered locally. 

### 5. Undo bullets are closer, but still miss lifecycle semantics

US7’s bullets are better because they separate operand-entry undo, operator-application undo, state restoration, and reset/startup boundaries. But they still do not force decisions about repeated undo, whether undo itself is recorded, post-undo branch behavior, redo absence, or whether feedback/status is part of undoable state. The audit found exactly those holes. 

---

## Desired Rule

`Included Behavior` must be authored so that each bullet can be audited without guesswork.

It should answer:

> “What accepted execution behavior belongs to this user story, and what semantic surface must SSS or this story define before the behavior is specification-ready?”

It should **not** merely list feature nouns.

---

## Prompt Patch: Included Behavior Creation Rules

Insert this under **User Story Decomposition Rules**, after the existing paragraph that says each user story must define a complete interaction cycle.

```markdown
---

#### Included Behavior Creation Rules

The `#### Included Behavior` section of each user story is a semantic execution contract, not a loose capability list.

Each `Included Behavior` item MUST identify a distinct accepted behavior that occurs inside the user story’s primary interaction cycle.

Each item MUST be specific enough to support later edge-class enumeration, SSS coverage analysis, state interaction analysis, acceptance scenario derivation, and exception scenario derivation.

The LLM MUST create `Included Behavior` items according to the following rules.

##### 1. Accepted-Behavior Rule

Each item MUST describe behavior that occurs when the user story succeeds.

The item MUST NOT describe only:

- validation;
- rejection;
- error handling;
- constraints;
- internal mechanisms;
- implementation tasks;
- visual styling;
- future behavior from another user story.

Invalid, rejected, and exceptional behavior belongs in `Exception Scenarios` or SSS, unless the item describes the successful behavior whose invalid classes must later be audited.

##### 2. Execution-Semantics Rule

Each item MUST describe what the system does in response to the user action.

Prefer action-oriented phrasing:

- `Commit a valid numeric token as an operand`
- `Push the committed operand onto the stack`
- `Apply add using binary RPN operand order`
- `Restore the previous undoable calculator state`

Avoid vague capability phrasing:

- `Numeric input`
- `Stack support`
- `Undo support`
- `Desktop mode`
- `Error handling`

##### 3. Atomicity Rule

Each item SHOULD describe one auditable behavior.

Split an item when its parts have materially different:

- arity;
- operand roles;
- state mutations;
- domain constraints;
- rejection classes;
- history behavior;
- display behavior;
- environment behavior.

Do NOT split merely for mechanical verbosity when multiple operations have identical execution semantics and identical edge classes.

##### 4. Operation Grouping Rule

Operations MAY be grouped in one `Included Behavior` item only when they share the same semantic class.

The LLM MUST NOT group operations merely because they are displayed together or are mathematically related.

A grouped operation item MUST state the shared semantic basis.

Examples:

- Valid: `Apply unary sign/magnitude operators with top-of-stack operand semantics: neg, abs`
- Valid: `Apply binary additive operators with RPN operand order: add, sub`
- Valid: `Apply unary transcendental operators with real-valued finite-result constraints: exp, ln`
- Invalid: `Apply advanced operators: sqr, sqrt, pow, exp, ln`

If grouped operations have different domain constraints, the item MUST either:

- split them into separate Included Behavior items; or
- reference an SSS section that defines the operator-specific domain rules.

##### 5. State-Surface Rule

Each `Included Behavior` item MUST make clear which conceptual state surface it acts on.

Where applicable, the item SHOULD identify whether it involves:

- input buffer;
- committed operand;
- stack;
- history;
- status / feedback;
- reset baseline;
- environment / packaging boundary.

The detailed state declaration still belongs in `#### State Interaction`, but `Included Behavior` MUST be specific enough that the state interaction can be derived without guessing.

##### 6. SSS Trigger Rule

While creating each `Included Behavior` item, the LLM MUST ask whether the behavior depends on a shared rule.

If the behavior depends on a rule that applies to more than one user story, the LLM MUST create or revise an SSS section instead of embedding the rule locally.

Common SSS-triggering concerns include:

- valid input token syntax;
- numeric value validity;
- numeric representation and comparison;
- stack ordering and capacity;
- stack display expectations;
- operation arity and operand ordering;
- real-valued operator domain constraints;
- rejection and no-mutation behavior;
- reset and initialization semantics;
- history and undo boundaries;
- feedback/status treatment;
- cross-environment behavior parity;
- packaging and portability boundaries.

##### 7. Edge-Class Prompting Rule

For every `Included Behavior` item, the LLM MUST ensure that the item is phrased so that the following question can be answered directly:

> What domain edge classes must be considered for this behavior?

If the answer is unclear, the item is too vague and MUST be rewritten before Phase 1 is accepted.

##### 8. Scope Boundary Rule

`Included Behavior` MUST stay inside the user story’s scope.

The item MUST NOT pull in:

- future user stories;
- feature-level packaging concerns unless the user story is specifically about packaging;
- global policies better represented in SSS;
- acceptance examples better represented in `Acceptance Scenarios`.

##### 9. Exception Separation Rule

`Included Behavior` MUST define accepted behavior only.

Rejected behavior MUST be represented by:

- SSS rules, when cross-cutting; and/or
- `Exception Scenarios`, when specific to the user story.

However, if an accepted behavior has known invalid classes, the behavior item MUST be specific enough for those invalid classes to be audited.

##### 10. Verifiability Rule

Each `Included Behavior` item MUST be verifiable through at least one of:

- an acceptance scenario;
- an exception scenario;
- an SSS rule reference;
- a state interaction declaration;
- a later feature-level requirement.

If no verification path exists, the item is too vague or out of scope.

---
```

---

## Patch: Included Behavior Quality Gate

Add this near the end of **User Story Decomposition Rules**, before “Reject or refine any user story…”

```markdown
##### Included Behavior Quality Gate

Before accepting a user story, the LLM MUST validate its `Included Behavior` section.

For each item, verify:

1. The item describes accepted execution behavior.
2. The item is not merely a capability label.
3. The item is inside the story scope.
4. The item has a clear initiating user action or occurs directly within the user action’s response cycle.
5. The item has a clear observable or state outcome.
6. The item’s state surface is identifiable.
7. The item can be audited for domain edge classes.
8. The item does not duplicate SSS.
9. The item does not hide multiple materially different semantic classes.
10. The item has an obvious verification path through SSS, State Interaction, Acceptance Scenarios, or Exception Scenarios.

If any item fails this gate, the LLM MUST revise the `Included Behavior` section before Phase 1 can be considered complete.
```

---

## Patch: Add a Better Definition to the User Story Subtemplate

Replace this in the User Story Subtemplate:

```markdown
#### Included Behavior

Defines execution semantics of this user story.
```

with:

```markdown
#### Included Behavior

List the accepted execution behaviors included in this user story.

Each item MUST be an auditable behavior statement, not a vague capability label.

Each item MUST:

- describe successful system behavior within this user story’s interaction cycle;
- be specific enough to support edge-class enumeration;
- make the relevant state surface identifiable;
- avoid duplicating SSS rules;
- avoid rejected behavior unless naming the successful behavior whose invalid classes are handled elsewhere.

Use concise bullets.

Example format:

- [Accepted behavior statement]
- [Accepted behavior statement]
- [Accepted behavior statement]
```

The current subtemplate only says “Defines execution semantics,” which is too weak; it does not tell the model what an execution-semantic item looks like. 

---

## Patch: Add Intermediate Authoring Checklist During Phase 1

Insert this under **Phase 1 — Exploration and Decomposition**, after “propose an initial user story breakdown”.

```markdown
For each proposed user story, the LLM MUST author `Included Behavior` before finalizing the story.

The LLM MUST derive `Included Behavior` using this sequence:

1. Identify the primary user action.
2. Identify the accepted interaction cycle.
3. Identify each distinct successful system behavior inside that cycle.
4. Determine whether each behavior:
    - belongs locally in the user story;
    - belongs in SSS;
    - belongs in exception scenarios;
    - belongs in acceptance scenarios only;
    - belongs in a different user story.
5. Write only accepted local behavior items under `Included Behavior`.
6. Promote shared rules to SSS.
7. Validate every item using the Included Behavior Quality Gate.
```

---

## Optional Patch: Add an `Included Behavior` Anti-Examples Section

This would be useful because the exact failure mode is “the model writes short labels”. Add this after `Included Behavior Creation Rules`.

```markdown
##### Included Behavior Examples and Anti-Examples

The following are anti-examples because they are vague capability labels:

- `Accept input`
- `Display result`
- `Support undo`
- `Handle errors`
- `Desktop support`
- `Advanced math`

The following are better because they describe auditable accepted behavior:

- `Commit a valid numeric input token as a finite operand`
- `Push the committed operand onto the top of the stack while preserving existing stack order`
- `Display all current stack entries with the top of stack identifiable`
- `Apply add and sub as binary operators using RPN operand order`
- `Apply neg and abs as unary operators to the top stack value`
- `Restore stack and input buffer to the immediately previous undoable state`
- `Launch the packaged desktop calculator without requiring a separate user-managed backend service`
- `Preserve calculator semantics between browser and desktop environments`
```

---

## What This Would Have Changed in `roadmap-v1.md`

### US2 before

```markdown
* Accept numeric input
* Push operand to stack
* Display stack
```

### US2 after

```markdown
* Commit a valid numeric input token as a finite operand.
* Push the committed operand onto the top of the stack while preserving existing stack order.
* Display all current stack entries with the top of stack identifiable.
```

This immediately triggers SSS needs for token validity, stack ordering/capacity, display orientation, and numeric representation.

### US4 before

```markdown
* Multiplication
* Division
* Reciprocal
```

### US4 after

```markdown
* Apply `mul` as a binary operator using RPN operand order.
* Apply `div` as a binary operator using RPN operand order and real-valued finite-result constraints.
* Apply `inv` as a unary reciprocal operator using top-of-stack operand semantics and real-valued finite-result constraints.
```

This exposes the need for zero-equivalence, reciprocal-zero rejection, and finite-result policy.

### US7 before

```markdown
* Undo accepted operand entry.
* Undo accepted operator application.
* Restore the immediately previous undoable calculator state.
* Preserve reset and startup initialization as non-undoable boundaries.
```

This is mostly good, but should become:

```markdown
* Undo the most recent accepted operand-entry action.
* Undo the most recent accepted operator-application action.
* Restore stack and input buffer to the immediately previous undoable calculator state.
* Treat reset and startup initialization as hard non-undoable history boundaries.
```

This forces the audit to ask whether undo itself is recorded, whether repeated undo is allowed, and whether feedback/status is part of undoable state.

---

## Recommended Minimal Insertion Points

Use three insertions only:

1. Under **User Story Decomposition Rules**
   Add `Included Behavior Creation Rules`.

2. Under **Phase 1 — Exploration and Decomposition**
   Add the derivation sequence for `Included Behavior`.

3. In **User Story Subtemplate**
   Replace the one-line placeholder with the stronger definition.

That is enough. The later Phase 2 audit can then operate on better-authored `Included Behavior`, instead of repairing weak bullets after the fact.
