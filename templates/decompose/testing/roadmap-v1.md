---
url: https://chatgpt.com/g/g-p-69e6210469388191b8880a8407594f1a-rpn-calculator/c/69ee0f9b-a040-83eb-9e23-9e6da8230474
---

> [!NOTE] Prompt
> 
> Help me perform user story decomposition and feature synthesis. I want to build an RPN calculator web app and have a packaged portable desktop version later on. I am thinking of supporting add, sub, neg, mul, div, sqr, sqrt, inv, pow, abs, exp, and ln functions. I also want to have undo capability.

# Roadmap: RPN Calculator

## Notes *(if applicable)*

This roadmap defines a staged development plan for a Reverse Polish Notation (RPN) calculator implemented as a browser-based application with subsequent packaging as a portable desktop application. The roadmap follows strict Specification-Driven Development principles and is aligned with the project constitution. 

---

## Shared System Semantics (SSS)

The following rules define shared system semantics and apply to all user stories and features.

User stories and features MAY further constrain these rules but MUST NOT contradict, weaken, or bypass them.

---

### Core Domain Definitions

**Definition**: Defines fundamental entities and terminology used across the system.

1. The calculator operates on a **stack** of numeric values.
2. The **top of the stack** is the most recently added or computed value.
3. An **operand** is a valid numeric value entered by the user.
4. An **operator** is a function that consumes one or more operands and produces a result.
5. A **state mutation** is any accepted action that changes the stack or input state.
6. An **undoable action** is any accepted state mutation except reset and initialization.

---

### State Model Assumptions

**Definition**: Defines the conceptual structure of system state.

The system state includes:

1. Stack (ordered collection of numeric values)
2. Input buffer (current numeric input, if applicable)
3. History (ordered list of undoable state mutations)

User stories MUST explicitly declare:

* which state components they read;
* which state components they mutate;
* which state components they preserve;
* which state components they reset.

---

### Numeric Policy

**Definition**: Defines constraints on numeric values and computation results.

1. All numeric values MUST be finite floating-point numbers.
2. Operations producing non-finite results (NaN, ±Infinity) MUST be rejected.
3. Domain-invalid operations (e.g., sqrt of negative number) MUST be rejected.
4. Numeric precision follows standard floating-point semantics without arbitrary rounding rules.

---

### Interaction / Evaluation Convention

**Definition**: Defines how operations are interpreted and executed.

1. For binary operators:
    * rhs := stack[top]
    * lhs := stack[top - 1]
2. Evaluation MUST follow Reverse Polish Notation semantics.
3. Unary operators operate on the top of the stack.
4. Operators consume operands and replace them with the result.

---

### Error / Rejection Policy

**Definition**: Defines how invalid actions are handled.

1. Actions MUST be rejected if:
    * insufficient operands exist;
    * numeric policy violations occur;
    * input is invalid.
2. Rejected actions MUST NOT mutate any state.
3. Rejections MUST produce user-visible feedback.
4. Feedback MUST be non-modal and non-blocking.

---

### State Mutation Policy

**Definition**: Defines global guarantees about state transitions.

1. State changes occur only through accepted operations.
2. Rejected operations MUST NOT mutate any state.
3. State transitions MUST be atomic from the user perspective.

---

### History / Reversibility Policy

**Definition**: Defines undo behavior.

1. Operand entry actions are undoable.
2. Operator applications are undoable.
3. Reset is NOT undoable.
4. Startup initialization is NOT undoable.
5. Rejected actions are NOT recorded in history.
6. Undo restores the immediately previous undoable state.

---

### Initialization / Reset Policy

**Definition**: Defines baseline state behavior.

1. The system MUST initialize into an empty stack state.
2. Reset MUST:
    * clear the stack;
    * clear input buffer;
    * preserve no prior history.
3. Reset MUST produce a consistent baseline state.
4. Initialization MUST be equivalent to reset.

---

### Interaction and UX Conventions

**Definition**: Defines user interaction expectations.

