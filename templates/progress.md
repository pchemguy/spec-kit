
# Project Progress Memory

This document tracks feature-level implementation history and serves as a structured memory source for agents. It is split into 3 major sections:

1. Agent instructions
2. Entry Format
3. Ledger

---

## 1. Agent Instructions

This file serves as the structured project evolution ledger for both humans and LLM agents.

### Purpose

- Provide a concise, structured summary of implemented features
- Enable fast mapping from high-level feature history to detailed per-feature artifacts
- Support deterministic context loading for agent workflows

### Rules

- Feature ledger entry MUST be created after feature implementation is completed and all tests are green
- Entries MUST be appended to the Ledger section using the Entry Format defined below
- Each governed feature MUST have exactly one canonical entry
- All required template fields MUST be present in every finalized entry that is not a brownfield summary record
- Paths MUST be relative to repository root
- Summary MUST describe implemented behavior only and MUST NOT include speculation, planned work, or aspirational scope
- After a feature entry has been finalized, updates are limited to:
    - factual corrections
    - status transitions to `superseded` or `deprecated`
    - addition of missing artifact references that already exist
- Post-completion updates MUST NOT rewrite implemented scope in a way that changes the historical meaning of the record.

### Brownfield Baseline Handling

When governed feature history does not exist, a `BROWNFIELD SUMMARY` entry MAY be added to capture baseline project state before subsequent governed feature entries are appended.

Such entries SHOULD summarize:

- the known baseline functionality already present in the repository
- any uncertainty about undocumented or out-of-band changes
- the absence of earlier governed feature records, if applicable

### Interpretation Guidance

Agents should use this file as a compact navigation and evolution index:

1. identify the relevant feature entry in the Ledger;
2. read the feature summary and status;
3. follow the referenced artifact paths for detailed context;
4. combine this file with `constitution.md` during initialization.




## 1. Agent Instructions

This file serves as a structured project evolution ledger for both humans and LLM agents.

### Purpose

- Provide a concise, structured summary of implemented features
- Enable fast mapping from high-level feature history to detailed spec artifacts
- Support deterministic context loading for agent workflows

### Rules

- Entries MUST be appended only — existing records MUST NOT be modified except to update `Status`
- Each feature MUST have exactly one entry
- All fields in the template MUST be present
- Paths MUST be relative to repository root
- Summary MUST describe implemented behavior only (no speculation or planned features)

### Status Lifecycle

- `completed` → feature completed and usable
- `superseded` → replaced by a newer feature
- `deprecated` → no longer relevant but retained for history

### Update Requirement

After completing a feature, append a new entry to the Ledger section using the template below.

---

## 2. Entry Format

### Template

```
### [FEATURE TITLE]

Feature: [FEATURE SHORT NAME]
Branch: [FEATURE BRANCH NAME]
Spec Dir: specs/[FEATURE DIR NAME]
Milestone Prefix: [GH MILESTONE PREFIX]
Status: completed | superseded | deprecated
Completed: [YYYY-MM-DD]

#### Summary

[3–6 line description of implemented capabilities and behavioral scope]

#### References:

- Spec: specs/[FEATURE DIR NAME]/spec.md
- Plan: specs/[FEATURE DIR NAME]/plan.md
- Research: specs/[FEATURE DIR NAME]/research.md (if available)
- Data Model: specs/[FEATURE DIR NAME]/data-model.md (if available)
- Contracts: specs/[FEATURE DIR NAME]/contracts/ (if available)
- Tasks: specs/[FEATURE DIR NAME]/tasks.md
- Issue Mapping: specs/[FEATURE DIR NAME]/task-to-issue.md (if available)
  
---

```

### One-shot example

```
### Greenfield RPN Calculator Prototype

Feature: greenfield-proto
Branch: 001-greenfield-proto
Spec Dir: specs/001-greenfield-proto
Milestone Prefix: GF Proto
Status: completed
Completed: 2026-04-15

#### Summary

Implements a browser-based RPN calculator with deterministic stack evaluation.
Supports operand push, unary and binary operations, stack inspection, operand-count validation,
token validation, arithmetic domain error handling, reset, and undo of last accepted mutation.

#### References

- Spec: specs/001-greenfield-proto/spec.md
- Plan: specs/001-greenfield-proto/plan.md
- Research: specs/001-greenfield-proto/research.md
- Data Model: specs/001-greenfield-proto/data-model.md
- Contracts: specs/001-greenfield-proto/contracts/
- Tasks: specs/001-greenfield-proto/tasks.md
- Issue Mapping: specs/001-greenfield-proto/task-to-issue.md

---

```

## 3. Ledger

_(Append feature records following Entry Format Template below.)_

