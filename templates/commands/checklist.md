---
description: Generate a custom checklist for the current feature based on user requirements.
scripts:
  sh: scripts/bash/check-prerequisites.sh --json
  ps: scripts/powershell/check-prerequisites.ps1 -Json
---

# Specification Quality Evaluation and Diagnostic Protocol

**CRITICAL CONCEPT**: Checklists produced by this protocol validate the internal quality of specification artifacts, not implementation.

**✅ Correct Patterns (Specification Quality Validation)**:

- "Are visual hierarchy requirements defined for all card types?"
- "Is 'prominent display' quantified with specific sizing/positioning?"
- "Are hover state requirements consistent across all interactive elements?"
- "Are accessibility requirements defined for keyboard navigation?"
- "Does the spec define what happens when logo image fails to load?"
- "Can 'balanced visual weight' be objectively verified?"

**❌ Incorrect Patterns (Implementation Testing and Assessment)**:

- "Verify the button clicks correctly"
- "Test error handling works"
- "Confirm the API returns 200"
- "Does the implementation match the specification?"

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Specification Quality Model

This document defines a protocol for evaluating the quality of specifications by generating diagnostic checklist items. A specification produced by the `specify` command defines system behavior, capabilities, and operating context, along with measurable success criteria:

- **Scenario Space** — behavioral flows and state transitions
  Scenario types:
    - **Primary** — main success/happy path  
    - **Alternate** — valid variations of primary flow  
    - **Exception / Error** — failure paths  
    - **Recovery** — restoration after failure
- **Requirement Set** — capabilities and system-level constraints
    * Functional (FR)
    * Non-Functional (NFR)
- **Context** — external conditions and dependencies
    * Assumptions
    * Dependencies

Additionally, a specification defines:  
  
- **Success Criteria** — measurable outcomes used to evaluate system success

The generated checklist enables systematic evaluation of both individual specification elements and their relationships.

### Quality Dimensions

| Quality Dimension | Defect Marker | Key Question                                                                           | Problem                                 |
| ----------------- | ------------- | -------------------------------------------------------------------------------------- | --------------------------------------- |
| Clarity           | Ambiguity     | Are specification elements unambiguous and specific?                                   | vague, unclear, or multi-interpretation |
| Consistency       | Conflict      | Do specification elements align without contradiction?                                 | contradictory or inconsistent           |
| Completeness      | Gap           | Are all required capabilities and constraints specified for each defined scenario?     | missing requirement or constraint       |
| Coverage          | Gap           | Are all relevant scenarios, flows, and conditions defined?                             | missing scenario or condition           |
| Measurability     | Unverifiable  | Can specification elements be objectively verified?                                    | cannot be objectively verified          |
| Correctness       | Incorrect     | Do specification elements reflect intended behavior and domain constraints accurately? | invalid or wrong relative to context    |
| Feasibility       | Infeasible    | Can requirements be realistically implemented?                                         | cannot be realistically implemented     |
| Relevance         | Redundant     | Are all elements necessary and within scope?                                           | unnecessary or duplicate                |

### Component Relationships

- Scenario → Requirements (Completeness):
  "Does the specification define required capabilities and constraints for each scenario?"
- Scenario Coverage:
  "Are all relevant scenarios defined?"
- Requirements ↔ Context:
  "Do assumptions or dependencies conflict with requirements?"
- Cross-component consistency:
  "Are specification elements consistent across scenarios, requirements, and context?"

## Execution Steps

### 1. Setup

Run `{SCRIPT}` from repo root and parse JSON for FEATURE_DIR and AVAILABLE_DOCS list.

- All file paths must be absolute.
- Handle single quotes in arguments:
    - Prefer double quotes syntax, e.g., "I'm Groot"
    - Fallback to escaping syntax, e.g., 'I'\''m Groot'

### 2. Clarify Intent via Focused Questions (Dynamic)

#### Initial Questions

Derive up to THREE (Q1-Q3) initial contextual clarifying questions (no pre-baked catalog):

- derive from the user's phrasing and extracted signals from spec/plan/tasks for derivation;
- focus on information that
    - materially changes checklist content;
    - is unclear or ambiguous based on context provided by `$ARGUMENTS`.
