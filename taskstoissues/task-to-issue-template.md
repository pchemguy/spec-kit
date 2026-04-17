# Task to Issue Template

## `task-to-issue.md` Output Template

```
---
Feature: [FEATURE_NAME]
Repository: [OWNER/REPO]
Remote: [https://github.com/OWNER/REPO.git]
---

## Milestones

| #   | Title                                                              |
| --- | ------------------------------------------------------------------ |
| 1   | [GH MILESTONE PREFIX] [GH MILESTONE NUMBER] - [GH MILESTONE FOCUS] |
| 2   | [GH MILESTONE PREFIX] [GH MILESTONE NUMBER] - [GH MILESTONE FOCUS] |
| ... | ...                                                                |
| N   | [GH MILESTONE PREFIX] [GH MILESTONE NUMBER] - [GH MILESTONE FOCUS] |

## Task Mapping

| Task          | GitHub Issue # | Milestone Title   | Labels   |
| ------------- | -------------: | ----------------- | -------- |
| [TASK_NUMBER] | [ISSUE_NUMBER] | [MILESTONE_TITLE] | [LABELS] |

```

## Notes

## One-Shot Example

```
---
Feature: greenfield-proto
Repository: xyz/RPNCalc
Remote: [https://github.com/xyz/RPNCalc.git]
---

## Milestones 

| #   | Name                                                                          |
| --- | ----------------------------------------------------------------------------- |
| 1   | GF Proto 1 - Setup                                                            |
| 2   | GF Proto 2 - Foundational                                                     |
| 3   | GF Proto 3 - US1 Start a New Calculation Session (MVP)                        |
| 4   | GF Proto 4 - US2 Apply Valid Tokens to the Stack (MVP Core Execution)         |
| 5   | GF Proto 5 - US3 Inspect Current Stack (MVP Observability)                    |
| 6   | GF Proto 6 - US4 Reject Operations With Insufficient Operands (MVP Stability) |
| 7   | GF Proto 7 - US5 Reject Invalid Tokens & Unsupported Operations               |
| 8   | GF Proto 8 - US6 Handle Arithmetic Domain Errors & Numeric Limits             |
| 9   | GF Proto 9 - US7 Reset Calculator                                             |
| 10  | GF Proto 10 - US8 Undo Last Accepted Token                                    |
| 11  | GF Proto 11 - Documentation & Developer Experience                            |

## Task Mapping

| Task | GitHub Issue # | Milestone                                                                     | Labels                                                                                                                                                     |
| ---- | -------------: | ----------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| T001 |         1      | GF Proto 1 - Setup                                                            | `feature:greenfield-proto`, `phase:setup`, `type:setup`, `type:config`, `priority:p1`, `risk:blocking`                                                     |
| T002 |         2      | GF Proto 1 - Setup                                                            | `feature:greenfield-proto`, `phase:setup`, `type:setup`, `type:test`, `priority:p1`, `risk:blocking`                                                       |
| T003 |         3      | GF Proto 1 - Setup                                                            | `feature:greenfield-proto`, `phase:setup`, `type:setup`, `type:ui`, `priority:p1`, `risk:blocking`                                                         |
| T004 |         4      | GF Proto 2 - Foundational                                                     | `feature:greenfield-proto`, `phase:foundational`, `type:core`, `type:config`, `priority:p1`, `risk:blocking`                                               |
| T005 |         5      | GF Proto 2 - Foundational                                                     | `feature:greenfield-proto`, `phase:foundational`, `type:core`, `type:state`, `priority:p1`, `risk:blocking`                                                |
| T006 |         6      | GF Proto 2 - Foundational                                                     | `feature:greenfield-proto`, `phase:foundational`, `type:orchestration`, `type:ui`, `priority:p1`, `risk:blocking`                                          |
| T007 |         7      | GF Proto 3 - US1 Start a New Calculation Session (MVP)                        | `feature:greenfield-proto`, `phase:us1-mvp`, `story:us1-new-session`, `type:core`, `type:test`, `priority:p1`, `risk:mvp`                                  |
| T008 |         8      | GF Proto 3 - US1 Start a New Calculation Session (MVP)                        | `feature:greenfield-proto`, `phase:us1-mvp`, `story:us1-new-session`, `type:orchestration`, `type:test`, `priority:p1`, `risk:mvp`                         |
| T009 |         9      | GF Proto 4 - US2 Apply Valid Tokens to the Stack (MVP Core Execution)         | `feature:greenfield-proto`, `phase:us2-core-execution`, `story:us2-apply-valid-tokens`, `type:core`, `type:test`, `priority:p1`, `risk:mvp`                |
| T010 |        10      | GF Proto 4 - US2 Apply Valid Tokens to the Stack (MVP Core Execution)         | `feature:greenfield-proto`, `phase:us2-core-execution`, `story:us2-apply-valid-tokens`, `type:orchestration`, `type:test`, `priority:p1`, `risk:mvp`       |
| T011 |        11      | GF Proto 5 - US3 Inspect Current Stack (MVP Observability)                    | `feature:greenfield-proto`, `phase:us3-observability`, `story:us3-inspect-current-stack`, `type:orchestration`, `type:test`, `priority:p1`, `risk:mvp`     |
| T012 |        12      | GF Proto 5 - US3 Inspect Current Stack (MVP Observability)                    | `feature:greenfield-proto`, `phase:us3-observability`, `story:us3-inspect-current-stack`, `type:ui`, `type:test`, `priority:p1`, `risk:mvp`                |
| T013 |        13      | GF Proto 6 - US4 Reject Operations With Insufficient Operands (MVP Stability) | `feature:greenfield-proto`, `phase:us4-stability`, `story:us4-insufficient-operands`, `type:core`, `type:test`, `priority:p1`, `risk:mvp`                  |
| T014 |        14      | GF Proto 6 - US4 Reject Operations With Insufficient Operands (MVP Stability) | `feature:greenfield-proto`, `phase:us4-stability`, `story:us4-insufficient-operands`, `type:orchestration`, `type:test`, `priority:p1`, `risk:mvp`         |
| T015 |        15      | GF Proto 7 - US5 Reject Invalid Tokens & Unsupported Operations               | `feature:greenfield-proto`, `phase:us5-invalid-tokens`, `story:us5-invalid-tokens-unsupported-operations`, `type:core`, `type:test`, `priority:p2`         |
| T016 |        16      | GF Proto 7 - US5 Reject Invalid Tokens & Unsupported Operations               | `feature:greenfield-proto`, `phase:us5-invalid-tokens`, `story:us5-invalid-tokens-unsupported-operations`, `type:input`, `type:test`, `priority:p2`        |
| T017 |        17      | GF Proto 8 - US6 Handle Arithmetic Domain Errors & Numeric Limits             | `feature:greenfield-proto`, `phase:us6-arithmetic-numeric`, `story:us6-arithmetic-domain-numeric-limits`, `type:core`, `type:test`, `priority:p2`          |
| T018 |        18      | GF Proto 8 - US6 Handle Arithmetic Domain Errors & Numeric Limits             | `feature:greenfield-proto`, `phase:us6-arithmetic-numeric`, `story:us6-arithmetic-domain-numeric-limits`, `type:orchestration`, `type:test`, `priority:p2` |
| T019 |        19      | GF Proto 9 - US7 Reset Calculator                                             | `feature:greenfield-proto`, `phase:us7-reset`, `story:us7-reset-calculator`, `type:orchestration`, `type:test`, `priority:p3`                              |
| T020 |        20      | GF Proto 9 - US7 Reset Calculator                                             | `feature:greenfield-proto`, `phase:us7-reset`, `story:us7-reset-calculator`, `type:ui`, `type:test`, `priority:p3`                                         |
| T021 |        21      | GF Proto 10 - US8 Undo Last Accepted Token                                    | `feature:greenfield-proto`, `phase:us8-undo`, `story:us8-undo-last-accepted-token`, `type:orchestration`, `type:test`, `priority:p3`                       |
| T022 |        22      | GF Proto 10 - US8 Undo Last Accepted Token                                    | `feature:greenfield-proto`, `phase:us8-undo`, `story:us8-undo-last-accepted-token`, `type:ui`, `type:test`, `priority:p3`                                  |
| T023 |        23      | GF Proto 11 - Documentation & Developer Experience                            | `feature:greenfield-proto`, `phase:docs-dx`, `type:docs`, `priority:p2`                                                                                    |
| T024 |        24      | GF Proto 11 - Documentation & Developer Experience                            | `feature:greenfield-proto`, `phase:docs-dx`, `type:config`, `type:docs`, `priority:p2`                                                                     |
| T025 |        25      | GF Proto 11 - Documentation & Developer Experience                            | `feature:greenfield-proto`, `phase:docs-dx`, `type:docs`, `type:architecture`, `priority:p2`                                                               |
| T026 |        26      | GF Proto 11 - Documentation & Developer Experience                            | `feature:greenfield-proto`, `phase:docs-dx`, `type:docs`, `type:validation`, `priority:p2`                                                                 |
| T027 |        27      | GF Proto 11 - Documentation & Developer Experience                            | `feature:greenfield-proto`, `phase:docs-dx`, `type:docs`, `type:test`, `priority:p2`                                                                       |
| T028 |        28      | GF Proto 11 - Documentation & Developer Experience                            | `feature:greenfield-proto`, `phase:docs-dx`, `type:docs`, `type:traceability`, `priority:p2`                                                               |
| T029 |        29      | GF Proto 11 - Documentation & Developer Experience                            | `feature:greenfield-proto`, `phase:docs-dx`, `type:docs`, `type:validation`, `priority:p2`                                                                 |

```

