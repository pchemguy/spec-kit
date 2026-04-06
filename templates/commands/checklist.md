---
description: Generate a custom checklist for the current feature based on user requirements.
scripts:
  sh: scripts/bash/check-prerequisites.sh --json
  ps: scripts/powershell/check-prerequisites.ps1 -Json
---
# Specification Quality Evaluation and Diagnostic Protocol

**CRITICAL CONCEPT**: Checklists produced by this protocol validate the internal quality (clarity, completeness, consistency, coverage) of specification artifacts (scenarios, requirements, context constraints), not implementation.

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

## Conceptual Model

This document defines a protocol for evaluating the quality of specifications (produced by the `specify` command) by generating diagnostic checklist items. The generated checklist functions analogously to a "unit test suite" for a specification, targeting:

* Three critical components of specifications:
    * Scenarios
        - Primary
        - Alternate
        - Exception / Error
        - Recovery
    * Requirements
        * Functional
        * Non-Functional
    * Context Constraints
        * Assumptions
        * Dependencies
* Component relationships, e.g.:
    * Does the specification define all required capabilities and constraints for each scenario? (Completeness)
    * Does the specification cover all relevant scenarios? (Coverage)
    * Do assumptions conflict with requirements? (Consistency)

The purpose of this protocol is to support iterative refinement of specifications before any implementation occurs. For this reason, implementation behavior and conformance are completely out of scope.

## Execution Steps

### 1. Setup

Run `{SCRIPT}` from repo root and parse JSON for FEATURE_DIR and AVAILABLE_DOCS list.

- All file paths must be absolute.
- Handle single quotes in args:
    - Prefer double quotes syntax, e.g., "I'm Groot"
    - Fallback to escaping syntax, e.g., 'I'\''m Groot'

### 2. Clarify Intent via Focused Questions (Dynamic)

Derive up to THREE (Q1-Q3) initial contextual clarifying questions (no pre-baked catalog):

- derive from the user's phrasing and extracted signals from spec/plan/tasks for derivation;
- focus on information that
    - materially changes checklist content;
    - is unclear or ambiguous based on context provided by `$ARGUMENTS`.
- prefer precision over breadth.

Up to two (Q4-Q5) follow-up questions can be asked as detailed below.

#### Derivation Algorithm

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
      - scenario class gap (e.g., "No recovery flows detected - are rollback / partial failure paths in scope?")

Use defaults, when interaction impossible:

   - Depth: Standard
   - Audience: Reviewer (PR) if code-related; Author otherwise
   - Focus: Top 2 relevance clusters

#### Formatting Rules

   - Use labels Q1/Q2/Q3
   - If presenting options
       - Generate a compact table with columns: Option | Candidate | Why It Matters
       - Limit to A–E options maximum; omit table if a free-form answer is clearer
   - Never ask the user to restate what they already said
   - Avoid speculative categories (no hallucination). If uncertain, ask explicitly: "Confirm whether X belongs in scope."

#### Follow-up Questions

   After answers: if at least 2 scenario classes (Alternate / Exception / Recovery / Non-Functional domain) remain unclear, you MAY ask up to TWO more targeted follow‑ups (Q4/Q5) with a one-line justification each (e.g., "Unresolved recovery path risk"). Do not exceed five total questions. Skip escalation if user explicitly declines more.

### 3. Understand User Request

Combine `$ARGUMENTS` + clarifying answers:

   - Derive checklist theme (e.g., security, review, deploy, ux)
   - Consolidate explicit must-have items mentioned by user
   - Map focus selections to category scaffolding
   - Infer any missing context from spec/plan/tasks (do NOT hallucinate)

### 4. Load Feature Context
 
