Create a browser-based Reverse Polish Notation (RPN) calculator allowing users to perform basic calculations and see results.

The system must support:

- starting a new session with an empty stack
- applying valid tokens, where operands push to the stack and unary/binary operators consume operands and push results
- inspecting the full stack and top-of-stack
- rejecting operations with insufficient operands
- rejecting invalid tokens and unsupported operators
- handling arithmetic domain errors and numeric limits such as division by zero while preserving state
- resetting the calculator to a clean session
- undoing the last accepted stack mutation

Global behavioral constraints:

- The system must behave deterministically.
- Critical invariant: on any rejected token or failed operation, the calculator must preserve the exact pre-error stack state.
- Rejected tokens must not affect undo history.

User story decomposition constraints:

1. The following user story set is canonical for the spec.
2. Preserve exactly these user stories in the User Scenarios & Testing section.
3. Do not merge, split, reorder, rename, or re-prioritize these user stories.
4. Preserve the provided Stage and Priority values as part of the story clearly associated metadata.
5. Provide user story details for each story according to the spec-template.
6. Consider story titles being set, but improve descriptions, where possible.
7. Any additional detail must go into acceptance scenarios, edge cases, requirements, assumptions, non-goals, or success criteria, not into changing the story decomposition.

Use exactly this canonical story set:

| #   | Title                                                        | Description                                                                                                      | Stage                 | Priority |
| --- | ------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------- | --------------------- | -------- |
| 1   | Start a New Calculation Session                              | System initializes an empty stack and ready state.                                                               | MVP / tracer bullet   | P1       |
| 2   | Apply Valid Tokens to the Stack                              | User can input operands and apply supported operators, producing correct stack transformations.                  | MVP / tracer bullet   | P1       |
| 3   | Inspect Current Stack                                        | User can view the full stack and/or top-of-stack at any time.                                                    | MVP / tracer bullet   | P1       |
| 4   | Reject Operations With Insufficient Operands                 | System rejects operations that cannot be applied due to insufficient operands, preserving state.                 | MVP / tracer bullet   | P1       |
| 5   | Reject Invalid Tokens and Unsupported Operations             | System rejects malformed operands and unknown operators without altering the stack.                              | correctness hardening | P2       |
| 6   | Handle Arithmetic Domain Errors and Numeric Limits Correctly | System detects and handles invalid mathematical operations and numeric overflow/limits without corrupting state. | correctness hardening | P2       |
| 7   | Reset Calculator                                             | User can clear all state and restart from a clean session.                                                       | convenience           | P3       |
| 8   | Undo the Last Accepted Token                                 | User can revert the most recent accepted stack mutation.                                                         | convenience           | P3       |

Specification expectations:

- Focus on what users need and why.
- Avoid implementation details.
- Make requirements testable and unambiguous.
- Define edge cases explicitly.
- Keep success criteria measurable and technology-agnostic.
- Preserve the canonical story set exactly as given.