1. Input is provided via a UI mechanism (keyboard or UI controls).
2. The full stack MUST be visible to the user.
3. Feedback MUST be displayed without blocking interaction.
4. No modal dialogs are permitted for standard operation feedback.

---

### Cross-Environment Consistency Constraint

**Definition**: Defines invariants across web and desktop environments.

1. All calculation semantics MUST remain identical across environments.
2. Stack behavior, operator semantics, and history MUST be identical.
3. Packaging MUST NOT alter computation behavior.

---

## User Stories

### User Story US1 — Reset calculator state

Status: planned

#### Description

User resets the calculator to return it to a known clean baseline state.

#### Shared System Semantics References

* SSS - Initialization / Reset Policy
* SSS - State Model Assumptions
* SSS - State Mutation Policy

#### Scope

Reset operation and implicit initialization behavior.

#### Included Behavior

* Clear stack
* Clear input buffer
* Clear history

#### State Interaction

* Reads: none
* Mutates: stack, input buffer, history
* Preserves: none
* Resets: all state
* Must Not Affect: external environment

#### Acceptance Scenarios

1. **Given** a non-empty stack, **When** reset is triggered, **Then** stack becomes empty
2. **Given** prior operations, **When** reset is triggered, **Then** history is cleared

#### Exception Scenarios

* Reset is always valid and cannot be rejected

---

### User Story US2 — Enter operands and view the stack

Status: planned

#### Description

User enters numeric operands and observes the stack.

#### Shared System Semantics References

* SSS - Core Domain Definitions
* SSS - Numeric Policy - (1)
* SSS - State Model Assumptions
* SSS - State Mutation Policy

#### Scope

Operand input and stack visibility.

#### Included Behavior

* Accept numeric input
* Push operand to stack
* Display stack

#### State Interaction

* Reads: input buffer
* Mutates: stack, history
* Preserves: existing stack values
* Resets: none
* Must Not Affect: invalid inputs must not mutate state

#### Acceptance Scenarios

1. **Given** empty stack, **When** user enters 5, **Then** stack becomes [5]
2. **Given** stack [5], **When** user enters 3, **Then** stack becomes [5, 3]

#### Exception Scenarios

* Invalid numeric input is rejected without state change

---

### User Story US3 — Apply additive and sign-altering operators

Status: planned

#### Description

User applies additive and sign-altering operators to compute results.

#### Shared System Semantics References

* SSS - Interaction / Evaluation Convention
* SSS - Numeric Policy
* SSS - Error / Rejection Policy

#### Scope

Operators: add, sub, neg, abs

#### Included Behavior

* Binary operations: add, sub
* Unary operations: neg, abs

#### State Interaction

* Reads: stack
* Mutates: stack, history
* Preserves: unaffected stack values
* Resets: none
* Must Not Affect: invalid operations must not mutate state

#### Acceptance Scenarios

1. **Given** stack [2, 3], **When** add is applied, **Then** stack becomes [5]
2. **Given** stack [5], **When** neg is applied, **Then** stack becomes [-5]
3. **Given** stack [-5], **When** abs is applied, **Then** stack becomes [5]

#### Exception Scenarios

* Insufficient operands → rejected
* Invalid numeric results → rejected

---

### User Story US4 — Apply multiplicative operators

Status: planned

#### Description

User applies multiplicative operators.

#### Shared System Semantics References

* SSS - Interaction / Evaluation Convention
* SSS - Numeric Policy
* SSS - Error / Rejection Policy

#### Scope

Operators: mul, div, inv

#### Included Behavior

* Multiplication
* Division
* Reciprocal

#### State Interaction

* Reads: stack
* Mutates: stack, history
* Preserves: unaffected stack values
* Resets: none
* Must Not Affect: invalid operations must not mutate state

#### Acceptance Scenarios

1. **Given** stack [2, 3], **When** mul is applied, **Then** stack becomes [6]
2. **Given** stack [6, 2], **When** div is applied, **Then** stack becomes [3]
3. **Given** stack [2], **When** inv is applied, **Then** stack becomes [0.5]

