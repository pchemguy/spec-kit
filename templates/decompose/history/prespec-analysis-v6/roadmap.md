---
url: https://chatgpt.com/c/69f25c48-1d00-83eb-9687-9493dd94a816
---

> [!NOTE] Prompt
> 
> I want to build an RPN calculator web app and have a packaged desktop version later on. I am thinking of supporting add, sub, neg, mul, div, sqr, sqrt, inv, pow, abs, exp, and ln functions. I also want to have undo capability.

# Roadmap: RPN Calculator

## Shared System Semantics (SSS)

The following rules define shared system semantics and apply to all user stories and features.

User stories and features MAY add story-specific or feature-specific constraints only when those constraints do not apply across multiple user stories or features. Any repeated or cross-cutting constraint MUST be promoted to SSS. If a story-specific or feature-specific constraint later appears in another story or feature, it MUST be promoted to SSS and removed from local definitions.

---

### Core Domain Definitions

**Definition**: Defines fundamental entities and terminology used across the system.

1. The system operates as a Reverse Polish Notation (RPN) calculator.
2. A stack is an ordered collection of numeric values.
3. The top of stack is the most recently added value.
4. For binary operators:
   - the top of stack is the right-hand operand;
   - the next value below is the left-hand operand.
5. Unary operators operate on the top of stack only.
6. An operator is a user-invoked action that transforms stack values.
7. An operand is a numeric value provided by the user and pushed onto the stack.
8. A calculation session spans from reset to reset.

---

### State Model

**Definition**: Defines the conceptual structure of system state.

The system state includes:

1. Stack — ordered collection of numeric values.
2. Input buffer — current numeric entry being constructed.
3. History — ordered record of reversible actions.
4. Status — current user-visible feedback message.

---

### Numeric Policy

**Definition**: Defines valid numeric values and computation constraints.

1. Only finite real numbers are allowed as valid operands and results.
2. Operations that would produce `NaN`, positive infinity, or negative infinity MUST be rejected.
3. Numeric representation uses standard floating-point semantics.
4. Overflow that produces a non-finite result MUST be rejected.
5. Underflow to a finite representable value is allowed.
6. Division by zero MUST be rejected.
7. Square root of a negative number MUST be rejected.
8. Natural logarithm of a non-positive number MUST be rejected.
9. Zero raised to zero MUST be rejected.
10. Zero raised to a negative exponent MUST be rejected.
11. A negative base raised to a non-integer exponent MUST be rejected.

---

### Operation Evaluation Semantics

**Definition**: Defines how operators are applied to the stack.

1. Binary operators MUST pop the right-hand operand first, then the left-hand operand.
2. The result of an accepted operation MUST be pushed onto the stack.
3. Unary operators MUST replace the top-of-stack value with the computed result.
4. If insufficient operands exist on the stack, the operation MUST be rejected.
5. All operations MUST be evaluated atomically.

---

### Input Parsing and Normalization Policy

**Definition**: Defines how numeric input is interpreted and normalized.

1. Numeric input MUST accept standard decimal and scientific notation.
2. Input MUST be normalized to a canonical floating-point representation before use.
3. Formatting artifacts (e.g., leading zeros) MUST NOT affect numeric value.
4. Invalid numeric formats MUST be rejected.

---

### Stack Capacity Constraint

**Definition**: Defines limits of stack structure.

1. Stack MUST have a fixed maximum capacity.
2. Pushing onto a full stack MUST be rejected.
3. Capacity MUST be consistent across environments.

---

### Error and Rejection Semantics

**Definition**: Defines how invalid actions are handled.

1. Any invalid input or operation MUST be rejected.
2. Rejected actions MUST NOT mutate any system state.
3. Rejection MUST produce a user-visible status message.
4. Feedback MUST be non-modal and must not block further interaction.

---

### State Mutation Policy

**Definition**: Defines guarantees about state transitions.

1. State changes occur only through accepted user actions.
2. Rejected actions MUST NOT mutate any state component.
3. All state transitions MUST be atomic from the user perspective.

---

### History / Reversibility Policy

**Definition**: Defines undo behavior and history tracking.

1. All accepted operand entries MUST be recorded in history.
2. All accepted operator applications MUST be recorded in history.
3. Undo MUST restore the previous stack state exactly.
4. Rejected actions MUST NOT be recorded in history.
5. Reset actions MUST clear history and MUST NOT be undoable.

---

### History Boundary Rules

**Definition**: Defines limits of undo behavior.

1. Undo MUST operate only on recorded actions.
2. Undo beyond available history MUST be rejected or produce a no-op with status feedback.

---