- prefer precision over breadth.

##### Derivation Algorithm

1. Extract signals:
    - feature domain keywords (e.g., auth, latency, UX, API),
    - risk indicators (e.g., "critical", "must", "compliance"),
    - stakeholder hints (e.g., "QA", "review", "security team"),
    - explicit deliverables (e.g., "a11y", "rollback", "contracts").
2. Cluster signals into candidate focus areas (max 4) ranked by relevance.
3. Identify probable audience & timing (author, reviewer, QA, and release) if not explicit.
4. Detect missing dimensions:
    - scope breadth,
    - depth/rigor,
    - risk emphasis,
    - exclusion boundaries,
    - measurable acceptance criteria.
5. Formulate questions chosen from these archetypes:
    - scope refinement (e.g., "Should this include integration touchpoints with X and Y or stay limited to local module correctness?")
    - risk prioritization (e.g., "Which of these potential risk areas should receive mandatory gating checks?")
    - depth calibration (e.g., "Is this a lightweight pre-commit sanity list or a formal release gate?")
    - audience framing (e.g., "Will this be used by the author only or peers during PR review?")
    - boundary exclusion (e.g., "Should we explicitly exclude performance tuning items this round?")
    - scenario type gap (e.g., "No recovery flows detected - are rollback / partial failure paths in scope?")

Use defaults, when interaction impossible:

- Depth: Standard
- Audience: Reviewer (PR) if code-related; Author otherwise
- Focus: Top 2 relevance clusters

##### Formatting Rules

- Use labels Q1/Q2/Q3
- If presenting options
    - Generate a compact table with columns: Option | Candidate | Why It Matters
    - Limit to A–E options maximum; omit table if a free-form answer is clearer
- Never ask the user to restate what they already said
- Avoid speculative categories (no hallucination). If uncertain, ask explicitly: "Confirm whether X belongs in scope."

#### Follow-up Questions

If at least 2 scenario types remain unclear after collecting answers to the initial questions, you MAY ask up to TWO more targeted follow‑ups (Q4/Q5) with a one-line justification each (e.g., "Unresolved recovery path risk"). Do not exceed five total questions. Skip escalation if user explicitly declines further interrogation.

### 3. Understand User Request

Combine `$ARGUMENTS` + clarifying answers:

- Derive checklist theme (e.g., security, review, deploy, UX)
- Extract and prioritize focus selections from user input, clarifications, and context signals (e.g., UX consistency, API reliability)
- Consolidate explicit must-have items mentioned by user
- Map focus selections to quality dimensions
- Infer preliminary context from $ARGUMENTS and clarifications only
    - identify candidate scenario types  
    - identify candidate requirement areas  
    - identify likely risk areas  
    - do not assume specification content

### 4. Load Feature Context
 
Read from FEATURE_DIR:

- `spec.md`: Feature requirements, scenarios, context, success criteria, and scope
- `plan.md` (if exists): Technical details, dependencies

**Context Loading Strategy**:
   
- Load only necessary portions relevant to active focus areas (avoid full-file dumping)
    <!-- TODO: This point needs to be revised. The present formulation is not actionable. How can LLM decide what is relevant before loading the entire document? Possibly instruct it to analyzed heading first (need to consider document structure)? Can a script be used to extract the relevant portions? -->
- Use progressive disclosure: add follow-on retrieval only if gaps detected
- Compress large textual blocks, if appropriate:
    - **long sections**: preferably summarize into concise scenario/requirement bullets
    - **large source docs**: generate interim summary items instead of embedding raw text

### 5. Contextual Synthesis

Combine user intent with loaded feature context:

- Refine checklist theme based on actual specification content
- Validate or adjust inferred focus areas
- Identify:
    - missing requirements
    - missing scenarios
    - inconsistencies between spec sections
- Infer any identified context omissions (all inferences MUST be grounded in available user inputs and loaded content; do not introduce unsupported assumptions)

### 6. Generate Checklist
   
Perform checklist generation using the steps defined below.  

#### 6.1. Initialize or append checklist file