#### Exception Scenarios

* Division by zero → rejected
* Insufficient operands → rejected

---

### User Story US5 — Apply power and root operators

Status: planned

#### Description

User applies power and root operators to compute nonlinear numeric results.

#### Shared System Semantics References

* SSS - Core Domain Definitions
* SSS - Numeric Policy
* SSS - Interaction / Evaluation Convention
* SSS - Error / Rejection Policy
* SSS - State Mutation Policy
* SSS - History / Reversibility Policy

#### Scope

Power and root operators: `sqr`, `sqrt`, `pow`.

#### Included Behavior

* Apply `sqr` to square the top stack value.
* Apply `sqrt` to compute the square root of the top stack value.
* Apply `pow` as a binary operator using RPN operand ordering.

#### State Interaction

* Reads: stack
* Mutates: stack, history
* Preserves: unaffected stack values, input buffer
* Resets: none
* Must Not Affect: rejected operations must not mutate stack, input buffer, or history

#### Acceptance Scenarios

1. **Given** stack `[4]`, **When** `sqr` is applied, **Then** stack becomes `[16]`
2. **Given** stack `[9]`, **When** `sqrt` is applied, **Then** stack becomes `[3]`
3. **Given** stack `[2, 3]`, **When** `pow` is applied, **Then** stack becomes `[8]`

#### Exception Scenarios

* `sqrt` with negative input is rejected without state change.
* `pow` with insufficient operands is rejected without state change.
* Any operation producing a non-finite result is rejected without state change.

---

### User Story US6 — Apply exponential and logarithmic operators

Status: planned

#### Description

User applies exponential and natural logarithm operators to compute transcendental numeric results.

#### Shared System Semantics References

* SSS - Core Domain Definitions
* SSS - Numeric Policy
* SSS - Interaction / Evaluation Convention
* SSS - Error / Rejection Policy
* SSS - State Mutation Policy
* SSS - History / Reversibility Policy

#### Scope

Exponential and logarithmic operators: `exp`, `ln`.

#### Included Behavior

* Apply `exp` to compute the natural exponential of the top stack value.
* Apply `ln` to compute the natural logarithm of the top stack value.

#### State Interaction

* Reads: stack
* Mutates: stack, history
* Preserves: unaffected stack values, input buffer
* Resets: none
* Must Not Affect: rejected operations must not mutate stack, input buffer, or history

#### Acceptance Scenarios

1. **Given** stack `[0]`, **When** `exp` is applied, **Then** stack becomes `[1]`
2. **Given** stack `[1]`, **When** `ln` is applied, **Then** stack becomes `[0]`
3. **Given** stack `[2]`, **When** `exp` is applied, **Then** stack contains the finite floating-point result of `e^2`

#### Exception Scenarios

* `ln` with zero input is rejected without state change.
* `ln` with negative input is rejected without state change.
* Any operation producing a non-finite result is rejected without state change.
* Any operator applied with insufficient operands is rejected without state change.

---

### User Story US7 — Undo the last undoable calculator action

Status: planned

#### Description

User undoes the most recent undoable calculator action to restore the immediately previous calculator state.

#### Shared System Semantics References

* SSS - Core Domain Definitions - (6)
* SSS - State Model Assumptions
* SSS - State Mutation Policy
* SSS - History / Reversibility Policy
* SSS - Error / Rejection Policy

#### Scope

Undo behavior for accepted operand entry and accepted operator application actions.

#### Included Behavior

* Undo accepted operand entry.
* Undo accepted operator application.
* Restore the immediately previous undoable calculator state.
* Preserve reset and startup initialization as non-undoable boundaries.

#### State Interaction

* Reads: history, stack, input buffer
* Mutates: stack, input buffer, history
* Preserves: external environment
* Resets: none
* Must Not Affect: reset must not become undoable; rejected actions must not become undoable

#### Acceptance Scenarios

