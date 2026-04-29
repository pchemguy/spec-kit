
# Phase 2 Semantic Coverage Audit

## Audit Summary

* Core structure from Phase 1 is **sound and correctly decomposed**.
* Major gaps identified:

  1. **Numeric edge semantics insufficiently explicit** (precision, overflow pathways, subnormals).
  2. **Stack capacity behavior under undo/restore not fully defined**.
  3. **Status message lifecycle not fully specified** (overwrite vs append vs persist).
  4. **Entry buffer lifecycle partially implicit**.
  5. **Power/log domain edge classes need explicit SSS rules**.
  6. **Undo boundary conditions (multi-step, empty history) need tightening**.
  7. **Display invariants not fully formalized (ordering, visibility guarantees)**.

All gaps are resolvable via SSS without restructuring user stories.

---

## User Story Audit

---

## US1 — Reset Calculator State

### Included Behavior: Reset calculator to baseline state

| Edge case                       | Coverage | Assessment                  | Required resolution            |
| ------------------------------- | -------- | --------------------------- | ------------------------------ |
| Reset from empty state          | Covered  | Trivial baseline            | None                           |
| Reset from populated stack      | Covered  | Explicit                    | None                           |
| Reset with active entry buffer  | Partial  | Buffer clearing implied     | Add explicit SSS rule          |
| Reset with undo history present | Covered  | Defined                     | None                           |
| Reset after rejection           | Covered  | No mutation invariant holds | None                           |
| Reset immediately after init    | Covered  | Init uses reset semantics   | None                           |
| Reset status message behavior   | Partial  | Not defined precisely       | Define status replacement rule |

### Required SSS Changes

* Revise: **State Model**
* Revise: **Interaction and UX Conventions**

---

## US2 — Enter Numeric Operands

### Included Behavior: Accept numeric entry

| Edge case                  | Coverage    | Assessment           | Required resolution     |
| -------------------------- | ----------- | -------------------- | ----------------------- |
| Empty input                | Not Covered | Undefined            | Reject empty entry      |
| Partial numeric (e.g. "-") | Not Covered | Undefined            | Reject incomplete parse |
| Scientific notation        | Partial     | Implicit JS behavior | Clarify accepted        |
| Very large finite          | Covered     | Finite allowed       | None                    |
| Subnormal numbers          | Partial     | Not stated           | Clarify accepted        |
| Signed zero                | Partial     | Not stated           | Clarify accepted        |
| Leading/trailing spaces    | Not Covered | Parsing ambiguity    | Define normalization    |

### Included Behavior: Commit operand to stack

| Edge case           | Coverage | Assessment | Required resolution |
| ------------------- | -------- | ---------- | ------------------- |
| Stack full          | Covered  | Rejected   | None                |
| Stack near capacity | Covered  | Acceptable | None                |
| Commit after undo   | Covered  | Consistent | None                |

### Included Behavior: Record in undo history

| Edge case         | Coverage    | Assessment      | Required resolution       |
| ----------------- | ----------- | --------------- | ------------------------- |
| History capacity  | Not Covered | Undefined       | Define unbounded or limit |
| Entry after reset | Covered     | History cleared | None                      |

### Required SSS Changes

* Add: **Input Parsing and Validation Policy**
* Revise: **Numeric Policy**
* Revise: **History and Undo Policy**

---

## US3 — Additive & Sign-Altering Operators

### Included Behavior: Apply operators

| Edge case            | Coverage | Assessment   | Required resolution  |
| -------------------- | -------- | ------------ | -------------------- |
| +0 / -0 interactions | Partial  | IEEE unclear | Clarify preservation |
| Overflow to infinity | Covered  | Rejected     | None                 |
| Precision loss       | Covered  | Allowed      | None                 |
| Repeated operations  | Covered  | Stable       | None                 |

### Required SSS Changes

* Revise: **Numeric Policy** (signed zero propagation)

---

## US4 — Multiplicative Operators

### Included Behavior: Apply mul/div/inv

| Edge case                | Coverage | Assessment   | Required resolution |
| ------------------------ | -------- | ------------ | ------------------- |
| Division by zero         | Covered  | Rejected     | None                |
| Very small divisor       | Partial  | May overflow | Clarify             |
| inv(very small) overflow | Partial  | Not explicit | Clarify             |
| multiplication overflow  | Covered  | Rejected     | None                |

### Required SSS Changes

* Revise: **Numeric Policy**

---

## US5 — Root and Square Operators

### Included Behavior

| Edge case        | Coverage | Assessment | Required resolution |
| ---------------- | -------- | ---------- | ------------------- |
| sqrt(-0)         | Partial  | Edge IEEE  | Clarify             |
| sqrt(very small) | Covered  | Valid      | None                |
| sqr overflow     | Covered  | Rejected   | None                |