- Create `FEATURE_DIR/checklists/` directory if it does not exist
- Create new or locate existing file:
    - Use short, descriptive filenames `[domain].md` (e.g., `ux.md`, `api.md`, `security.md`)
    - If chosen `[domain].md` file does NOT exist:
        - Create new file
        - Start numbering at CHK001
    - If file exists:
        - Append new items
        - Continue numbering following the last existing CHK ID
- Use `templates/checklist-template.md` as canonical template, filling title, metadata section, grouping category headings, and ID formatting.
- NEVER delete, overwrite, or renumber existing content

#### 6.2. Generate candidate items  

Each checklist item MUST follow pattern:

```
"Are/Is [requirement aspect] [quality condition] for [scope]?" [<category tags>, <traceability markers>]
```

Each checklist item MUST:

- Evaluate **specification item quality**, not system behavior
- NOT reference implementation or runtime behavior
- Assess at least one quality dimension defined above
- Be answerable using ONLY spec/plan
- Refer to what is WRITTEN (or missing)
- Use interrogative form (question)
- Include:
    - Category tag(s)
        - Quality dimension, e.g., `Completeness`.
        - Other appropriate categories, such as `Exception Flow`, `Edge Case`, `Traceability`, etc.
    - Traceability marker(s), if available, e.g.,
        - `Spec §FR-005`.

##### Scenario Coverage

For EACH scenario type, at least ONE checklist item MUST be generated:

- Requirement quality:
  "Are [scenario type] requirements complete, clear, and consistent?"
- If requirements are NOT defined:
  "Are [scenario type] requirements intentionally excluded or missing?"

Recovery scenarios (state mutation):

- If the feature involves state-changing operations (e.g., database writes, migrations, transactions):
  "Are recovery/rollback requirements defined when state mutation fails?"

##### Traceability Requirements

- MINIMUM 80% of items MUST reference a specific requirement
- If no requirement identification scheme exists (IDs or stable section references), produce a traceability item:
    "Is a requirement & acceptance criteria ID scheme established? [Traceability]"

##### Sample Patterns

   - "Are [requirement type] defined/specified/documented for [scenario]?"
   - "Is [vague term] quantified/clarified with specific criteria?"
   - "Are requirements consistent between [section A] and [section B]?"
   - "Can [requirement] be objectively measured/verified?"
   - "Are [edge cases/scenarios] addressed in requirements?"
   - "Does the spec define [missing aspect]?"

#### 6.3. Validate items against rules  

Reject any checklist item that:

- Tests implementation behavior or details (frameworks, APIs, algorithms) instead of requirements
- Refers to runtime execution, user interaction, or system behavior
- Includes test procedures, test cases, or QA steps
- Cannot be answered from specification text

Heuristic red-flag keywords (require scrutiny, not automatic rejection):

- "click", "render", "navigate", "execute", "load"
- "works properly", "functions as expected"
- "verify", "test", "confirm", "check"

#### 6.4. Group checklist items

Group checklist items using quality dimension as grouping category.

#### 6.5. Apply consolidation rules

- If candidate items > 40, prioritize by risk and impact
- Merge near-duplicate items
- If many low-impact edge cases, combine into one item, e.g.:
    "Are edge cases X, Y, Z addressed in requirements?"

### 7. Reporting

In addition to generating a checklist, return prompt output:

- full checklist file path, indicating whether the run created a new checklist file or appended to an existing one
- number of created checklist items
- summary
    - Focus areas selected
    - Depth level
    - Actor/timing
    - Any explicit user-specified must-have items incorporated

## Example Checklist Types & Sample Items

Completeness:

- "Are error handling requirements defined for all API failure modes? [Completeness]"
- "Are accessibility requirements specified for all interactive elements? [Completeness]"
- "Are mobile breakpoint requirements defined for responsive layouts? [Completeness]"
- “Are success criteria defined for all critical user stories? [Completeness]”

Clarity:

- "Is 'fast loading' quantified with specific timing thresholds? [Clarity, Spec §NFR-2]"
- "Are 'related episodes' selection criteria explicitly defined? [Clarity, Spec §FR-5]"
- "Is 'prominent' defined with measurable visual properties? [Clarity, Spec §FR-4]"

