I need help with this Phase 1.

Basically, I probably need to have the "Process" in two stages. Right now, LLM begins fully materializing #### Reference USS — User Story Subtemplate right away, which is pointless until the story set is accepted. Basically, I probably want LLM first come up with a candidate user story set without outputting it first. Then, LLM must perform a full revision of the first variant against all Phase 1 general rules and Completion Criteria, which only relate to user story decomposition. The LLM must be critical and objective when assessing variant 1, assuming there is at least 50% chance the first version is not optimal. The LLM should then revise and make the second set. This second set is then presented to user for approval as 

```markdown
## User Stories

| #   | User Story               | Brief Scope         |
| --- | ------------------------ | ------------------- |
| 1   | US1 — [User Story Name]  | [Terse scope label] |
| 2   | US2 — [User Story Name]  | [Terse scope label] |
| ... | ...                      | ...                 |
```

The `Brief Scope` column MUST be terse and analysis-oriented. It MUST contain only the minimum text needed to quickly identify the story boundary. Full sentences are not required.

Then, story decomposition may be revised iteratively until accepted. Only then the LLM should complete the full Phase 1.

---
---

Yes. Add an explicit **Phase 1A / 1B split**. Your current file already says Phase 1 output is a “candidate / revised user story set and preliminary SSS” and that full USS sections are required only for Phase 1 completion, so this change fits the existing structure.  

Replace **`#### Process`** under **Phase 1** with this:

`````markdown
#### Process

Phase 1 MUST be performed in two stages:

1. **Stage 1 — Candidate Decomposition and Critical Revision**
2. **Stage 2 — Full Phase 1 Materialization**

The LLM MUST NOT fully materialize `Reference USS — User Story Subtemplate` during Stage 1.

---

##### Stage 1 — Candidate Decomposition and Critical Revision

The LLM MUST first perform candidate decomposition internally.

The LLM MUST:

- analyze the target system and identify major capabilities;
- create an initial candidate user story set;
- assume there is at least a 50% probability that the first candidate set is not optimal;
- critically evaluate the initial candidate set against all Phase 1 General Rules, and Completion Criteria that relate to user story decomposition;
- identify:
    - invalid candidates;
    - missing user stories;
    - improper granularity;
    - weak cohesion;
    - invalid ordering;
    - unjustified separation;
    - unjustified grouping;
    - ambiguities that materially affect decomposition;
- revise the initial candidate set into a second candidate set before presenting it to the user.

The LLM MUST be critical, objective, and adversarial toward its first decomposition attempt.

The LLM MUST NOT present the first candidate set unless explicitly asked.

The LLM MUST NOT output full user story subtemplates during Stage 1.

The LLM MUST present the revised candidate user story set using only the following format:

```markdown
## User Stories

| #   | User Story               | Brief Scope         |
| --- | ------------------------ | ------------------- |
| 1   | US1 — [User Story Name]  | [Terse scope label] |
| 2   | US2 — [User Story Name]  | [Terse scope label] |
| ... | ...                      | ...                 |
```

The `Brief Scope` column MUST be terse and analysis-oriented. It MUST contain only the minimum text needed to quickly identify the story boundary. Full sentences are not required.

After presenting the revised candidate set, the LLM MUST ask the user to accept or revise the decomposition.

If ambiguities remain that materially affect decomposition, the LLM MUST include targeted clarification questions with options sorted in descending suitability and a recommended answer set.

This Stage 1 process MUST repeat until the user accepts the candidate user story set.

---

##### Stage 2 — Full Phase 1 Materialization

After the user accepts the candidate user story set, the LLM MUST complete Phase 1 by fully materializing the accepted stories and preliminary SSS.

The LLM MUST:

* generate the preliminary SSS;
* fully expand `Reference USS — User Story Subtemplate` for every accepted user story;
* complete all Phase 1-required user story sections:
  * Description;
  * Shared System Semantics References;
  * Scope;
  * Included Behavior;
  * State Interaction;
* ensure every accepted user story satisfies all Phase 1 rules;
* ensure cross-cutting behavior is promoted to SSS instead of duplicated locally;
* ensure all user stories reference applicable SSS sections or rules.

Acceptance Scenarios and Exception Scenarios MAY remain preliminary during Phase 1, but they MUST be present in the full USS structure and MUST be completed and aligned during Phase 2.

The LLM MUST NOT begin Phase 2 until:

* the full Phase 1 materialization is complete;
* the preliminary SSS is complete;
* the user accepts the fully materialized Phase 1 output.

---

##### Iteration Rule

If the user revises the story decomposition during Stage 1, the LLM MUST repeat Stage 1 review before presenting the next candidate table.

If the user revises the fully materialized Phase 1 output during Stage 2, the LLM MUST update the affected SSS and user story sections and revalidate Phase 1 before asking for acceptance again.

````

I would also revise **Output Mode by Phase** from:

```markdown
- Phase 1 output: candidate / revised user story set and preliminary SSS.
````

to:

```markdown
- Phase 1 Stage 1 output: revised candidate user story summary table only.
- Phase 1 Stage 2 output: preliminary SSS and fully expanded accepted user stories.
```

This cleanly prevents premature USS expansion while preserving your strict schema requirement for completed Phase 1.
