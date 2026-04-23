---
url: https://chatgpt.com/g/g-p-69e6210469388191b8880a8407594f1a-rpn-calculator/c/69e9987a-5974-83eb-a797-60c849059d3a
---

# Roadmap

## PREAMBLE: Shared Calculator Semantics

The following rules apply to all planned features unless a feature explicitly narrows them without contradicting them.

### Numeric Policy

* The calculator operates on standard floating-point numeric values.
* Only finite numeric values are valid calculator state.
* Any action that would produce `NaN`, positive infinity, or negative infinity is rejected.
* Domain-invalid mathematical operations are rejected.
* Rejected numeric outcomes do not alter calculator state.

### Operand Order Convention

For every binary operator:

* the top stack value is the right-hand operand;
* the next value below the top is the left-hand operand;
* evaluation uses the form `lhs op rhs`.

Examples:

* `a b add` means `a + b`
* `a b sub` means `a - b`
* `a b mul` means `a * b`
* `a b div` means `a / b`
* `a b pow` means `a ^ b`

### Rejection and Warning Policy

* Any rejected action leaves calculator state unchanged.
* Rejections must produce a user-visible warning.
* Warnings must be presented through a non-modal status mechanism.
* Rejections must not use modal dialogs or popups.

### State Model Assumptions

Calculator state may include, as applicable to the feature being specified:

* stack contents;
* editable numeric input;
* status / warning state;
* undo history.

A feature must explicitly define which parts of calculator state it reads, mutates, preserves, or resets.

### Undo Policy Baseline

* Undo applies only to successful state-changing actions.
* Rejected actions are not undoable.
* Clear / reset is not undoable.

### Delivery Consistency Constraint

Any packaged desktop form of the calculator must preserve the same calculator semantics as the web app, including:

* numeric policy;
* operator behavior;
* rejection behavior;
* warning behavior;
* undo behavior.

## Feature 1 — Start and Reset a Calculator Session

**Status:** planned

##### Specify User Prompt

Create a browser-based Reverse Polish Notation (RPN) calculator feature that starts a new calculator session in a well-defined initial state and provides a clear / reset action that returns the calculator to that same state. The feature must present a visible empty stack, a ready state for numeric entry, and a defined initial status state. Specify exactly which parts of calculator state are initialized and which parts are reset by clear / reset, including stack, editable input, status state, and undo history where applicable. Define the user-visible behavior, state transitions, acceptance criteria, and rejection behavior for session start and clear / reset semantics only, consistent with the shared calculator semantics. Clear / reset must not be undoable.

## Feature 2 — Enter and Submit Numeric Operands Through Editable Input

**Status:** planned

##### Specify User Prompt

Create a browser-based Reverse Polish Notation (RPN) calculator feature that lets the user construct a numeric token through editable input and submit a valid number onto the stack. The feature must define the full user-visible input-editing behavior before submission, including editable intermediate states, what constitutes a valid committed number, submission behavior, post-submission input state, and resulting stack transformation. It must also define rejection behavior for malformed, incomplete, or otherwise invalid numeric input without altering calculator state, consistent with the shared numeric and warning policies.

## Feature 3 — Apply Additive and Sign-Related Transforms

**Status:** planned

##### Specify User Prompt

Create a browser-based Reverse Polish Notation (RPN) calculator feature supporting the operators `add`, `sub`, `neg`, and `abs`. The feature must apply the shared operand-order convention for binary operators and define top-of-stack behavior for unary operators. Specify the exact stack transformations, user-visible results, and acceptance criteria for each operator. The feature must also define rejection behavior for insufficient operands and any invalid numeric outcomes under the shared numeric policy, ensuring rejected operations leave calculator state unchanged and produce a user-visible non-modal warning.

## Feature 4 — Apply Multiplicative Operators

**Status:** planned

##### Specify User Prompt

Create a browser-based Reverse Polish Notation (RPN) calculator feature supporting the operators `mul`, `div`, and `inv`. The feature must apply the shared operand-order convention for binary operators and define top-of-stack behavior for unary reciprocal. Specify the exact stack transformations, user-visible results, and acceptance criteria for each operator. The feature must also define rejection behavior for insufficient operands, division by zero, reciprocal of zero, and any invalid numeric outcomes under the shared numeric policy, ensuring rejected operations leave calculator state unchanged and produce a user-visible non-modal warning.

## Feature 5 — Apply Power-Extension Operations

**Status:** planned

##### Specify User Prompt

Create a browser-based Reverse Polish Notation (RPN) calculator feature supporting the operators `sqr`, `sqrt`, `pow`, `exp`, and `ln`. The feature must apply the shared operand-order convention for binary power and define top-of-stack behavior for unary operators. Specify the exact stack transformations, user-visible results, and acceptance criteria for each operator. The feature must also define rejection behavior for insufficient operands, invalid mathematical domains, and any invalid numeric outcomes under the shared numeric policy, ensuring rejected operations leave calculator state unchanged and produce a user-visible non-modal warning.

## Feature 6 — Undo the Last Successful State-Changing Action

**Status:** planned

##### Specify User Prompt

Create a browser-based Reverse Polish Notation (RPN) calculator feature that allows the user to undo the most recent successful state-changing action. The feature must define exactly which successful actions are undoable, what state is restored when undo succeeds, how repeated undo behaves, and what happens when undo is unavailable. The feature must remain consistent with the shared undo baseline: rejected actions are not undoable and clear / reset is not undoable. Define the user-visible behavior, acceptance criteria, and rejection behavior so that unavailable undo does not corrupt calculator state and instead produces a user-visible non-modal warning.

## Feature 7 — Deliver a Portable Desktop-Packaged Application

**Status:** planned

##### Specify User Prompt

Create a feature that delivers the browser-based Reverse Polish Notation calculator as a portable desktop-packaged application while preserving the web app as the primary pure application. The feature must define the required user-visible behavior of the packaged desktop version, the portability expectations, and the constraint that packaging must require only minimal adjustments to the pure web app. Define acceptance criteria that ensure calculator semantics remain consistent between web and desktop delivery modes, including numeric behavior, operator behavior, rejection behavior, warning behavior, and undo behavior.