1. **Given** stack `[5]` after entering `5`, **When** undo is triggered, **Then** stack returns to `[]`
2. **Given** stack `[2, 3]` followed by accepted `add`, **When** undo is triggered, **Then** stack returns to `[2, 3]`
3. **Given** stack `[2, 3]` followed by reset, **When** undo is triggered, **Then** reset is not reverted

#### Exception Scenarios

* Undo with no undoable history is rejected without state change.
* Undo after reset with no subsequent undoable action is rejected without state change.
* Undo must not restore states prior to reset.

---

### User Story US8 — Use the calculator as a packaged portable desktop app

Status: planned

#### Description

User runs the calculator as a packaged portable desktop application while preserving the same behavior available in the browser-based application.

#### Shared System Semantics References

* SSS - Core Domain Definitions
* SSS - State Model Assumptions
* SSS - Numeric Policy
* SSS - Interaction / Evaluation Convention
* SSS - Error / Rejection Policy
* SSS - State Mutation Policy
* SSS - History / Reversibility Policy
* SSS - Initialization / Reset Policy
* SSS - Interaction and UX Conventions
* SSS - Cross-Environment Consistency Constraint

#### Scope

Portable desktop packaging and behavior parity.

#### Included Behavior

* Launch the calculator as a portable desktop application.
* Preserve calculator behavior from the browser-based application.
* Preserve stack, numeric, operator, error, rejection, reset, and undo semantics.
* Provide a desktop-packaged user experience without redefining calculator behavior.

#### State Interaction

* Reads: calculator state according to already implemented behavior
* Mutates: calculator state according to already implemented behavior
* Preserves: calculator semantics across environments
* Resets: startup initialization according to SSS
* Must Not Affect: operator semantics, numeric policy, rejection semantics, undo semantics

#### Acceptance Scenarios

1. **Given** the packaged desktop app is launched, **When** the user enters operands and applies operators, **Then** behavior matches the browser-based calculator
2. **Given** the packaged desktop app is launched, **When** startup completes, **Then** the calculator initializes into the reset-equivalent baseline state
3. **Given** the packaged desktop app is used, **When** invalid operations are attempted, **Then** rejection behavior matches the browser-based calculator

#### Exception Scenarios

* Packaging must not introduce calculator behavior differences.
* Packaging must not weaken or bypass numeric, state, history, reset, or rejection policies.

---

## Features

### Feature F1 — Core RPN Calculator MVP

Status: planned

#### Metadata

* ID: F1
* User stories included: US1, US2, US3, US4
* Scope: Browser-based RPN calculator MVP supporting reset/initialization, operand entry, full stack visibility, additive/sign-altering operators, and multiplicative operators.

#### Specify User Prompt

Create a browser-based Reverse Polish Notation calculator MVP.

The calculator MUST allow the user to reset the calculator state, enter finite numeric operands, view the full stack, and apply the following operators:

* additive and sign-altering operators: `add`, `sub`, `neg`, `abs`
* multiplicative operators: `mul`, `div`, `inv`

The calculator MUST initialize into the same clean baseline state produced by reset.

The calculator MUST use standard RPN stack semantics. For binary operators, the top stack value is the right-hand operand and the next value below it is the left-hand operand.

The calculator MUST reject invalid numeric input, insufficient operands, division by zero, reciprocal of zero, domain-invalid operations, and non-finite results without mutating state.

Rejected actions MUST produce non-modal, non-blocking, user-visible feedback.

The calculator MUST display the full stack.

##### Agent Override

###### Shared Definitions, Conventions, and Policies

The specification MUST inherit the roadmap SSS as authoritative shared context.

Applicable SSS definitions, conventions, policies, and rules for this feature:

* SSS - Core Domain Definitions
* SSS - State Model Assumptions
* SSS - Numeric Policy
* SSS - Interaction / Evaluation Convention
* SSS - Error / Rejection Policy
* SSS - State Mutation Policy
* SSS - Initialization / Reset Policy
* SSS - Interaction and UX Conventions