### Interaction and UX Conventions

**Definition**: Defines user interaction and feedback expectations.

1. The system MUST display the full stack to the user.
2. The system MUST display status messages for errors and operations.
3. Input MUST be user-initiated through explicit actions.
4. Feedback MUST be immediate and reflect the current state.

---

### Display and Visibility Constraints

**Definition**: Defines how state is presented.

1. Entire stack MUST be visible or scrollable.
2. Ordering MUST preserve stack semantics.
3. Empty stack MUST be explicitly represented.

---

### Status Lifecycle Policy

**Definition**: Defines how status messages behave over time.

1. Status MUST update immediately after any action.
2. New status MUST overwrite previous status.
3. Neutral state MUST be represented when no active message exists.

---

### Cross-Feature Continuity

**Definition**: Defines assumptions preserved across user story progression.

1. All subsequent user stories operate on the same stack-based evaluation model.
2. All user stories inherit the same numeric policy and rejection semantics.
3. Previously introduced behaviors MUST remain valid and unchanged unless explicitly extended.

---

### Cross-Environment Consistency Constraint

**Definition**: Defines invariants across deployment environments.

1. All numeric operations MUST produce identical results across environments within floating-point tolerance.
2. Stack behavior and limits MUST be identical across environments.
3. Undo and history behavior MUST be identical across environments.

---

## User Stories

### Summary

| #   | User Story                          | Brief Scope                         |
| --- | ----------------------------------- | ----------------------------------- |
| 1   | US1 — Reset Calculator State        | baseline/init/clear state           |
| 2   | US2 — Enter Numeric Operands        | operand entry to stack              |
| 3   | US3 — View Stack and Status         | stack/result/status visibility      |
| 4   | US4 — Apply Additive Operators      | add/sub/neg/abs                     |
| 5   | US5 — Apply Multiplicative Operators| mul/div/inv                         |
| 6   | US6 — Apply Power and Root Operators| sqr/sqrt/pow                        |
| 7   | US7 — Apply Exponential and Log Operators | exp/ln                        |
| 8   | US8 — Undo Reversible Actions       | undo history                        |
| 9   | US9 — Package Desktop Application   | desktop packaging                   |

---

### User Story US1 — Reset Calculator State

Status: planned

#### Description

User can reset the calculator to a clean initial state, clearing all prior data and preparing for a new calculation session.

#### Shared System Semantics References  

- SSS - State Model  
- SSS - State Mutation Policy - (1)  
- SSS - History / Reversibility Policy - (5)

#### Scope  

Defines the ability to reinitialize the calculator state, including stack, input buffer, history, and status, to a baseline empty state. Includes both explicit reset and initialization.

#### Included Behavior  

- Reinitialize the stack to an empty state  
- Clear any active numeric input from the input buffer  
- Clear the full history of prior actions  
- Reset status feedback to a neutral baseline  
- Establish a new calculation session boundary  

#### State Interaction

- Reads: none  
- Mutates: stack, input buffer, history, status  
- Preserves: none  
- Resets: stack, input buffer, history, status  
- Must Not Affect: none  

#### Acceptance Scenarios

1. **Given** a non-empty stack and history, **When** reset is triggered, **Then** all state is cleared  
2. **Given** application start, **When** initialization occurs, **Then** the system matches reset state  

#### Exception Scenarios

- Reset is always valid and MUST NOT be rejected  

---

### User Story US2 — Enter Numeric Operands

Status: planned

#### Description

User can input numeric values and push them onto the stack.

#### Shared System Semantics References  

- SSS - Numeric Policy  
- SSS - Input Parsing and Normalization Policy  
- SSS - Stack Capacity Constraint  
- SSS - Error and Rejection Semantics  
- SSS - State Mutation Policy  

#### Scope  

Defines numeric input acceptance, validation, normalization, and commitment to the stack.

#### Included Behavior  

- Accept a valid numeric value according to parsing and numeric policy  
- Normalize the accepted numeric value  
- Push the normalized value onto the stack  
- Record the action in history  

#### State Interaction

- Reads: input buffer  
- Mutates: stack, input buffer, history  
- Preserves: status  
- Resets: input buffer (post-commit)  
- Must Not Affect: none  

#### Acceptance Scenarios

1. **Given** valid numeric input, **When** committed, **Then** it appears at top of stack  
2. **Given** multiple entries, **When** committed sequentially, **Then** stack order is preserved  

#### Exception Scenarios

1. **Given** invalid numeric format, **When** entered, **Then** it is rejected  
2. **Given** full stack, **When** pushing value, **Then** action is rejected  

---