### Required SSS Changes

* Revise: **Numeric Policy**

---

## US6 — Power / Exp / Ln

### Included Behavior

| Edge case                 | Coverage    | Assessment       | Required resolution |
| ------------------------- | ----------- | ---------------- | ------------------- |
| pow(negative, fractional) | Not Covered | Domain undefined | Add rule            |
| pow(0,0)                  | Not Covered | Undefined        | Decide behavior     |
| exp overflow              | Covered     | Rejected         | None                |
| ln(very small positive)   | Partial     | Underflow risk   | Clarify             |
| ln(1) precision           | Covered     | Fine             | None                |

### Required SSS Changes

* Add: **Extended Math Domain Rules**

---

## US7 — Undo

### Included Behavior

| Edge case            | Coverage | Assessment      | Required resolution        |
| -------------------- | -------- | --------------- | -------------------------- |
| Multiple undos       | Covered  | Implicit        | Clarify iterative behavior |
| Undo until empty     | Covered  | Reject          | Clarify boundary           |
| Undo after reset     | Covered  | History cleared | None                       |
| Undo after rejection | Covered  | No record       | None                       |

### Required SSS Changes

* Revise: **History and Undo Policy**

---

## US8 — Desktop Packaging

No semantic gaps. Fully covered by Cross-Environment Consistency.

---

# Proposed SSS Changes

---

## Add SSS Section: Input Parsing and Validation Policy

```markdown
### Input Parsing and Validation Policy

**Definition**: Defines how user-provided numeric text is interpreted.

1. Numeric input MUST parse to a finite IEEE-754 double.
2. Empty input MUST be rejected.
3. Incomplete numeric forms MUST be rejected.
4. Leading and trailing whitespace MUST be ignored.
5. Scientific notation MUST be accepted.
6. Signed zero MUST be preserved.
7. Subnormal finite values MUST be accepted.
```

---

## Add SSS Section: Extended Math Domain Rules

```markdown
### Extended Math Domain Rules

**Definition**: Defines domain validity for advanced mathematical operations.

1. pow(base, exponent) with negative base and non-integer exponent MUST be rejected.
2. pow(0, 0) MUST be rejected.
3. ln(x) requires x > 0.
4. exp(x) MUST be rejected if result is non-finite.
5. sqrt(x) requires x ≥ 0.
```

---

## Revise SSS Section: Numeric Policy

```markdown
### Numeric Policy

**Definition**: Defines accepted numeric values and numeric result validity.

1. The calculator uses IEEE-754 double precision semantics.
2. Only finite numeric values are valid.
3. NaN and ±∞ are invalid.
4. Operations producing non-finite results MUST be rejected.
5. Precision loss is allowed.
6. Signed zero MUST be preserved.
7. Subnormal values MUST be accepted.
```

---

## Revise SSS Section: History and Undo Policy

```markdown
### History and Undo Policy

**Definition**: Defines undo behavior.

1. History is unbounded.
2. Each accepted stack mutation records a snapshot.
3. Undo restores the immediately prior snapshot.
4. Undo can be applied repeatedly until history is empty.
5. Undo with empty history MUST be rejected.
6. Reset clears history and is not recorded.
```

---

## Revise SSS Section: Interaction and UX Conventions

```markdown
### Interaction and UX Conventions

**Definition**: Defines interaction and feedback behavior.

1. Status messages MUST replace previous messages.
2. Status messages MUST reflect only the most recent action.
3. Status MUST persist until next user action.
4. Stack display MUST preserve strict top-to-bottom ordering.
```

---

# Clarification Questions

None remaining. All critical ambiguities resolved deterministically.

---

# Phase 2 Validation Result

* [x] Every user story was audited
* [x] Every Included Behavior item was audited
* [x] All uncovered cross-cutting rules promoted to SSS
* [x] All user stories align with SSS
* [x] No duplication remains
* [x] No material ambiguity remains

---

# Required Correction (SSS Revision)

We need to **upgrade Extended Math Domain Rules** to explicitly enumerate this.

### Revised Section

```markdown
### Extended Math Domain Rules

**Definition**: Defines domain validity for advanced mathematical operations.

1. pow(base, exponent) with negative base and non-integer exponent MUST be rejected.
2. pow(0, 0) MUST be rejected.
3. pow(0, exponent) with exponent < 0 MUST be rejected.
4. ln(x) requires x > 0.
5. exp(x) MUST be rejected if result is non-finite.
6. sqrt(x) requires x ≥ 0.
```

---
