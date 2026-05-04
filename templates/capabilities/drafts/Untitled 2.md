* **RPN Calculation Workflow** — Users can enter and evaluate expressions using reverse Polish notation.
  Scope signal: stack-based input, operand entry, operation execution, and visible calculation state.

* **Arithmetic Operations** — Users can perform core numeric calculations.
  Scope signal: add, subtract, multiply, divide, negation, inverse, and absolute value.

* **Power and Root Operations** — Users can compute powers, squares, and square roots.
  Scope signal: square, square root, and arbitrary exponentiation.

* **Exponential and Logarithmic Operations** — Users can perform common scientific calculator functions.
  Scope signal: exponential and natural logarithm functions.

* **Undo and Error Recovery** — Users can reverse recent calculator actions to recover from mistakes.
  Scope signal: undoing entries, operations, and stack/state changes.

* **Web App Access** — Users can run the calculator in a browser without installing software.
  Scope signal: browser-based launch, interaction, and responsive web usability.

* **Packaged Desktop Access** — Users can later install and launch the calculator as a desktop application.
  Scope signal: desktop packaging, local app launch, and desktop-style usage expectations.

---
---

Why separate

Power and Root Operations — Users can compute powers, squares, and square roots.
Scope signal: square, square root, and arbitrary exponentiation.
Exponential and Logarithmic Operations — Users can perform common scientific calculator functions.
Scope signal: exponential and natural logarithm functions.

---

Because they represent **different mental models and usage intents** from an end-user perspective—not just different math functions.

Here’s the precise reasoning:

### 1) Distinct user intent clusters

* **Power & Root Operations** are typically perceived as *direct numeric transformations*:

  * “square this”
  * “take the square root”
  * “raise to a power”

These feel like *extensions of arithmetic*.

* **Exponential & Logarithmic Operations** are perceived as *scientific/analytical functions*:

  * “apply exp”
  * “take ln”

These are not usually grouped mentally with basic arithmetic—they signal a shift into **scientific mode**.

---

### 2) UI/UX implications (important for your anchor design)

In most calculators (physical or digital):

* `x²`, `√`, `xʸ` → appear in **primary or near-primary controls**
* `exp`, `ln` → appear in **secondary/scientific panels**

So separating them reflects:

* different **discoverability expectations**
* different **interaction frequency**
* different **user expertise levels**

---

### 3) Cognitive load and learnability

For many users:

* Power/root = *intuitive extensions*
* Exp/ln = *requires domain knowledge*

If you merge them into one anchor, you blur:

* beginner-friendly operations
* advanced mathematical functions

That weakens the “user-recognizable capability” requirement.

---

### 4) Functional cohesion test

A good anchor should answer: *“Would a user reasonably expect these to belong together?”*

* `square`, `sqrt`, `pow` → yes (same conceptual family)
* `ln` with `sqrt` → not really

Even though mathematically related, **they are not used together operationally** in typical calculator workflows.

---

### When you *could* combine them

If your product direction is explicitly:

* “scientific calculator” as a single cohesive mode
* minimal UI distinction between function types

Then a merged anchor like:

> **Advanced Mathematical Functions** — Users can perform higher-order scientific operations.
> Scope signal: powers, roots, exponentials, and logarithms.

…would be justified.

---

### Bottom line

The separation isn’t about math—it’s about:

* **user expectations**
* **interaction patterns**
* **UI grouping**
* **cognitive categories**

If your goal is *user-centric capability modeling*, the split is the safer, more precise abstraction.