### User Story US3 — View Stack and Status

Status: planned

#### Description

User can view stack contents and system status.

#### Shared System Semantics References  

- SSS - Interaction and UX Conventions  
- SSS - Display and Visibility Constraints  
- SSS - Status Lifecycle Policy  

#### Scope  

Defines visibility of stack and status.

#### Included Behavior  

- Display full stack with correct ordering  
- Display top-of-stack clearly  
- Display current status message  
- Update display immediately after state change  

#### State Interaction

- Reads: stack, status  
- Mutates: none  
- Preserves: all state  
- Resets: none  
- Must Not Affect: all state  

#### Acceptance Scenarios

1. **Given** populated stack, **When** displayed, **Then** values appear in order  
2. **Given** status update, **When** displayed, **Then** message is visible  

#### Exception Scenarios

- Empty stack MUST display explicit empty state  

---

### User Story US4 — Apply Additive Operators

Status: planned

#### Description

User can apply additive and sign operators.

#### Shared System Semantics References  

- SSS - Operation Evaluation Semantics  
- SSS - Numeric Policy  
- SSS - Error and Rejection Semantics  

#### Scope  

Defines additive and sign transformations.

#### Included Behavior  

- Apply addition to top two stack values  
- Apply subtraction to top two stack values  
- Apply negation to top stack value  
- Apply absolute value to top stack value  

#### State Interaction

- Reads: stack  
- Mutates: stack, history  
- Preserves: input buffer  
- Resets: none  
- Must Not Affect: none  

#### Acceptance Scenarios

1. **Given** two values, **When** add is applied, **Then** result replaces them  
2. **Given** one value, **When** neg is applied, **Then** sign is inverted  

#### Exception Scenarios

1. **Given** insufficient operands, **When** operator is invoked, **Then** it is rejected  

---

### User Story US5 — Apply Multiplicative Operators

Status: planned

#### Description

User can apply multiplicative operators.

#### Shared System Semantics References  

- SSS - Operation Evaluation Semantics  
- SSS - Numeric Policy  
- SSS - Error and Rejection Semantics  

#### Scope  

Defines multiplication, division, and reciprocal operations.

#### Included Behavior  

- Apply multiplication to top two stack values  
- Apply division to top two stack values  
- Apply reciprocal to top stack value  

#### State Interaction

- Reads: stack  
- Mutates: stack, history  
- Preserves: input buffer  
- Resets: none  
- Must Not Affect: none  

#### Acceptance Scenarios

1. **Given** values a and b, **When** mul is applied, **Then** a*b replaces them  
2. **Given** value x, **When** inv is applied, **Then** 1/x replaces it  

#### Exception Scenarios

1. **Given** division by zero, **When** operation applied, **Then** rejected  

---

### User Story US6 — Apply Power and Root Operators

Status: planned

#### Description

User can apply power and root operations.

#### Shared System Semantics References  

- SSS - Operation Evaluation Semantics  
- SSS - Numeric Policy  
- SSS - Error and Rejection Semantics  

#### Scope  

Defines squaring, square root, and exponentiation.

#### Included Behavior  

- Apply squaring  
- Apply square root  
- Apply exponentiation  

#### State Interaction

- Reads: stack  
- Mutates: stack, history  
- Preserves: input buffer  
- Resets: none  
- Must Not Affect: none  

#### Acceptance Scenarios

1. **Given** x, **When** sqr, **Then** x²  
2. **Given** a and b, **When** pow, **Then** a^b  

#### Exception Scenarios

1. Negative sqrt rejected  
2. Invalid exponent rejected  

---

### User Story US7 — Apply Exponential and Log Operators

Status: planned

#### Description

User can apply exponential and logarithmic functions.

#### Shared System Semantics References  

- SSS - Operation Evaluation Semantics  
- SSS - Numeric Policy  
- SSS - Error and Rejection Semantics  

#### Scope  

Defines exp and ln operations.

#### Included Behavior  

- Apply exponential  
- Apply natural logarithm  

#### State Interaction

- Reads: stack  
- Mutates: stack, history  
- Preserves: input buffer  
- Resets: none  
- Must Not Affect: none  

#### Acceptance Scenarios

1. **Given** x, **When** exp, **Then** e^x  
2. **Given** x, **When** ln, **Then** ln(x)  

#### Exception Scenarios

1. ln of non-positive rejected  

---

### User Story US8 — Undo Reversible Actions

Status: planned

#### Description

User can undo previous actions.

#### Shared System Semantics References  

- SSS - History / Reversibility Policy  
- SSS - History Boundary Rules  
- SSS - State Mutation Policy  

