---
urls:
  - https://chatgpt.com/g/g-p-69de610325f08191aaf60c2de8f32282/c/69e139f9-f2f0-838d-9ccf-d3621e78eeab
  - https://chatgpt.com/c/69e5b2b1-c950-83eb-b13c-8bf09d5e5d53
  - https://chatgpt.com/g/g-p-69e6210469388191b8880a8407594f1a-rpn-calculator/c/69e6254f-c084-83eb-a38f-908afc14da4d
  - https://chatgpt.com/g/g-p-69e6210469388191b8880a8407594f1a-rpn-calculator/c/69e7d75f-7440-8392-b5fb-7e22d37872c8
---

## specify

Create a browser-based Reverse Polish Notation (RPN) calculator allowing users to perform basic calculations and see results.

### Supported Operators

The calculator MUST support the following operators:

- additive: `add`, `sub`, `neg`
- multiplicative: `mul`, `div`
- power: `sqr`, `sqrt`

### Operand Order Convention

For binary operators, the calculator MUST interpret the top stack value as the right-hand operand and the next value below it as the left-hand operand following the standard RPN evaluation convention:

`push operands → apply operator → pop rhs first, then lhs → compute lhs op rhs`

That is

- `rhs := stack[top]` (right-hand operand - stack top)
- `lhs := stack[top - 1]` (left-hand operand - the second element from the top of the stack)

So:

- `a b add` => `a + b`
- `a b sub` => `a - b`
- `a b mul` => `a * b`
- `a b div` => `a / b`

### Numeric policy — finite real model with rejection

The calculator MUST accept and produce only finite numeric values. Any token application that would produce a non-finite result MUST be rejected and MUST leave the stack unchanged. 

- calculator accepts only finite numeric values
- `NaN`, `+Infinity`, `-Infinity` are not allowed results
- any operation yielding a non-finite value is rejected

### Normative execution rule

For any accepted operator token:

1. validate required operand count;
2. read operands in defined RPN order;
3. validate operator domain constraints;
4. compute the result;
5. if computation succeeds, replace consumed operands with result;
6. if any validation or computation rule fails, reject the token and leave stack unchanged

### Required error categories

At the operator-contract level, at least the following failure categories must be included:

| Category                   | Description                                                                                                         | Target                                                                                   |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| Arity rejection            | The stack does not contain enough operands.                                                                         | - all unary operators when stack is empty<br>- all binary operators when stack depth < 2 |
| Token validation rejection | The token is not a supported operator and is not a valid numeric operand.                                           | - malformed numeric input<br>- unknown token                                             |
| Domain rejection           | The operator is defined syntactically but not mathematically for the provided operand values.                       | - `div` with divisor `0`<br>- `sqrt` with negative input                                 |
| Numeric-result rejection   | The operation produces a value outside the supported numeric model, or a non-finite result if those are disallowed. | - all operators, depending on numeric model                                              |

### Capabilities

1. Reset the calculator:
    - the user can clear all state and return to a clean session
2. Apply valid numeric operands to the stack:
    - capability
        - a valid numeric operand token is pushed onto the stack
    - exception behavior
        - invalid numeric input is rejected
        - a valid input is rejected if the stack is full
3. Apply arithmetic operators:
    - capability
        - the calculator must support `add`, `sub`, `neg`, `mul`, and `div`
        - accepted operator application produces the mathematically correct result
    - exception behavior
        - if the stack does not contain enough operands, the operator is rejected
        - division by zero is rejected
        - operations that would produce non-finite numeric results are rejected
4. Apply power operators:
    - capability
        - the calculator must support `sqr` and `sqrt`
        - accepted operator application produces the mathematically correct result
    - exception behavior
        - if the stack does not contain enough operands, the operator is rejected
        - negative input to `sqrt` is rejected
        - operations that would produce non-finite numeric results are rejected
5. Inspect the current stack state.
6. Undo the last accepted token:
    - capability
        - the user can revert the most recent accepted stack mutation
    - state invariants
        - rejected tokens do not affect undo history