Read from FEATURE_DIR:

   - `spec.md`: Feature requirements and scope
   - `plan.md` (if exists): Technical details, dependencies
   - `tasks.md` (if exists): Implementation tasks

   **Context Loading Strategy**:
   
   - Load only necessary portions relevant to active focus areas (avoid full-file dumping)
    <!-- TODO: This point needs to be revised. The present formulation is not actionable. How can LLM decide what is relevant before loading the entire document? Possibly instruct it to analyzed heading first (need to consider document structure)? Can a script be used to extract the relevant portions? -->
   - Use progressive disclosure: add follow-on retrieval only if gaps detected
   - Compress large textual blocks, if appropriate:
       - **long sections**: preferably summarize into concise scenario/requirement bullets
       - **large source docs**: generate interim summary items instead of embedding raw text

### 5. Generate Checklist
   
Perform checklist generation using the rules defined below.  
  
Steps:  

1. Initialize or append checklist file  
2. Generate candidate items  
3. Validate items against rules  
4. Group items by category  
5. Apply consolidation rules

#### File Handling

- Create `FEATURE_DIR/checklists/` directory if it does not exist
- Use short, descriptive filenames `[domain].md` (e.g., `ux.md`, `api.md`, `security.md`)
    - If file does NOT exist:
        - Create new file
        - Start numbering at CHK001
    - If file exists:
        - Append new items
        - Continue numbering following the last existing CHK ID
- NEVER delete, overwrite, or renumber existing content

#### Generation Rules

Each checklist item MUST evaluate **requirement quality**, not system behavior.

Each item MUST assess at least one of quality dimensions:

- **Completeness**: Are all necessary requirements present?
- **Clarity**: Are requirements unambiguous and specific?
- **Consistency**: Do requirements align with each other?
- **Coverage**: Are all scenarios/edge cases addressed?
- **Measurability**: Can requirements be objectively verified?

Each item MUST:

- Be answerable using ONLY spec/plan/tasks
- Refer to what is WRITTEN (or missing)
- NOT reference implementation or runtime behavior
- Use interrogative form (question)
- Include:
    - a quality dimension tag
    - a traceability marker

#### Item Structure

Each item MUST follow pattern:

```
"Are/Is [requirement aspect] [quality condition] for [scope]?"
```

Each item MUST include:

- Quality dimension  
    `[Completeness | Clarity | Consistency | Coverage | Measurability | etc.]`
- Traceability marker:
    - `[Spec §X.Y]`, OR
    - `[Gap]`, `[Ambiguity]`, `[Conflict]`, `[Assumption]`

#### Category Model

Group items by requirement scope:

- **Functional Requirements**  
- **Non-Functional Requirements** (Performance, Security, Accessibility, etc.)  
- **Scenario Coverage**
- **Dependencies & Assumptions**

#### Scenario Coverage

Ensure coverage across scenario types:

- Primary
- Alternate
- Exception / Error
- Recovery
- Non-Functional

For EACH scenario type, at least ONE checklist item MUST be generated:

- Requirement quality:
  "Are [scenario type] requirements complete, clear, and consistent?"
- If requirements are NOT defined:
  "Are [scenario type] requirements intentionally excluded or missing? [Gap]"

Recovery scenarios (state mutation):

- If the feature involves state-changing operations (e.g., database writes, migrations, transactions):
  "Are recovery/rollback requirements defined when state mutation fails? [Gap]"

#### Traceability Requirements

- MINIMUM 80% of items MUST reference a specific requirement
- Each item SHOULD include:
    - [Spec §X.Y] — reference to a requirement via its ID or stable section
    - [Gap] — no corresponding requirement exists
- If no requirement identification scheme exists (IDs or stable section references), produce a traceability item:
    "Is a requirement & acceptance criteria ID scheme established? [Traceability]"

#### Validation Rules (MANDATORY)

Reject any checklist item that:

- Tests implementation behavior or details (frameworks, APIs, algorithms) instead of requirements
- Refers to runtime execution, user interaction, or system behavior
- Includes test procedures, test cases, or QA steps
- Cannot be answered from specification text

Checklist items SHOULD surface requirement quality issues using explicit markers:  
  