#### Scope  

Defines undo of accepted actions.

#### Included Behavior  

- Restore previous stack state  
- Remove last history entry  
- Update status  

#### State Interaction

- Reads: history  
- Mutates: stack, history, status  
- Preserves: input buffer  
- Resets: none  
- Must Not Affect: none  

#### Acceptance Scenarios

1. Undo restores prior state  
2. Multiple undo steps supported  

#### Exception Scenarios

1. Empty history undo rejected or no-op  

---

### User Story US9 — Package Desktop Application

Status: planned

#### Description

User can run the calculator as a desktop application.

#### Shared System Semantics References  

- SSS - Cross-Environment Consistency Constraint  
- SSS - Cross-Feature Continuity  

#### Scope  

Defines packaging and runtime parity.

#### Included Behavior  

- Provide desktop executable  
- Preserve identical behavior  
- Initialize consistent state  

#### State Interaction

- Reads: none  
- Mutates: none  
- Preserves: all runtime semantics  
- Resets: initialization state  
- Must Not Affect: calculator logic  

#### Acceptance Scenarios

1. Desktop behavior matches web  
2. Initialization consistent  

#### Exception Scenarios

- Packaging failures out of scope  

---

## Features

### Summary

| #   | Feature                                | Brief Scope         |
| --- | -------------------------------------- | ------------------- |
| 1   | F1 — Core RPN Calculator MVP           | {US1–US5}           |
| 2   | F2 — Advanced Mathematical Operators   | {US6–US7}           |
| 3   | F3 — Undo Reversible Calculator Actions| {US8}               |
| 4   | F4 — Desktop Application Packaging     | {US9}               |

---

### Feature F1 — Core RPN Calculator MVP

Status: planned

#### Metadata

- ID: F1  
- User stories included: US1, US2, US3, US4, US5  
- Scope: baseline calculator functionality  

#### Specify User Prompt

Create a usable RPN calculator supporting reset, input, display, and core arithmetic operations.

##### Agent Override

###### Shared Definitions, Conventions, and Policies

- SSS - Core Domain Definitions  
- SSS - State Model  
- SSS - Numeric Policy  
- SSS - Operation Evaluation Semantics  
- SSS - Input Parsing and Normalization Policy  
- SSS - Stack Capacity Constraint  
- SSS - Error and Rejection Semantics  
- SSS - State Mutation Policy  
- SSS - Interaction and UX Conventions  
- SSS - Display and Visibility Constraints  
- SSS - Status Lifecycle Policy  
- SSS - Cross-Feature Continuity  

###### User Story Decomposition

| # | User Story | Brief Scope |
|---|----------|------------|
| 1 | US1 | reset |
| 2 | US2 | input |
| 3 | US3 | display |
| 4 | US4 | additive |
| 5 | US5 | multiplicative |

---

### Feature F2 — Advanced Mathematical Operators

Status: planned

#### Metadata

- ID: F2  
- User stories included: US6, US7  
- Scope: advanced math  

#### Specify User Prompt

Extend calculator with power, root, exponential, and logarithmic operations.

##### Agent Override

###### Shared Definitions, Conventions, and Policies

- SSS - Numeric Policy  
- SSS - Operation Evaluation Semantics  
- SSS - Error and Rejection Semantics  
- SSS - Cross-Feature Continuity  

###### User Story Decomposition

| # | User Story | Brief Scope |
|---|----------|------------|
| 1 | US6 | power/root |
| 2 | US7 | exp/log |

---

### Feature F3 — Undo Reversible Calculator Actions

Status: planned

#### Metadata

- ID: F3  
- User stories included: US8  
- Scope: undo  

#### Specify User Prompt

Add undo functionality restoring prior states.

##### Agent Override

###### Shared Definitions, Conventions, and Policies

- SSS - History / Reversibility Policy  
- SSS - History Boundary Rules  
- SSS - State Mutation Policy  
- SSS - Error and Rejection Semantics  

###### User Story Decomposition

| # | User Story | Brief Scope |
|---|----------|------------|
| 1 | US8 | undo |

---

### Feature F4 — Desktop Application Packaging

Status: planned

#### Metadata

- ID: F4  
- User stories included: US9  
- Scope: desktop packaging  

#### Specify User Prompt

Package calculator as a desktop app preserving identical behavior.

##### Agent Override

###### Shared Definitions, Conventions, and Policies

- SSS - Cross-Environment Consistency Constraint  
- SSS - Cross-Feature Continuity  

###### User Story Decomposition

| #   | User Story | Brief Scope |
| --- | ---------- | ----------- |
| 1   | US9        | desktop     |