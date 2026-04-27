
> [!NOTE] Prompt
> 
> Go through each item in "#### Included Behavior" for each user story. For each item enumerate domain related edge cases/classes. If possible, perform comprehensive enumeration. Assess wether every such edge case/class is covered by defined SSS. Identify any cases/classes that might require further SSS elaboration and propose changes.
 
## Audit result

The current SSS covers the main classes, but it is under-specified in several places that will matter during `/speckit.specify`, especially numeric token syntax, stack capacity, input buffer semantics, equality/approximation expectations, and desktop portability scope. The constitution also requires explicit scope, assumptions, test evidence, and bounded agent guidance, so these ambiguities should be promoted into SSS before the roadmap is treated as stable. 

## US1 — Reset calculator state

### Included Behavior: Clear stack

Edge classes:

| Case/class                                      | Covered by current SSS? | Assessment                                                                         |
| ----------------------------------------------- | ----------------------: | ---------------------------------------------------------------------------------- |
| Empty stack reset                               |                 Partial | Reset is always valid, but idempotence is not explicit.                            |
| Non-empty stack reset                           |                     Yes | Covered by Initialization / Reset Policy.                                          |
| Stack containing results from prior operations  |                     Yes | Covered.                                                                           |
| Stack with maximum capacity, if capacity exists |                      No | Stack capacity is undefined.                                                       |
| Stack after rejected operation                  |                 Partial | Rejection preserves state, but reset after rejection is not explicitly called out. |

Needed SSS elaboration: reset must be idempotent; stack capacity must be defined or explicitly unlimited-for-spec.

### Included Behavior: Clear input buffer

Edge classes:

| Case/class                              | Covered? | Assessment                                                         |
| --------------------------------------- | -------: | ------------------------------------------------------------------ |
| Empty input buffer                      |  Partial | Idempotence not explicit.                                          |
| Valid partial numeric token             |  Partial | Input buffer exists, but token lifecycle is undefined.             |
| Invalid partial numeric token           |       No | Invalid token representation is not defined.                       |
| Input buffer after operator application |       No | Whether operators commit/ignore/clear input buffer is not defined. |

Needed SSS elaboration: define input buffer lifecycle or remove input buffer from domain SSS if the spec should avoid token-entry details.

### Included Behavior: Clear history

Edge classes:

| Case/class                           | Covered? | Assessment                                 |
| ------------------------------------ | -------: | ------------------------------------------ |
| Empty history                        |  Partial | Idempotence not explicit.                  |
| Non-empty history                    |      Yes | Reset clears history.                      |
| History after rejected actions       |      Yes | Rejected actions are not recorded.         |
| Attempt undo immediately after reset |      Yes | Covered by History / Reversibility Policy. |
| Startup initialization history       |      Yes | Startup initialization is not undoable.    |

Needed SSS elaboration: reset creates a hard undo boundary.

## US2 — Enter operands and view the stack

### Included Behavior: Accept numeric input

Edge classes:

| Case/class                           | Covered? | Assessment                                                                  |
| ------------------------------------ | -------: | --------------------------------------------------------------------------- |
| Integer literal                      |  Partial | Numeric values are finite floats, but accepted lexical forms are undefined. |
| Decimal literal                      |  Partial | Same issue.                                                                 |
| Negative literal                     |  Partial | Need explicit distinction between entering `-5` and applying `neg`.         |
| Scientific notation                  |       No | Undefined.                                                                  |
| Leading/trailing whitespace          |       No | Undefined.                                                                  |
| Leading plus sign                    |       No | Undefined.                                                                  |
| Decimal separator comma vs dot       |       No | Undefined.                                                                  |
| Empty input                          |       No | Undefined.                                                                  |
| `NaN`, `Infinity`, `-Infinity`       |      Yes | Rejected by finite policy.                                                  |
| Overflow literal parsing to Infinity |      Yes | Rejected if non-finite.                                                     |
| Underflow to zero/subnormal          |       No | Finite, but not explicitly addressed.                                       |
| `-0`                                 |       No | Floating-point edge with visible/display implications.                      |

Needed SSS elaboration: add **Numeric Input Token Policy**.

### Included Behavior: Push operand to stack

Edge classes:

| Case/class                         | Covered? | Assessment                                                    |
| ---------------------------------- | -------: | ------------------------------------------------------------- |
| Push onto empty stack              |      Yes | Basic stack model covers it.                                  |
| Push onto non-empty stack          |      Yes | Covered.                                                      |
| Push when stack has capacity limit |       No | Capacity undefined.                                           |
| Push after reset                   |      Yes | Reset baseline defined.                                       |
| Push after undo                    |  Partial | Allowed by continuity, but history branching is not explicit. |
| Push duplicate values              |      Yes | No uniqueness constraint implied.                             |
| Push zero / negative zero          |  Partial | Zero accepted, negative zero display/normalization undefined. |

Needed SSS elaboration: define stack capacity and history behavior after undo.

### Included Behavior: Display stack

Edge classes:

| Case/class                       | Covered? | Assessment                                                                |
| -------------------------------- | -------: | ------------------------------------------------------------------------- |
| Empty stack display              |  Partial | Full stack visible, but empty representation undefined.                   |
| Single value                     |      Yes | Covered conceptually.                                                     |
| Multiple values                  |      Yes | Covered conceptually.                                                     |
| Many values requiring scrolling  |       No | Visibility says full stack visible, but not scrolling/overflow semantics. |
| Top-of-stack orientation         |       No | Top is defined conceptually, but visual orientation is not.               |
| Floating-point display format    |       No | Representation rules absent.                                              |
| Long numbers / precision display |       No | Undefined.                                                                |

Needed SSS elaboration: add **Stack Visibility and Numeric Display Policy**.

## US3 — Apply additive and sign-altering operators

### Included Behavior: Binary operations `add`, `sub`

Edge classes:

| Case/class                         | Covered? | Assessment                                                                         |
| ---------------------------------- | -------: | ---------------------------------------------------------------------------------- |
| Exactly two operands               |      Yes | Covered.                                                                           |
| More than two operands             |      Yes | Unaffected stack values preserved.                                                 |
| One or zero operands               |      Yes | Insufficient operands rejected.                                                    |
| Operand order for `sub`            |      Yes | RPN binary convention covers it.                                                   |
| Result is finite                   |      Yes | Accepted.                                                                          |
| Result overflows to non-finite     |      Yes | Rejected.                                                                          |
| Result underflows                  |  Partial | Finite underflow result would be accepted, but not explicit.                       |
| Signed zero from subtraction       |       No | Display/normalization undefined.                                                   |
| Floating-point precision artifacts |  Partial | Standard floating-point semantics, but success criteria may need tolerance policy. |

Needed SSS elaboration: define signed zero and comparison/display policy.

### Included Behavior: Unary operations `neg`, `abs`

Edge classes:

| Case/class            | Covered? | Assessment                                                    |
| --------------------- | -------: | ------------------------------------------------------------- |
| Exactly one operand   |      Yes | Unary operator top-of-stack rule covers it.                   |
| More than one operand |      Yes | Unaffected values preserved.                                  |
| Empty stack           |      Yes | Insufficient operands rejected.                               |
| `neg(0)` / `neg(-0)`  |       No | Signed zero undefined.                                        |
| `abs(-0)`             |       No | Signed zero undefined.                                        |
| `abs(min finite)`     |      Yes | Finite result accepted unless non-finite.                     |
| NaN/infinity operand  |   Mostly | Such values should never exist if Numeric Policy is enforced. |

Needed SSS elaboration: add rule that stack cannot contain non-finite values and define signed-zero normalization.

## US4 — Apply multiplicative operators

### Included Behavior: Multiplication

Edge classes:

| Case/class                                | Covered? | Assessment                         |
| ----------------------------------------- | -------: | ---------------------------------- |
| Operand order                             |      Yes | Binary convention.                 |
| Multiplication by zero                    |      Yes | Valid finite operation.            |
| Negative × positive / negative × negative |      Yes | Standard float semantics.          |
| Overflow to infinity                      |      Yes | Rejected.                          |
| Underflow to zero                         |  Partial | Finite accepted, but not explicit. |
| Signed zero result                        |       No | Undefined.                         |

### Included Behavior: Division

Edge classes:

| Case/class                  | Covered? | Assessment                                    |
| --------------------------- | -------: | --------------------------------------------- |
| `lhs / rhs` order           |      Yes | Binary convention.                            |
| Divide by positive zero     |      Yes | Division by zero rejected.                    |
| Divide by negative zero     |  Partial | Zero policy does not explicitly include `-0`. |
| Zero divided by non-zero    |      Yes | Valid.                                        |
| Negative division           |      Yes | Standard float semantics.                     |
| Overflow/non-finite result  |      Yes | Rejected.                                     |
| Very small non-zero divisor |      Yes | Accepted unless non-finite result.            |

Needed SSS elaboration: define zero equivalence for rejection (`+0` and `-0` both zero).

### Included Behavior: Reciprocal

Edge classes:

| Case/class                      | Covered? | Assessment                                                                |
| ------------------------------- | -------: | ------------------------------------------------------------------------- |
| `inv(x)` for non-zero finite x  |      Yes | Unary rule + numeric policy.                                              |
| `inv(0)`                        |  Partial | Division by zero mentioned, reciprocal zero rejection should be explicit. |
| `inv(-0)`                       |       No | Signed zero undefined.                                                    |
| Very small x producing infinity |      Yes | Non-finite result rejected.                                               |
| Very large x producing zero     |  Partial | Finite accepted, underflow not explicit.                                  |

Needed SSS elaboration: reciprocal zero is a domain-invalid operation.

## US5 — Apply power and root operators

### Included Behavior: `sqr`

Edge classes:

| Case/class                      | Covered? | Assessment                               |
| ------------------------------- | -------: | ---------------------------------------- |
| Positive input                  |      Yes | Unary rule.                              |
| Negative input                  |      Yes | Square is valid.                         |
| Zero / negative zero            |  Partial | Signed zero undefined.                   |
| Large finite input overflowing  |      Yes | Non-finite rejected.                     |
| Small finite input underflowing |  Partial | Finite result accepted but not explicit. |

### Included Behavior: `sqrt`

Edge classes:

| Case/class       |         Covered? | Assessment                                        |
| ---------------- | ---------------: | ------------------------------------------------- |
| Positive input   |              Yes | Valid.                                            |
| Zero             |              Yes | Valid.                                            |
| Negative input   |              Yes | Domain-invalid rejected.                          |
| Negative zero    |               No | Should probably be treated as zero or normalized. |
| Non-finite input | Yes by invariant | Non-finite stack values should not exist.         |

### Included Behavior: `pow`

Edge classes:

| Case/class                                                                             | Covered? | Assessment                                                                 |
| -------------------------------------------------------------------------------------- | -------: | -------------------------------------------------------------------------- |
| Operand order `lhs ^ rhs`                                                              |      Yes | Binary convention.                                                         |
| Positive base, any finite exponent                                                     |  Partial | Usually defined if result finite, but not fully specified.                 |
| Zero base, positive exponent                                                           |  Partial | Should be valid.                                                           |
| Zero base, zero exponent                                                               |       No | Must choose whether `0^0` follows JS `Math.pow` or is rejected.            |
| Zero base, negative exponent                                                           |  Partial | Non-finite/division-like result, but domain-invalid should be explicit.    |
| Negative base, integer exponent                                                        |  Partial | Valid in real arithmetic.                                                  |
| Negative base, non-integer exponent                                                    |       No | Must be rejected as non-real/domain-invalid.                               |
| Negative base with fractional exponent that is mathematically rational odd denominator |       No | Too hard unless explicitly supported; likely reject non-integer exponents. |
| Overflow/non-finite result                                                             |      Yes | Rejected.                                                                  |

Needed SSS elaboration: add **Real-Valued Operator Domain Policy**, especially `pow`.

## US6 — Apply exponential and logarithmic operators

### Included Behavior: `exp`

Edge classes:

| Case/class                               |              Covered? | Assessment                                   |
| ---------------------------------------- | --------------------: | -------------------------------------------- |
| `exp(0)`                                 |                   Yes | Valid.                                       |
| Positive input                           | Yes if finite result. |                                              |
| Large positive input                     |                   Yes | Reject if non-finite.                        |
| Negative input                           |                   Yes | Valid finite result unless underflow.        |
| Very negative input underflowing to zero |               Partial | Finite accepted, but underflow not explicit. |

### Included Behavior: `ln`

Edge classes:

| Case/class                | Covered? | Assessment                                                     |
| ------------------------- | -------: | -------------------------------------------------------------- |
| Positive input            |      Yes | Valid.                                                         |
| Input `1`                 |      Yes | Valid.                                                         |
| Input `0`                 |      Yes | Rejected by exception scenario, but should be SSS domain rule. |
| Input `-0`                |       No | Signed zero undefined.                                         |
| Negative input            |      Yes | Rejected.                                                      |
| Very small positive input |      Yes | Valid if finite result.                                        |

Needed SSS elaboration: domain constraints should be centralized rather than partly in exception scenarios.

## US7 — Undo the last undoable calculator action

### Included Behavior: Undo accepted operand entry

Edge classes:

| Case/class                              | Covered? | Assessment                                |
| --------------------------------------- | -------: | ----------------------------------------- |
| Undo first operand entry                |      Yes | Restores empty stack.                     |
| Undo later operand entry                |      Yes | Restores previous stack.                  |
| Undo after rejected input               |      Yes | Rejected input not recorded.              |
| Undo after reset                        |      Yes | Reset boundary.                           |
| Undo after undo, then enter new operand |       No | History branching/redo absence undefined. |

### Included Behavior: Undo accepted operator application

Edge classes:

| Case/class                       | Covered? | Assessment                                                               |
| -------------------------------- | -------: | ------------------------------------------------------------------------ |
| Undo unary operator              |      Yes | Restores prior state.                                                    |
| Undo binary operator             |      Yes | Restores prior state.                                                    |
| Undo after rejected operator     |      Yes | Rejected action not recorded.                                            |
| Multiple successive undo actions |  Partial | “Immediately previous” implies it, but not explicit.                     |
| Undo action itself recorded?     |       No | Critical: if undo is recorded, repeated undo semantics become ambiguous. |

### Included Behavior: Restore immediately previous undoable state

Edge classes:

| Case/class                             | Covered? | Assessment                                             |
| -------------------------------------- | -------: | ------------------------------------------------------ |
| Restore stack                          |      Yes | History state exists.                                  |
| Restore input buffer                   |  Partial | State model includes it, but buffer lifecycle unclear. |
| Restore feedback/status                |       No | Should status be part of state/history?                |
| Restore after reset boundary           |      Yes | Covered.                                               |
| Restore across feature-added operators |      Yes | General if operator actions are undoable.              |

Needed SSS elaboration: undo must not itself be recorded as undoable unless redo exists; status feedback should not be part of undoable calculator state unless explicitly intended.

### Included Behavior: Preserve reset and startup initialization as non-undoable boundaries

Covered, but improve wording: reset clears history and establishes a new history root.

## US8 — Use the calculator as packaged portable desktop app

### Included Behavior: Launch as portable desktop application

Edge classes:

| Case/class                                     | Covered? | Assessment                                                |
| ---------------------------------------------- | -------: | --------------------------------------------------------- |
| First launch                                   |  Partial | Startup init covered, but packaging expectations are not. |
| Launch after prior desktop use                 |       No | Persistence is unclear.                                   |
| Launch without installation                    |       No | “Portable” undefined.                                     |
| Launch from extracted/self-contained directory |       No | Undefined.                                                |
| Launch on unsupported OS                       |       No | Platform matrix undefined.                                |

### Included Behavior: Preserve calculator behavior from browser app

Covered by Cross-Environment Consistency Constraint.

### Included Behavior: Preserve all calculator semantics

Covered.

### Included Behavior: Desktop-packaged UX without redefining behavior

Partial. Need define whether desktop app may add menus, shortcuts, window chrome, storage, auto-update, file access, etc.

Needed SSS elaboration: add **Desktop Packaging Boundary Policy**.

---

# Proposed SSS changes

Add or revise these sections.

## 1. Numeric Input Token Policy

```markdown
### Numeric Input Token Policy

**Definition**: Defines which user-entered numeric tokens are valid operands.

1. A valid numeric input token MUST parse to a finite floating-point number.
2. Empty input MUST be rejected.
3. Tokens parsing to `NaN`, `Infinity`, or `-Infinity` MUST be rejected.
4. Scientific notation MAY be accepted only if it parses to a finite floating-point number.
5. The decimal separator MUST be `.` unless localization is explicitly specified.
6. Leading and trailing whitespace MUST NOT change the numeric value if accepted by the input mechanism.
7. A leading minus sign is part of numeric input and MUST NOT be confused with the `neg` operator.
```