Consistency:

- "Do navigation requirements align across all pages? [Consistency, Spec §FR-10]"
- "Are card component requirements consistent between landing and detail pages? [Consistency]"

Coverage:

- "Are requirements defined for zero-state scenarios (no episodes)? [Coverage, Edge Case]"
- "Are concurrent user interaction scenarios addressed? [Coverage]"
- "Are requirements specified for partial data loading failures? [Coverage, Exception Flow]"

Measurability:

- "Are visual hierarchy requirements measurable/testable? [Acceptance Criteria, Spec §FR-1]"
- "Can 'balanced visual weight' be objectively verified? [Measurability, Spec §FR-2]"

Feasibility:

- “Are defined targets realistically achievable? [Feasibility, Spec §FR-5]”

Correctness:

- “Do success criteria reflect actual user/business outcomes? [Correctness, Spec §FR-2]”

### UX Requirements Quality: `ux.md`

- "Are visual hierarchy requirements defined with measurable criteria? [Clarity, Spec §FR-1]"
- "Is the number and positioning of UI elements explicitly specified? [Completeness, Spec §FR-1]"
- "Are interaction state requirements (hover, focus, active) consistently defined? [Consistency]"
- "Are accessibility requirements specified for all interactive elements? [Completeness]"
- "Is fallback behavior defined when images fail to load? [Edge Case, Coverage]"
- "Can 'prominent display' be objectively measured? [Measurability, Spec §FR-4]"

### API Requirements Quality: `api.md`

- "Are error response formats specified for all failure scenarios? [Completeness]"
- "Are rate limiting requirements quantified with specific thresholds? [Clarity]"
- "Are authentication requirements consistent across all endpoints? [Consistency]"
- "Are retry/timeout requirements defined for external dependencies? [Completeness]"
- "Is versioning strategy documented in requirements? [Coverage]"

### Performance Requirements Quality: `performance.md`

- "Are performance requirements quantified with specific metrics? [Clarity]"
- "Are performance targets defined for all critical user journeys? [Completeness]"
- "Are performance requirements under different load conditions specified? [Completeness]"
- "Can performance requirements be objectively measured? [Measurability]"
- "Are degradation requirements defined for high-load scenarios? [Edge Case, Completeness]"

### Security Requirements Quality: `security.md`

- "Are authentication requirements specified for all protected resources? [Completeness]"
- "Are data protection requirements defined for sensitive information? [Completeness]"
- "Is the threat model documented and requirements aligned to it? [Coverage, Consistency]"
- "Are security requirements consistent with compliance obligations? [Consistency]"
- "Are security failure/breach response requirements defined? [Coverage, Exception Flow]"

### What Do and NOT To Do

**✅ CORRECT - These test requirements quality:**

```markdown
- [ ] CHK001 - Are the number and layout of featured episodes explicitly specified? [Completeness, Spec §FR-001]
- [ ] CHK002 - Are hover state requirements consistently defined for all interactive elements? [Consistency, Spec §FR-003]
- [ ] CHK003 - Are navigation requirements clear for all clickable brand elements? [Clarity, Spec §FR-010]
- [ ] CHK004 - Is the selection criteria for related episodes documented? [Completeness, Spec §FR-005]
- [ ] CHK005 - Are loading state requirements defined for asynchronous episode data? [Coverage]
- [ ] CHK006 - Can "visual hierarchy" requirements be objectively measured? [Measurability, Spec §FR-001]
```

**❌ WRONG - These test implementation, not requirements:**

```markdown
- [ ] CHK001 - Verify landing page displays 3 episode cards [Spec §FR-001]
- [ ] CHK002 - Test hover states work correctly on desktop [Spec §FR-003]
- [ ] CHK003 - Confirm logo click navigates to home page [Spec §FR-010]
- [ ] CHK004 - Check that related episodes section shows 3-5 items [Spec §FR-005]
```

**Key Differences:**

- Wrong: Tests if the system works correctly
- Correct: Tests if the requirements are written correctly
- Wrong: Verification of behavior
- Correct: Validation of requirement quality
- Wrong: "Does it do X?"
- Correct: "Is X clearly specified?"