Do not redefine, weaken, duplicate, or localize these rules inside the feature specification. Use them as inherited constraints when writing requirements, acceptance scenarios, edge cases, exceptions, assumptions, and success criteria.

###### User Story Decomposition

Follow these constraints:

1. The following user story set is canonical for the spec.
2. Preserve exactly these user stories in the User Scenarios & Testing section.
3. Do not merge, split, reorder, rename, or re-prioritize these user stories.
4. Provide user story details for each story according to the spec-template.
5. Any additional detail must go into acceptance scenarios, edge cases, requirements, assumptions, non-goals, or success criteria, not into changing the story decomposition.

Use exactly this canonical story set:

| # | User Story                                       |
| - | ------------------------------------------------ |
| 1 | US1 — Reset calculator state                     |
| 2 | US2 — Enter operands and view the stack          |
| 3 | US3 — Apply additive and sign-altering operators |
| 4 | US4 — Apply multiplicative operators             |

---

### Feature F2 — Advanced Mathematical Operations

Status: planned

#### Metadata

* ID: F2
* User stories included: US5, US6
* Scope: Extension of the calculator operator set with power/root operators and exponential/logarithmic operators.

#### Specify User Prompt

Extend the existing browser-based Reverse Polish Notation calculator with advanced mathematical operators.

The calculator MUST add support for:

* power and root operators: `sqr`, `sqrt`, `pow`
* exponential and logarithmic operators: `exp`, `ln`

The feature MUST preserve the already implemented calculator behavior from the core MVP, including reset, initialization, operand entry, stack visibility, additive/sign-altering operators, multiplicative operators, RPN operand ordering, numeric validity rules, rejection behavior, and state mutation guarantees.

The new operators MUST use the same stack, numeric, feedback, and rejection policies as existing operators.

The calculator MUST reject insufficient operands, domain-invalid operations, and non-finite results without mutating state.

##### Agent Override

###### Shared Definitions, Conventions, and Policies

The specification MUST inherit the roadmap SSS as authoritative shared context.

Applicable SSS definitions, conventions, policies, and rules for this feature:

* SSS - Core Domain Definitions
* SSS - State Model Assumptions
* SSS - Numeric Policy
* SSS - Interaction / Evaluation Convention
* SSS - Error / Rejection Policy
* SSS - State Mutation Policy
* SSS - Initialization / Reset Policy
* SSS - Interaction and UX Conventions
* SSS - Cross-Feature Continuity

Do not redefine, weaken, duplicate, or localize these rules inside the feature specification. Use them as inherited constraints when writing requirements, acceptance scenarios, edge cases, exceptions, assumptions, and success criteria.

###### User Story Decomposition

Follow these constraints:

1. The following user story set is canonical for the spec.
2. Preserve exactly these user stories in the User Scenarios & Testing section.
3. Do not merge, split, reorder, rename, or re-prioritize these user stories.
4. Provide user story details for each story according to the spec-template.
5. Any additional detail must go into acceptance scenarios, edge cases, requirements, assumptions, non-goals, or success criteria, not into changing the story decomposition.

Use exactly this canonical story set:

| # | User Story                                        |
| - | ------------------------------------------------- |
| 1 | US5 — Apply power and root operators              |
| 2 | US6 — Apply exponential and logarithmic operators |

---

### Feature F3 — Undo Capability

Status: planned

#### Metadata

* ID: F3
* User stories included: US7
* Scope: Undo support for accepted operand entry and accepted operator application actions, excluding reset and startup initialization.

#### Specify User Prompt

Extend the existing Reverse Polish Notation calculator with undo capability.

The calculator MUST allow the user to undo the most recent undoable calculator action.

Undoable actions are:

* accepted operand entry actions;
* accepted operator applications.

The following actions are not undoable:

* reset;
* startup initialization;
* rejected actions.

Undo MUST restore the immediately previous undoable calculator state. Undo MUST NOT restore states prior to reset. Undo with no undoable history MUST be rejected without state change and must produce non-modal, non-blocking, user-visible feedback.

