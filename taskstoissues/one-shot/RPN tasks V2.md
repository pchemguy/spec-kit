---

# Tasks: RPN Calculator (Browser-Based)

---

## Phase 1: Setup

**Purpose**: Initialize browser-based React + TypeScript project with test runner.

### Tests for Setup

* [ ] T001 Configure test runner bootstrap in tests/setup.ts and package.json
* [ ] T002 Add baseline sanity test in tests/unit/sanity.test.ts verifying test environment executes

### Implementation for Setup

* [ ] T003 Initialize React + TypeScript project structure in package.json, tsconfig.json, vite.config.ts, and index.html
* [ ] T004 [P] Create base app shell in src/app/main.tsx and src/app/App.tsx
* [ ] T005 [P] Create base styles in src/ui/styles/app.css

### Validation for Setup

* [ ] T006 Run test suite and dev server; fix issues until test runner executes and app boots cleanly

**Checkpoint**: Project runs locally and test environment is operational.

---

## Phase 2: Foundational

**Purpose**: Define shared domain model and architecture seams.

**⚠️ CRITICAL**: No user story work should begin until this phase is complete.

### Tests for Foundational Work

* [ ] T007 Add unit coverage for stack state primitives in tests/unit/core/state.test.ts
* [ ] T008 Add integration coverage for baseline calculator exposure in tests/integration/app-flow.test.ts

### Implementation for Foundational Work

* [ ] T009 Create domain types and token/operator definitions in src/calculator/core/types.ts
* [ ] T010 Create immutable stack state helpers in src/calculator/core/state.ts
* [ ] T011 Create orchestration hook in src/app/hooks/useRpnCalculator.ts
* [ ] T012 [P] Create UI shells in src/ui/components/TokenInput.tsx, StackView.tsx, CalculatorStatusPanel.tsx, ResetButton.tsx, UndoButton.tsx

### Validation for Foundational Work

* [ ] T013 Run foundational tests and fix issues until domain + orchestration baseline is green

**Checkpoint**: Core state model and architecture seams are stable.

---

## Phase 3: US1 Start a New Calculation Session (MVP)

**Goal**: Initialize a new calculator session with empty stack.

**Independent Test**: Initial render shows empty stack and ready state.

### Tests for User Story 1

* [ ] T014 [US1] Add unit tests for session initialization in tests/unit/core/state.test.ts
* [ ] T015 [US1] Add integration test for initial app state in tests/integration/app-flow.test.ts

### Implementation for User Story 1

* [ ] T016 [US1] Implement session initialization in src/calculator/core/state.ts
* [ ] T017 [US1] Expose initial state via useRpnCalculator.ts
* [ ] T018 [US1] Render initial UI state in StackView.tsx and CalculatorStatusPanel.tsx

### Validation for User Story 1

* [ ] T019 [US1] Run US1 tests and fix issues until all tests pass

**Checkpoint**: Calculator initializes correctly and is testable.

---

## Phase 4: US2 Apply Valid Tokens to the Stack (MVP Core Execution)

**Goal**: Apply numbers and operators to mutate stack.

**Independent Test**: Valid token sequence produces correct stack state.

### Tests for User Story 2

* [ ] T020 [US2] Add unit tests for token parsing and execution in tests/unit/core/execution.test.ts
* [ ] T021 [US2] Add integration test for token input flow in tests/integration/app-flow.test.ts

### Implementation for User Story 2

* [ ] T022 [US2] Implement token parsing in src/calculator/core/parser.ts
* [ ] T023 [US2] Implement execution engine in src/calculator/core/executor.ts
* [ ] T024 [US2] Integrate execution into useRpnCalculator.ts
* [ ] T025 [US2] Wire TokenInput.tsx to dispatch tokens

### Validation for User Story 2

* [ ] T026 [US2] Run US2 tests and fix issues until execution logic is correct

**Checkpoint**: Core RPN execution works.

---

## Phase 5: US3 Inspect Current Stack (MVP Observability)

**Goal**: Display stack to user.

**Independent Test**: Stack reflects execution results accurately.

### Tests for User Story 3

