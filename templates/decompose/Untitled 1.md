The LLM MUST derive a small set of high-level user-centric capability anchors from the target description. A capability anchor is a coarse functional area that reflects something an end user would recognize, intentionally use, or care about.

The capability anchor set MUST:

- cover all meaningful capabilities as well as specified or highly implied usability, access, and environment aspects from the target description;
- describe cohesive user-facing capability areas, not isolated implementation mechanisms;
- translate indicated architectural, deployment, or delivery requirements into user-centric terms based on how the user accesses, launches, or interacts with the system;
- avoid grouping unrelated capabilities merely because they were mentioned together;
- prefer end-user value and functional intent over implementation structure.

Each capability anchor MUST be expressed as a concise bullet using this form:
```markdown
- **[Capability Name]** — [End-user value / functional intent].  
  Scope signal:[brief statement of what kinds of behavior this capability includes and excludes].
```

***

## Target

I want to build an RPN calculator web app and have a packaged desktop version later on. I am thinking of supporting add, sub, neg, mul, div, sqr, sqrt, inv, pow, abs, exp, and ln functions. I also want to have undo capability




###### Stage 1A — User-Centric Capability Anchor Derivation

The LLM MUST derive a small set of high-level user-centric capability anchors from the target description. A capability anchor is a coarse functional area that reflects something an end user would recognize, intentionally use, or care about.

The capability anchor set MUST:

- cover the meaningful capabilities explicitly or strongly implied by the target description;
- remain small in number;
- describe cohesive user-facing capability areas, not isolated implementation mechanisms;
- avoid grouping unrelated capabilities merely because they were mentioned together;
- prefer end-user value and functional intent over implementation structure;

Each capability anchor MUST be expressed as a concise bullet using this form:

```markdown
- **[Capability Name]** — [End-user value / functional intent].  
  Scope signal: [brief statement of what kinds of behavior this capability includes and excludes].
```






The LLM MUST use the capability anchors as the primary reference when creating candidate user stories.

When deriving user stories from capability anchors, the LLM MUST:

1. map each candidate user story to one primary capability anchor;
2. split a capability anchor into multiple user stories only when distinct minimal increments are justified by:

   * materially different user interactions;
   * materially different domain or edge-class semantics;
   * materially different state interaction;
   * necessary incremental viability or delivery sequencing;
3. merge candidate stories when they are merely fragments of the same user-recognizable capability and splitting would create unnecessary duplication, weak cohesion, or artificial sequencing;
4. reject candidate stories that cannot be traced to a user-centric capability anchor unless they are explicitly classified as:

   * SSS concern;
   * internal mechanism;
   * later feature-synthesis concern;
   * out of scope.

The LLM MUST NOT use capability anchors as final user stories unless the anchor itself satisfies all Phase 1 user story rules.

The LLM MUST NOT group user stories primarily by:

* implementation layer;
* UI component;
* data structure;
* operator arity;
* framework/package/runtime concern;
* input wording order;
* convenience of summarization.

After deriving candidate user stories, the LLM MUST validate the candidate set against the capability anchors.

The validation MUST ensure that:

* every capability anchor is represented by one or more justified user stories, promoted to SSS, deferred to feature synthesis, or explicitly rejected as out of scope;
* every candidate user story has a clear primary capability anchor;
* no user story crosses multiple capability anchors without a cohesion justification;
* no capability anchor has been split into multiple stories without a decomposition justification;
* no story grouping contradicts the user-centric capability structure.

If this validation identifies weak, missing, overly broad, overly narrow, or mechanically grouped user stories, the LLM MUST revise the candidate user story set before presenting it to the user.

---

Because your current Stage 1 output contract says “summary table only,” you should also revise it if you want this anchor visible to the user. Use this replacement:

```markdown
The LLM MUST present the revised Stage 1 result using only the following format:

```markdown
## Capability Anchors

- **[Capability Name]** — [End-user value / functional intent].  
  Scope signal: [brief included/excluded behavior boundary].
- **[Capability Name]** — [End-user value / functional intent].  
  Scope signal: [brief included/excluded behavior boundary].

## User Stories

| #   | User Story               | Primary Capability Anchor | Brief Scope         |
| --- | ------------------------ | ------------------------- | ------------------- |
| 1   | US1 — [User Story Name]  | [Capability Name]         | [Terse scope label] |
| 2   | US2 — [User Story Name]  | [Capability Name]         | [Terse scope label] |
| ... | ...                      | ...                       | ...                 |
````

The `Primary Capability Anchor` column MUST identify the capability anchor from which the user story is derived.

The `Brief Scope` column MUST be terse and analysis-oriented. It MUST contain only the minimum text needed to quickly identify the story boundary. Full sentences are not required.

