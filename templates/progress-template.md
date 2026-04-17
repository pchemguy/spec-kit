# `progress.md` template

```
# [FEATURE TITLE]

Feature: [FEATURE SHORT NAME]
Branch: [FEATURE BRANCH NAME]
Spec Dir: specs/[FEATURE DIR NAME]
Milestone Prefix: [GH MILESTONE PREFIX]
Status: completed | superseded | deprecated
Completed: [YYYY-MM-DD]

## Summary

[3–6 line description of implemented capabilities and behavioral scope]

## References:

- Spec: specs/[FEATURE DIR NAME]/spec.md
- Plan: specs/[FEATURE DIR NAME]/plan.md
- Research: specs/[FEATURE DIR NAME]/research.md
- Data Model: specs/[FEATURE DIR NAME]/data-model.md
- Contracts: specs/[FEATURE DIR NAME]/contracts/
- Tasks: specs/[FEATURE DIR NAME]/tasks.md
- Issue Mapping: specs/[FEATURE DIR NAME]/task-to-issue.md (if available)
  
---

```

# One-shot example

```
# Greenfield RPN Calculator Prototype

Feature: greenfield-proto
Branch: 001-greenfield-proto
Spec Dir: specs/001-greenfield-proto
Milestone Prefix: GF Proto
Status: completed
Completed: 2026-04-15

## Summary

Implements a browser-based RPN calculator with deterministic stack evaluation.
Supports operand push, unary and binary operations, stack inspection, operand-count validation,
token validation, arithmetic domain error handling, reset, and undo of last accepted mutation.

## References

- Spec: specs/001-greenfield-proto/spec.md
- Plan: specs/001-greenfield-proto/plan.md
- Research: specs/001-greenfield-proto/research.md
- Data Model: specs/001-greenfield-proto/data-model.md
- Contracts: specs/001-greenfield-proto/contracts/
- Tasks: specs/001-greenfield-proto/tasks.md
- Issue Mapping: specs/001-greenfield-proto/task-to-issue.md

---

```