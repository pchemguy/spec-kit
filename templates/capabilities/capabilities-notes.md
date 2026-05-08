# GigaChat Template Sensitivity

When using 

```
## Target Description

RPN calculator web app with a packaged desktop version option. The app supports add, sub, neg, mul, div, sqr, sqrt, inv, pow, abs, exp, and ln functions. The app also has the undo capability.

---

```

```markdown
## Capability Decomposition Report

### Capabilities

| Capability | Classification | Scope boundary | Extracted Keywords |
| ---------- | -------------- | -------------- | ------------------ |
| [Capability Name] | [Capability Model Class] | [Brief boundary-oriented scope statement] | [One or more capability signals] |

### Non-Functional and Form-Factor Aspect Classification

| Aspect | Taxonomy Category | User-Facing Relevance | Implementation Separability | Extracted Keywords |
| ------ | ----------------- | --------------------- | --------------------------- | ------------------ |
| [Explicit or strongly implied aspect] | [One or more taxonomy categories] | [Classification] | [Classification] | [One or more capability signals] |
```

GigaChat reproducibly replaces extracted math ops with "math operations". It typically does NOT do this, if

`### Capabilities` -> `### Capability Table`

or if prepending "Capability List" :

```markdown
## Capability Decomposition Report

### Capability List

- **[Capability Name]** — [End-user value / functional intent].
  Scope boundary: [Brief boundary-oriented scope statement].
  Classification: [Capability Model Class].
  Extracted Keywords: [One or more capability signals].

### Capability Table

| Capability | Classification | Scope boundary | Extracted Keywords |
| ---------- | -------------- | -------------- | ------------------ |
| [Capability Name] | [Capability Model Class] | [Brief boundary-oriented scope statement] | [One or more capability signals] |

### Non-Functional and Form-Factor Aspect Classification

| Aspect | Taxonomy Category | User-Facing Relevance | Implementation Separability | Extracted Keywords |
| ------ | ----------------- | --------------------- | --------------------------- | ------------------ |
| [Explicit or strongly implied aspect] | [One or more taxonomy categories] | [Classification] | [Classification] | [One or more capability signals] |
```

---