* [ ] T027 [US3] Add unit tests for stack representation in tests/unit/core/state.test.ts
* [ ] T028 [US3] Add UI integration test for stack rendering in tests/integration/ui.test.ts

### Implementation for User Story 3

* [ ] T029 [US3] Implement stack formatting logic in src/calculator/core/format.ts
* [ ] T030 [US3] Render stack in StackView.tsx

### Validation for User Story 3

* [ ] T031 [US3] Run tests and fix issues until stack rendering matches state

**Checkpoint**: Stack is observable and correct.

---

## Phase 6: US4 Reject Operations With Insufficient Operands (MVP Stability)

**Goal**: Prevent invalid operator execution.

**Independent Test**: Operators fail when insufficient operands exist.

### Tests for User Story 4

* [ ] T032 [US4] Add unit tests for operand validation in tests/unit/core/execution.test.ts
* [ ] T033 [US4] Add integration tests for invalid operations in tests/integration/app-flow.test.ts

### Implementation for User Story 4

* [ ] T034 [US4] Implement operand validation in executor.ts
* [ ] T035 [US4] Propagate validation errors via state.ts

### Validation for User Story 4

* [ ] T036 [US4] Run tests and fix issues until invalid operations are safely rejected

**Checkpoint**: Invalid operations are safely handled.

---

## Phase 7: US5 Reject Invalid Tokens & Unsupported Operations

### Tests for User Story 5

* [ ] T037 [US5] Add tests for invalid tokens in tests/unit/core/parser.test.ts
* [ ] T038 [US5] Add integration tests for invalid input handling

### Implementation for User Story 5

* [ ] T039 [US5] Implement token validation in parser.ts
* [ ] T040 [US5] Handle unsupported operations in executor.ts

### Validation for User Story 5

* [ ] T041 [US5] Run tests and fix issues until invalid tokens are rejected correctly

---

## Phase 8: US6 Handle Arithmetic Domain Errors & Numeric Limits

### Tests for User Story 6

* [ ] T042 [US6] Add tests for divide-by-zero and overflow in execution.test.ts
* [ ] T043 [US6] Add integration tests for domain errors

### Implementation for User Story 6

* [ ] T044 [US6] Implement domain error handling in executor.ts
* [ ] T045 [US6] Surface error states in useRpnCalculator.ts

### Validation for User Story 6

* [ ] T046 [US6] Run tests and fix issues until domain errors handled correctly

---

## Phase 9: US7 Reset Calculator

### Tests for User Story 7

* [ ] T047 [US7] Add tests for reset behavior in state.test.ts
* [ ] T048 [US7] Add integration test for reset flow

### Implementation for User Story 7

* [ ] T049 [US7] Implement reset logic in state.ts
* [ ] T050 [US7] Wire ResetButton.tsx

### Validation for User Story 7

* [ ] T051 [US7] Run tests and fix issues until reset works correctly

---

## Phase 10: US8 Undo Last Accepted Token

### Tests for User Story 8

* [ ] T052 [US8] Add tests for undo behavior in state.test.ts
* [ ] T053 [US8] Add integration test for undo flow

### Implementation for User Story 8

* [ ] T054 [US8] Implement history tracking in state.ts
* [ ] T055 [US8] Implement undo operation
* [ ] T056 [US8] Wire UndoButton.tsx

### Validation for User Story 8

* [ ] T057 [US8] Run tests and fix issues until undo works correctly

---

## Phase 11: Documentation & Developer Experience

**Purpose**: Ensure complete, accurate, and non-speculative documentation.

### Documentation Tasks

* [ ] T058 Update README.md with feature overview and usage
* [ ] T059 Create DEVELOPMENT.md with architecture and extension guidance
* [ ] T060 Create docs/architecture.md describing core vs UI separation
* [ ] T061 Create docs/testing.md describing test strategy
* [ ] T062 Review all documentation against implementation and remove inconsistencies

### Validation for Documentation

* [ ] T063 Verify documentation matches implemented behavior and update discrepancies

**Checkpoint**: Documentation is complete and aligned with implementation.

---