The feature MUST preserve all existing calculator behavior, including stack semantics, operator semantics, reset behavior, numeric policy, and rejection behavior.

##### Agent Override

###### Shared Definitions, Conventions, and Policies

The specification MUST inherit the roadmap SSS as authoritative shared context.

Applicable SSS definitions, conventions, policies, and rules for this feature:

* SSS - Core Domain Definitions - (6)
* SSS - State Model Assumptions
* SSS - Numeric Policy
* SSS - Interaction / Evaluation Convention
* SSS - Error / Rejection Policy
* SSS - State Mutation Policy
* SSS - History / Reversibility Policy
* SSS - Initialization / Reset Policy
* SSS - Interaction and UX Conventions
* SSS - Cross-Feature Continuity

Do not redefine, weaken, duplicate, or localize these rules inside the feature specification. Use them as inherited constraints when writing requirements, acceptance scenarios, edge cases, exceptions, assumptions, and success criteria.

###### User Story Decomposition

Follow these constraints:

1. The following user story set is canonical for the spec.
2. Preserve exactly these user stories in the User Scenarios & Testing section.
3. Do not merge, split, reorder, rename, or re-prioritize these user stories.
4. Provide user story details for each story according to the spec-template.
5. Any additional detail must go into acceptance scenarios, edge cases, requirements, assumptions, non-goals, or success criteria, not into changing the story decomposition.

Use exactly this canonical story set:

| # | User Story                                     |
| - | ---------------------------------------------- |
| 1 | US7 — Undo the last undoable calculator action |

---

### Feature F4 — Portable Desktop Packaging

Status: planned

#### Metadata

* ID: F4
* User stories included: US8
* Scope: Package the calculator as a portable desktop application while preserving browser calculator behavior.

#### Specify User Prompt

Package the existing Reverse Polish Notation calculator as a portable desktop application.

The desktop application MUST preserve the same calculator behavior as the browser-based application, including:

* reset and startup initialization behavior;
* operand entry;
* full stack visibility;
* all supported operators;
* RPN operand ordering;
* finite floating-point numeric policy;
* rejection behavior;
* undo behavior.

The desktop packaging MUST NOT redefine calculator behavior or introduce environment-specific calculation semantics.

The application MUST follow the repository technology platform: React, TypeScript, Electron, and SQLite where persistence or backend storage is required. The architecture MUST remain browser-oriented application logic packaged for desktop use rather than a server-centric application, as required by the constitution. 

##### Agent Override

###### Shared Definitions, Conventions, and Policies

The specification MUST inherit the roadmap SSS as authoritative shared context.

Applicable SSS definitions, conventions, policies, and rules for this feature:

* SSS - Core Domain Definitions
* SSS - State Model Assumptions
* SSS - Numeric Policy
* SSS - Interaction / Evaluation Convention
* SSS - Error / Rejection Policy
* SSS - State Mutation Policy
* SSS - History / Reversibility Policy
* SSS - Initialization / Reset Policy
* SSS - Interaction and UX Conventions
* SSS - Cross-Environment Consistency Constraint
* SSS - Cross-Feature Continuity

Do not redefine, weaken, duplicate, or localize these rules inside the feature specification. Use them as inherited constraints when writing requirements, acceptance scenarios, edge cases, exceptions, assumptions, and success criteria.

###### User Story Decomposition

Follow these constraints:

1. The following user story set is canonical for the spec.
2. Preserve exactly these user stories in the User Scenarios & Testing section.
3. Do not merge, split, reorder, rename, or re-prioritize these user stories.
4. Provide user story details for each story according to the spec-template.
5. Any additional detail must go into acceptance scenarios, edge cases, requirements, assumptions, non-goals, or success criteria, not into changing the story decomposition.

Use exactly this canonical story set:

| # | User Story                                                  |
| - | ----------------------------------------------------------- |
| 1 | US8 — Use the calculator as a packaged portable desktop app |

---
