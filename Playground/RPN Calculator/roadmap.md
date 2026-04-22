---
url: https://chatgpt.com/g/g-p-69e6210469388191b8880a8407594f1a-rpn-calculator/c/69e917ff-7b60-83eb-9576-4ab09f528dc1
---

# Roadmap

## Core RPN Calculator with Full Stack and Basic Arithmetic

**Status**: planned

### Specify User Prompt

Create a browser-based Reverse Polish Notation (RPN) calculator that allows users to enter numeric operands, view the full stack at all times, and perform basic arithmetic operations including `add`, `sub`, `neg`, `mul`, and `div`. The calculator must follow standard RPN evaluation rules for operand ordering and stack manipulation. The numeric model must use standard floating-point arithmetic and only allow finite results. Any invalid input or operation (including division by zero or non-finite results) must be rejected without changing the current state. The system must provide non-blocking user feedback via a visible status area rather than modal dialogs.

---

## Scientific Operator Expansion

**Status**: planned

### Specify User Prompt

Extend the existing RPN calculator by adding scientific operators including `sqr`, `sqrt`, `inv`, `pow`, `abs`, `exp`, and `ln`. All operators must follow standard RPN evaluation rules and operate on the stack accordingly. The calculator must continue to use a floating-point numeric model that allows only finite values. Any domain errors or invalid operations (such as square root of a negative number or logarithm of a non-positive number) must be rejected without modifying the current state, and must produce a non-blocking warning in the calculator’s status area.

---

## Undo Last Action

**Status**: planned

### Specify User Prompt

Add an undo capability to the RPN calculator that allows the user to reverse the most recent action. Undo must support reversing both numeric entry (including partially entered numbers) and operator application. When undo is applied, the calculator must restore the exact previous stack and input state. Undo must not apply to reset or clear actions. The behavior must be deterministic and consistent across all supported operations.

---

## Reset / Clear Calculator State

**Status**: planned

### Specify User Prompt

Add the ability for the user to reset the calculator to its initial state. This must clear the entire stack, discard any in-progress numeric input, and restore the calculator to a ready state equivalent to a new session. The reset operation must be explicit and user-triggered, and its effect must be immediately visible in the stack and input state.

---

## Portable Desktop Distribution with Dual-Mode Availability

**Status**: planned

### Specify User Prompt

Provide a portable desktop version of the RPN calculator that runs outside the browser while preserving the same behavior and functionality as the web application. The desktop version must be self-contained and require no installation. The project must remain available as a browser-based web application, and both desktop and web versions must exhibit consistent behavior, including identical calculation results, operator semantics, and user interaction patterns.