- [Ambiguity] — unclear or undefined terms  
- [Conflict] — inconsistent or contradictory requirements  
- [Assumption] — unstated dependencies or implicit conditions  
- [Dependency] — external systems or requirements not specified  
- [Gap] — missing requirement or scenario

Heuristic red-flag keywords (require scrutiny, not automatic rejection):

- "click", "render", "navigate", "execute", "load"
- "works properly", "functions as expected"
- "verify", "test", "confirm", "check"

#### Consolidation Rules

- If candidate items > 40, prioritize by risk and impact
- Merge near-duplicate items
- If many low-impact edge cases, combine into one item, e.g.:
    "Are edge cases X, Y, Z addressed in requirements? [Coverage]"



---
---




   **CORE PRINCIPLE - Test the Requirements, Not the Implementation**. Every checklist item MUST evaluate the REQUIREMENTS for:
   
 - **Completeness**: Are all necessary requirements present?
 - **Clarity**: Are requirements unambiguous and specific?
 - **Consistency**: Do requirements align without conflict?
 - **Measurability**: Can requirements be objectively verified?
 - **Coverage**: Are all scenarios, including edge cases, addressed?


 

   **HOW TO WRITE CHECKLIST ITEMS - "Unit Tests for English"**:

   ❌ **WRONG** (Testing implementation):
   - "Verify landing page displays 3 episode cards"
   - "Test hover states work on desktop"
   - "Confirm logo click navigates home"

   ✅ **CORRECT** (Testing requirements quality):
   - "Are the exact number and layout of featured episodes specified?" [Completeness]
   - "Is 'prominent display' quantified with specific sizing/positioning?" [Clarity]
   - "Are hover state requirements consistent across all interactive elements?" [Consistency]
   - "Are keyboard navigation requirements defined for all interactive UI?" [Coverage]
   - "Is the fallback behavior specified when logo image fails to load?" [Edge Cases]
   - "Are loading states defined for asynchronous episode data?" [Completeness]
   - "Does the spec define visual hierarchy for competing UI elements?" [Clarity]


   **EXAMPLES BY QUALITY DIMENSION**:

   Completeness:
   - "Are error handling requirements defined for all API failure modes? [Gap]"
   - "Are accessibility requirements specified for all interactive elements? [Completeness]"
   - "Are mobile breakpoint requirements defined for responsive layouts? [Gap]"

   Clarity:
   - "Is 'fast loading' quantified with specific timing thresholds? [Clarity, Spec §NFR-2]"
   - "Are 'related episodes' selection criteria explicitly defined? [Clarity, Spec §FR-5]"
   - "Is 'prominent' defined with measurable visual properties? [Ambiguity, Spec §FR-4]"

   Consistency:
   - "Do navigation requirements align across all pages? [Consistency, Spec §FR-10]"
   - "Are card component requirements consistent between landing and detail pages? [Consistency]"

   Coverage:
   - "Are requirements defined for zero-state scenarios (no episodes)? [Coverage, Edge Case]"
   - "Are concurrent user interaction scenarios addressed? [Coverage, Gap]"
   - "Are requirements specified for partial data loading failures? [Coverage, Exception Flow]"

   Measurability:
   - "Are visual hierarchy requirements measurable/testable? [Acceptance Criteria, Spec §FR-1]"
   - "Can 'balanced visual weight' be objectively verified? [Measurability, Spec §FR-2]"


   **✅ REQUIRED PATTERNS** - These test requirements quality:
   - ✅ "Are [requirement type] defined/specified/documented for [scenario]?"
   - ✅ "Is [vague term] quantified/clarified with specific criteria?"
   - ✅ "Are requirements consistent between [section A] and [section B]?"
   - ✅ "Can [requirement] be objectively measured/verified?"
   - ✅ "Are [edge cases/scenarios] addressed in requirements?"
   - ✅ "Does the spec define [missing aspect]?"

### 6. Structure Reference
 
Generate the checklist following the canonical template in `templates/checklist-template.md` for title, meta section, category headings, and ID formatting.

### 7. Report

Output full path to checklist file, item count, and summarize whether the run created a new file or appended to an existing one.

