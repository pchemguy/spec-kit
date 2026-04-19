# Short

Create a browser-based Reverse Polish Notation (RPN) calculator allowing users to perform basic calculations and see results. The system must support starting a new session with an empty stack, applying valid tokens (operands push to stack, unary/binary operators consume operands and push results), inspecting full stack and top-of-stack, rejecting operations with insufficient operands, rejecting invalid tokens and unsupported operators, handling arithmetic domain errors and numeric limits (e.g., division by zero) while preserving state, resetting the calculator to a clean session, and undoing the last accepted stack mutation (rejected tokens do not affect undo history). The system must behave deterministically, and a critical invariant is that on any rejected token or failed operation, the calculator preserves the pre-error stack state.

# Long

Create a browser-based Reverse Polish Notation (RPN) calculator as a greenfield prototype.

The goal is to establish the smallest usable vertical slice that proves the core model works: a user can perform basic calculations using RPN input and see results.

The calculator must support the following user capabilities:

1. Start a new calculation session with an empty stack and ready input state.
2. Apply valid tokens to the stack:
    - numeric operands are pushed onto the stack
    - supported unary and binary operators consume operands from the stack and push results back
3. Inspect the current stack at any time, including full stack contents and top-of-stack value.
4. Reject operations with insufficient operands:
    - if an operator is applied when the stack does not contain enough operands, the operation is rejected
    - the stack must remain unchanged
5. Reject invalid tokens and unsupported operations:
    - malformed numeric input or unknown operators are rejected
    - the stack must remain unchanged
6. Handle arithmetic domain errors and numeric limits:
    - invalid mathematical operations (e.g., division by zero or invalid domain inputs) are rejected
    - the stack must remain unchanged
7. Reset the calculator:
    - the user can clear all state and return to a clean session
8. Undo the last accepted token:
    - the user can revert the most recent accepted stack mutation
    - rejected tokens do not affect undo history

The system must behave deterministically.

A critical invariant across all failure cases:

- on any rejected token or failed operation, the calculator must preserve the pre-error stack state.

The application is browser-based and does not require persistence beyond the current session.

No advanced mathematical functions are required beyond basic unary and binary operations unless necessary to define core behavior.

The specification should focus strictly on behavior and user-visible outcomes, not implementation details.
