---
urls:
  - https://chatgpt.com/g/g-p-69de610325f08191aaf60c2de8f32282/c/69e139f9-f2f0-838d-9ccf-d3621e78eeab
  - https://chatgpt.com/c/69e5b2b1-c950-83eb-b13c-8bf09d5e5d53
  - https://chatgpt.com/g/g-p-69e6210469388191b8880a8407594f1a-rpn-calculator/c/69e6254f-c084-83eb-a38f-908afc14da4d
---

Create a browser-based Reverse Polish Notation (RPN) calculator allowing users to perform basic calculations and see results.

The calculator must support the following capabilities.

### Capabilities

1. Start a new calculation session with an empty stack and ready input state.
2. Apply valid tokens to the stack:
    - numeric operands are pushed onto the stack
3. Apply arithmetic operators:
    - operators `add`, `sub`, `neg`, `mul`, and `div` produce correct stack transformations and numeric results.
4. Apply power operators:
    - operators `sqr` and `sqrt` produce correct stack transformations and numeric results when mathematically valid.
5. Inspect the current stack at any time, including full stack contents and top-of-stack value.
6. Reject operations with insufficient operands:
    - if an operator is applied when the stack does not contain enough operands, the operation is rejected
    - the stack must remain unchanged
7. Reject invalid tokens and unsupported operations:
    - malformed numeric input or unknown operators are rejected
    - the stack must remain unchanged
8. Handle arithmetic domain errors and numeric limits:
    - invalid mathematical operations (e.g., division by zero or invalid domain inputs) are rejected
    - the stack must remain unchanged
9. Reset the calculator:
    - the user can clear all state and return to a clean session
10. Undo the last accepted token:
    - the user can revert the most recent accepted stack mutation
    - rejected tokens do not affect undo history

## Agent Override

### Global behavioral constraints

- The system must behave deterministically.
- Critical invariant: on any rejected token or failed operation, the calculator must preserve the exact pre-error stack state.
- Rejected tokens must not affect undo history.

### User story decomposition constraints

1. The following user story set is canonical for the spec.
2. Preserve exactly these user stories in the User Scenarios & Testing section.
3. Do not merge, split, reorder, rename, or re-prioritize these user stories.
4. Preserve the provided Stage and Priority values as part of the story clearly associated metadata.
5. Provide user story details for each story according to the spec-template.
6. Consider story titles being set, but improve descriptions, where possible.
7. Any additional detail must go into acceptance scenarios, edge cases, requirements, assumptions, non-goals, or success criteria, not into changing the story decomposition.

### User story decomposition

Use exactly this canonical story set:

| #   | Title                                                        | Description                                                                                         | Stage                 | Priority |
| --- | ------------------------------------------------------------ | --------------------------------------------------------------------------------------------------- | --------------------- | -------- |
| 1   | Start a New Calculation Session                              | System initializes an empty stack and ready state.                                                  | MVP / tracer bullet   | P1       |
| 2   | Push Valid Numeric Operands                                  | User can input valid numeric operands and have them pushed onto the stack.                          | MVP / tracer bullet   | P1       |
| 3   | Apply Arithmetic Operators                                   | User can apply `add`, `sub`, `neg`, `mul`, and `div`, producing correct stack transformations.      | MVP / tracer bullet   | P1       |
| 4   | Apply Power Operators                                        | User can apply `sqr` and `sqrt`, producing correct stack transformations when mathematically valid. | MVP / tracer bullet   | P1       |
| 5   | Inspect Current Stack                                        | User can view the full stack and/or top-of-stack at any time.                                       | MVP / tracer bullet   | P1       |
| 6   | Reject Operations With Insufficient Operands                 | System rejects operations that cannot be applied due to insufficient operands, preserving state.    | correctness hardening | P1       |
| 7   | Reject Invalid Tokens and Unsupported Operations             | System rejects malformed operands and unknown operators without altering the stack.                 | correctness hardening | P2       |
| 8   | Handle Arithmetic Domain Errors and Numeric Limits Correctly | System rejects mathematically invalid or non-finite results without corrupting state.               | correctness hardening | P2       |
| 9   | Reset Calculator                                             | User can clear all state and restart from a clean session.                                          | convenience           | P3       |
| 10  | Undo the Last Accepted Token                                 | User can revert the most recent accepted stack mutation.                                            | convenience           | P3       |

### Specification expectations

- Focus on what users need and why.
- Avoid implementation details.
- Make requirements testable and unambiguous.
- Define edge cases explicitly.
- Keep success criteria measurable and technology-agnostic.
- Preserve the canonical story set exactly as given.