Summarize:

   - Focus areas selected
   - Depth level
   - Actor/timing
   - Any explicit user-specified must-have items incorporated

**Important**: Each `/speckit.checklist` command invocation uses a short, descriptive checklist filename and either creates a new file or appends to an existing one. This allows:

- Multiple checklists of different types (e.g., `ux.md`, `test.md`, `security.md`)
- Simple, memorable filenames that indicate checklist purpose
- Easy identification and navigation in the `checklists/` folder

To avoid clutter, use descriptive types and clean up obsolete checklists when done.

## Example Checklist Types & Sample Items

### UX Requirements Quality: `ux.md`

Sample items *(testing the requirements, NOT the implementation)*:

- "Are visual hierarchy requirements defined with measurable criteria? [Clarity, Spec §FR-1]"
- "Is the number and positioning of UI elements explicitly specified? [Completeness, Spec §FR-1]"
- "Are interaction state requirements (hover, focus, active) consistently defined? [Consistency]"
- "Are accessibility requirements specified for all interactive elements? [Coverage, Gap]"
- "Is fallback behavior defined when images fail to load? [Edge Case, Gap]"
- "Can 'prominent display' be objectively measured? [Measurability, Spec §FR-4]"

### API Requirements Quality: `api.md`

Sample items:

- "Are error response formats specified for all failure scenarios? [Completeness]"
- "Are rate limiting requirements quantified with specific thresholds? [Clarity]"
- "Are authentication requirements consistent across all endpoints? [Consistency]"
- "Are retry/timeout requirements defined for external dependencies? [Coverage, Gap]"
- "Is versioning strategy documented in requirements? [Gap]"

### Performance Requirements Quality: `performance.md`

Sample items:

- "Are performance requirements quantified with specific metrics? [Clarity]"
- "Are performance targets defined for all critical user journeys? [Coverage]"
- "Are performance requirements under different load conditions specified? [Completeness]"
- "Can performance requirements be objectively measured? [Measurability]"
- "Are degradation requirements defined for high-load scenarios? [Edge Case, Gap]"

### Security Requirements Quality: `security.md`

Sample items:

- "Are authentication requirements specified for all protected resources? [Coverage]"
- "Are data protection requirements defined for sensitive information? [Completeness]"
- "Is the threat model documented and requirements aligned to it? [Traceability]"
- "Are security requirements consistent with compliance obligations? [Consistency]"
- "Are security failure/breach response requirements defined? [Gap, Exception Flow]"

## Anti-Examples: What NOT To Do

**❌ WRONG - These test implementation, not requirements:**

```markdown
- [ ] CHK001 - Verify landing page displays 3 episode cards [Spec §FR-001]
- [ ] CHK002 - Test hover states work correctly on desktop [Spec §FR-003]
- [ ] CHK003 - Confirm logo click navigates to home page [Spec §FR-010]
- [ ] CHK004 - Check that related episodes section shows 3-5 items [Spec §FR-005]
```

**✅ CORRECT - These test requirements quality:**

```markdown
- [ ] CHK001 - Are the number and layout of featured episodes explicitly specified? [Completeness, Spec §FR-001]
- [ ] CHK002 - Are hover state requirements consistently defined for all interactive elements? [Consistency, Spec §FR-003]
- [ ] CHK003 - Are navigation requirements clear for all clickable brand elements? [Clarity, Spec §FR-010]
- [ ] CHK004 - Is the selection criteria for related episodes documented? [Gap, Spec §FR-005]
- [ ] CHK005 - Are loading state requirements defined for asynchronous episode data? [Gap]
- [ ] CHK006 - Can "visual hierarchy" requirements be objectively measured? [Measurability, Spec §FR-001]
```

**Key Differences:**

- Wrong: Tests if the system works correctly
- Correct: Tests if the requirements are written correctly
- Wrong: Verification of behavior
- Correct: Validation of requirement quality
- Wrong: "Does it do X?"
- Correct: "Is X clearly specified?"