#### Global Rules

- Operator arity
    - Unary operators consume one operand and push one result.
    - Binary operators consume two operands and push one result.
- Operand order
    - Binary operators must use standard RPN operand order.
- State invariants
    - Any rejected token application MUST leave the stack state unchanged.
- Initialization invariant
    - On system initialization, the calculator MUST be in the same state as after a reset operation.


### Agent Override

#### Global behavioral constraints

- The system must behave deterministically.
- Critical invariant: on any rejected token or failed operation, the calculator must preserve the exact pre-error stack state.
- Rejected tokens must not affect undo history.

#### User story decomposition constraints

1. The following user story set is canonical for the spec.
2. Preserve exactly these user stories in the User Scenarios & Testing section.
3. Do not merge, split, reorder, rename, or re-prioritize these user stories.
4. Preserve the provided Stage and Priority values as part of the story clearly associated metadata.
5. Provide user story details for each story according to the spec-template.
6. Consider story titles being set, but improve descriptions, where possible.
7. Any additional detail must go into acceptance scenarios, edge cases, requirements, assumptions, non-goals, or success criteria, not into changing the story decomposition.

#### User story decomposition

Use exactly this canonical story set:

| #   | Title                                     | Description                                                                                         | Stage               | Priority |
| --- | ----------------------------------------- | --------------------------------------------------------------------------------------------------- | ------------------- | -------- |
| 1   | Reset calculator                          | User can clear all state and restart from a clean session.                                          | MVP / tracer bullet | P1       |
| 2   | Apply valid numeric operands to the stack | User can input valid numeric operands and have them pushed onto the stack.                          | MVP / tracer bullet | P1       |
| 3   | Apply arithmetic operators                | User can apply `add`, `sub`, `neg`, `mul`, and `div`, producing correct stack transformations.      | MVP / tracer bullet | P1       |
| 4   | Apply power operators                     | User can apply `sqr` and `sqrt`, producing correct stack transformations when mathematically valid. | MVP / tracer bullet | P1       |
| 5   | Inspect the current stack state.          | User can view the full stack and/or top-of-stack at any time.                                       | MVP / tracer bullet | P1       |
| 6   | Undo the Last Accepted Token              | User can revert the most recent accepted stack mutation.                                            | convenience         | P2       |

#### Specification expectations

- Focus on what users need and why.
- Avoid implementation details.
- Make requirements testable and unambiguous.
- Define edge cases explicitly.
- Keep success criteria measurable and technology-agnostic.
- Preserve the canonical story set exactly as given.

## plan

### Agent Override


##### Operator Contract Table (Research? Contracts?)

| Operator | Class          | Arity | Stack Effect  | Definition     | Rejection Conditions                                                     |
| -------- | -------------- | ----- | ------------- | -------------- | ------------------------------------------------------------------------ |
| `add`    | additive       | 2     | pop 2, push 1 | `a b -> a+b`   | insufficient operands; numeric overflow/invalid result                   |
| `sub`    | additive       | 2     | pop 2, push 1 | `a b -> a-b`   | insufficient operands; numeric overflow/invalid result                   |
| `neg`    | additive       | 1     | pop 1, push 1 | `a -> -a`      | insufficient operands; numeric overflow/invalid result                   |
| `mul`    | multiplicative | 2     | pop 2, push 1 | `a b -> a*b`   | insufficient operands; numeric overflow/invalid result                   |
| `div`    | multiplicative | 2     | pop 2, push 1 | `a b -> a/b`   | insufficient operands; division by zero; numeric overflow/invalid result |
| `sqr`    | power          | 1     | pop 1, push 1 | `a -> a^2`     | insufficient operands; numeric overflow/invalid result                   |
| `sqrt`   | power          | 1     | pop 1, push 1 | `a -> sqrt(a)` | insufficient operands; negative input; numeric overflow/invalid result   |


