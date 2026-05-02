---
url: https://chatgpt.com/c/69f5cb12-6d14-83eb-ab10-a57b41b1aa71
---
## Capability Decomposition

The LLM MUST decompose the target scope into a set of high-level user-centric capability anchors.

A **target scope** is the described product, system, feature area, extension, change, or project evolution being analyzed. It MAY represent a complete new system or a bounded change to an existing system.

A **capability anchor** is a coarse functional area that reflects something an end user would recognize, intentionally use, access, rely on, or care about.

Capability anchors create a concise user-centric map of the target scope. They identify the major kinds of value, access, behavior, or experience described or strongly implied by the target scope.

---

### Purpose

The capability anchor set MUST establish a compact, user-facing representation of the target scope.

The LLM MUST identify:

- major user-recognizable capability areas;
- the user value or functional intent of each capability;
- specified or strongly implied usability, access, launch, delivery, environment, or runtime concerns;
- natural capability boundaries indicated by the target description;
- capability areas that are distinct enough that combining them would obscure user intent.

---

### Derivation Rules

The LLM MUST derive capability anchors from:

- the target description and explicitly stated behavior;
- strongly implied user-facing behavior;
- specified usability, access, launch, delivery, environment, or runtime requirements;
- structural completeness expectations implied by the kind of target scope being described.

The capability anchor set MUST:

- cover all meaningful user-facing capabilities from the target description;
- cover specified or strongly implied usability, access, launch, delivery, environment, and runtime aspects;
- describe cohesive user-facing capability areas;
- translate architectural, deployment, or delivery requirements into user-centric terms based on how the user accesses, launches, uses, or experiences the system;
- prefer end-user value and functional intent over implementation structure;
- remain coarse enough to avoid becoming a list of isolated actions;
- remain specific enough to make capability coverage and boundaries inspectable.

The capability anchor set MUST NOT:

- copy the target description as a single broad capability;
- collapse unrelated capabilities merely because they were mentioned together;
- split capabilities into isolated operations or low-level actions;
- group potentially related capabilities when the target description indicates distinct user intent, access mode, environment, or delivery expectation;
- describe internal mechanisms unless they directly affect user-visible access, behavior, or experience;
- include implementation plans, validation scenarios, acceptance criteria, task lists, or sequencing.

---

### Capability Boundary Rules

When deciding whether to split or group capability anchors, the LLM MUST evaluate the following factors in order:

1. **User-recognizable intent** — whether users would understand the behaviors as serving the same purpose.
2. **User-facing experience** — whether the behaviors are used, accessed, or experienced as part of the same broad activity.
3. **Domain cohesion** — whether the behaviors belong to the same conceptual capability area.
4. **Access or environment distinction** — whether the target description indicates materially different access, launch, delivery, runtime, or environment expectations.
5. **Coverage clarity** — whether the anchor makes the target description easier to inspect for omitted, overloaded, or conflated capability areas.

The LLM SHOULD split capability anchors when:

- users would perceive the areas as different reasons to use the system;
- the target description signals distinct access, launch, delivery, environment, or runtime expectations;
- one capability represents baseline use and another represents a materially different extension of use;
- grouping would hide a meaningful capability boundary;
- grouping would create a vague umbrella that merely restates the target description.

The LLM SHOULD group capability anchors when:

- the behaviors serve the same user-recognizable purpose;
- separation would merely mirror implementation structure;
- separation would create anchors that are too narrow to represent meaningful user-facing capability areas;
- the distinction is only a variation within the same broader user intent.

---

### Required Output Format

The LLM MUST present the capability anchor set using only the following format:

```markdown
## Capability Anchors

- **[Capability Name]** — [End-user value / functional intent].  
  Scope signal: [Brief statement of what kinds of behavior, access, experience, or environment concern this capability includes].

- **[Capability Name]** — [End-user value / functional intent].  
  Scope signal: [Brief statement of what kinds of behavior, access, experience, or environment concern this capability includes].
````

Each capability anchor MUST:

* use a concise user-facing name;
* state the end-user value or functional intent;
* include a `Scope signal` that briefly identifies what belongs under the capability;
* avoid implementation-first naming unless the target requirement is itself about user access, launch, delivery, runtime, or environment experience;
* avoid low-level action names when a broader user-recognizable capability name is available.

The `Scope signal` MUST be brief and boundary-oriented.

The `Scope signal` MUST NOT contain:

* implementation steps;
* validation scenarios;
* acceptance criteria;
* task details;
* sequencing;
* exhaustive behavior lists.

---

### Validation

After producing the capability anchor set, the LLM MUST produce the following validation block:

```markdown
### Capability Anchor Validation Result

- ✅ / ❌ Target-description coverage checked.
- ✅ / ❌ User-facing capability areas checked.
- ✅ / ❌ Usability, access, launch, delivery, environment, and runtime aspects checked where applicable.
- ✅ / ❌ No capability anchor merely restates the full target scope.
- ✅ / ❌ No capability anchor is merely an implementation mechanism.
- ✅ / ❌ No unrelated capabilities are grouped without justification.
- ✅ / ❌ No capability anchor is split into isolated low-level actions.
- ✅ / ❌ Capability boundaries are clear enough for use.

Result: Valid / Invalid.
```

If any validation item fails, the LLM MUST revise the capability anchor set before returning the output.

---

### Completion Criteria

This analysis is complete only when:

* a capability anchor set has been produced;
* all meaningful user-facing capabilities from the target description are represented;
* specified or strongly implied usability, access, launch, delivery, environment, and runtime aspects are represented where applicable;
* every capability anchor is user-centric and functionally cohesive;
* no capability anchor merely restates the full target scope;
* no capability anchor is merely an implementation mechanism;
* no capability anchor is split into isolated low-level actions;
* capability boundaries are clear and inspectable;
* the validation result is passing.