## 2. Stack Capacity and Visibility Policy

```markdown
### Stack Capacity and Visibility Policy

**Definition**: Defines stack size, ordering, and display expectations.

1. The stack MUST preserve operand order exactly.
2. The stack top MUST be visually identifiable.
3. Empty stack state MUST be visibly distinguishable from a stack containing zero.
4. If a fixed stack capacity is imposed, capacity MUST be explicitly specified.
5. If no fixed stack capacity is specified, stack size MUST be limited only by practical runtime constraints.
6. Stack overflow, if capacity exists, MUST be rejected without state mutation.
7. Stack display MUST support viewing all current stack entries, including through scrolling when necessary.
```

## 3. Numeric Representation and Comparison Policy

```markdown
### Numeric Representation and Comparison Policy

**Definition**: Defines numeric result representation, signed zero handling, and verification expectations.

1. Internal computation MUST use standard finite floating-point semantics.
2. Stack values MUST NOT contain `NaN`, `Infinity`, or `-Infinity`.
3. `+0` and `-0` MUST be treated as zero for all domain checks.
4. The system MAY normalize `-0` to `0` for display and stack storage.
5. Acceptance tests involving non-integer floating-point results MUST use explicitly defined tolerance or representation expectations.
6. Finite underflow results MAY be accepted unless a specific operator domain rule rejects them.
```

## 4. Real-Valued Operator Domain Policy

```markdown
### Real-Valued Operator Domain Policy

**Definition**: Defines shared real-number domain constraints for supported mathematical operators.

1. Operators MUST produce real-valued finite results only.
2. Division by `+0` or `-0` MUST be rejected.
3. Reciprocal of `+0` or `-0` MUST be rejected.
4. `sqrt(x)` MUST reject `x < 0`.
5. `ln(x)` MUST reject `x <= 0`.
6. `pow(lhs, rhs)` MUST reject combinations that do not produce a real-valued finite result.
7. `pow(0, rhs)` MUST reject `rhs <= 0`.
8. `pow(lhs, rhs)` MUST reject `lhs < 0` unless `rhs` is an integer.
9. Any operator result that is non-finite MUST be rejected.
```

## 5. History Branching and Undo State Policy

```markdown
### History Branching and Undo State Policy

**Definition**: Defines how undo history behaves across accepted actions, rejected actions, reset, and undo.

1. Undo history MUST record accepted operand entry actions.
2. Undo history MUST record accepted operator applications.
3. Undo history MUST NOT record rejected actions.
4. Undo history MUST NOT record reset.
5. Undo history MUST be cleared by reset.
6. Undo MUST restore stack and input buffer to the immediately previous undoable state.
7. Undo itself MUST NOT create a new undoable action.
8. After undo, a newly accepted operand entry or operator application MUST continue from the restored state.
9. The system is not required to provide redo unless explicitly specified.
10. Feedback/status messages MUST NOT be treated as undoable calculator state unless explicitly specified.
```

## 6. Desktop Packaging Boundary Policy

```markdown
### Desktop Packaging Boundary Policy

**Definition**: Defines what desktop packaging may and may not change.

1. Desktop packaging MUST preserve all calculator semantics defined for the browser application.
2. Desktop packaging MUST NOT introduce server-centric execution requirements.
3. Portable desktop operation MUST NOT require a separate user-managed backend service.
4. Desktop-specific UI affordances MAY be added only if they do not change calculator semantics.
5. Persistence behavior MUST be explicitly specified before any calculator state is persisted across launches.
6. If persistence is not specified, startup MUST initialize into the reset-equivalent baseline state.
7. Supported operating systems and packaging format MUST be stated in the desktop packaging feature specification.
```

## Recommended edits to existing SSS

The biggest cleanup: move operator-specific domain constraints out of user-story exception scenarios into **Real-Valued Operator Domain Policy**, then let US3–US6 reference that policy.

Also revise **History / Reversibility Policy** to the fuller “History Branching and Undo State Policy”; current wording is too thin for repeated undo and post-undo behavior.
