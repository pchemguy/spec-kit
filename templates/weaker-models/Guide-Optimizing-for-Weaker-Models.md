---
urls:
  - https://chatgpt.com/c/69f96a84-5094-83eb-b066-c38ba9399c51
  - https://aistudio.google.com/prompts/1BIU5woWf8hjsqa8Hr4qL0WmVmfBr4lcA
---

> [!NOTE] Prompt
> 
> Give me a detailed guide on how to optimize performance of prompts for weaker/runner-up/subpar models as opposed to SOTA LLM.

# AI Studio

When working with State-of-the-Art (SOTA) LLMs like GPT-4o, Claude 3.5 Sonnet, or Gemini 1.5 Pro, you can often get away with lazy prompting. They can infer intent, parse convoluted instructions, perform implicit reasoning, and ignore irrelevant context. 

Weaker, smaller, or "runner-up" models (like 7B-8B parameter open-weight models, older API models, or heavily quantized local models) lack this cognitive elasticity. They take instructions literally, suffer from severe recency bias, get easily distracted, and struggle with multi-step reasoning.

To bridge the gap and achieve SOTA-level performance from weaker models, you must shift from being a **"casual conversationalist"** to a **"strict micromanager."** 

Here is a detailed guide on how to optimize prompts for weaker LLMs.

---

### 1. Instruction Design: Simplicity and Atomicity

SOTA models can handle "Compound Prompts" (e.g., "Summarize this article, extract the key entities, translate them to French, and format as JSON"). Weaker models will collapse under this cognitive load, usually forgetting 2-3 of the steps.

* **Prompt Chaining (The Assembly Line):** Break complex tasks down into single, atomic tasks. Instead of one prompt doing four things, use four separate prompts in a pipeline. Pass the output of Model A into the prompt of Model B.
* **Positive Framing:** Weaker models struggle with negative constraints ("Do not include an introduction," "Do not hallucinate"). They often focus on the negative word and do it anyway. Frame constraints positively.
    * *Bad:* "Do not write an introduction or conclusion."
    * *Good:* "Start your response directly with the first bullet point. End your response immediately after the last bullet point."
* **Explicit Step-by-Step Directives:** Don't just ask for the result; outline the exact algorithmic steps the model should take to get there.

### 2. Formatting and Delimiters: Creating Guardrails

Weaker models have a hard time distinguishing between instructions, context, and data. You must provide rigid structural boundaries.

* **Use XML Tags or Markdown:** Enclose all reference data in clear XML tags. Smaller models have been heavily trained on structured text and respond well to this.
    ```text
    Read the following article enclosed in <article> tags.
    <article>
    [Text here]
    </article>
    Now, extract the names...
    ```
* **Template the Expected Output:** Do not leave the output format up to the model's imagination. Provide an exact, empty template for it to fill out.
    ```text
    Respond ONLY using the following template:
    Name: [Extract Name]
    Age: [Extract Age]
    Occupation: [Extract Occupation]
    ```

### 3. Exploiting Few-Shot Prompting

While SOTA models excel at Zero-Shot execution (following abstract instructions without examples), weaker models are primarily **pattern-matching engines**. 

* **The Power of Few-Shot:** Providing 2 to 5 highly accurate input/output examples is the single most effective way to improve a weaker model's performance. It overrides the model's instruction-following limitations by allowing it to simply mimic the pattern.
* **Diversity in Examples:** Ensure your examples cover edge cases. If you only provide examples of "Positive" and "Negative" sentiment, the model will fail when it encounters a "Neutral" text unless you provide a "Neutral" example.

### 4. Context and Payload Management

Smaller context windows and weak attention mechanisms mean smaller models suffer heavily from the "Lost in the Middle" phenomenon. 

* **Recency Bias is Your Friend:** Weaker models pay the most attention to the very end of the prompt. **Always put your final, most crucial instruction at the very bottom**, after the context/payload. 
    * *Bad:* [Instructions] -> [Long Document]
    * *Good:* [Instructions] -> [Long Document] -> [Reiterate core Instruction]
* **Ruthless Context Trimming:** SOTA models can find a needle in a haystack. Weaker models get distracted by the hay. Pre-process your text to remove boilerplate, irrelevant paragraphs, or noisy metadata before feeding it to the model.
* **System Prompts vs. User Prompts:** Many weaker open-source models actually ignore or pay little attention to the System Prompt. If a rule is absolute, put it in the User Prompt.

### 5. Enhancing Reasoning

You cannot trust a 7B model to do complex math or logic in a single leap. You must force it to "think out loud" to generate the necessary tokens for reasoning.

* **Mandatory Chain-of-Thought (CoT):** Force the model to explain its reasoning *before* it outputs the final answer.

    ```text
    Step 1: Analyze the text and write down the key arguments in <scratchpad> tags.
    Step 2: Evaluate if the arguments are logical inside <evaluation> tags.
    Step 3: Provide your final Yes/No answer inside <final_answer> tags.
    ```

* **Pre-filling the Assistant Response:** If you are using an API that allows you to supply the beginning of the Assistant's message, use it to force the model straight into the desired format or reasoning path, bypassing conversational filler ("Sure, I can help with that!").
    * *User:* "Extract the data to JSON."
    * *Assistant (Pre-filled):* `{ "name": "` 
    *(The model has no choice but to immediately start generating the JSON value).*

### 6. Grammar and Output Constraining

If you need strict JSON, code, or specific categorical outputs from a weaker model, prompting alone might not be enough. 

* **Use Logit Bias / Grammar Constraining:** If you are running the model locally (e.g., via vLLM, llama.cpp, or Ollama) or using a structured output API, use tools like **JSON Schema enforcement** or **Outlines**. These act on the inference engine level, physically preventing the model from outputting tokens that don't match your schema, guaranteeing 100% perfect formatting regardless of the model's intelligence.

### Summary Checklist for Weaker Models

1. [ ] Is the prompt asking for only **one** thing? (If no, split it into multiple prompts).
2. [ ] Are there **few-shot examples** showing exactly what I want?
3. [ ] Is the input data separated by **XML tags**?
4. [ ] Is the exact **output format/template** explicitly written out?
5. [ ] Did I remove all **"Do not"** statements and replace them with positive directives?
6. [ ] Is the core instruction repeated at the **very bottom** of the prompt?
7. [ ] Have I forced the model to **"think step-by-step"** before giving the final answer?

---
---

# Guide: Optimizing Prompts for Weaker, Runner-Up, or Subpar Models

A prompt that works well on a frontier model often fails on weaker models because the stronger model silently compensates for ambiguity, missing structure, weak ordering, implied constraints, and overloaded instructions.

For weaker models, the goal is not to make the prompt "more sophisticated." The goal is to make the prompt **more executable**.

A strong prompt for weaker models should behave less like a polished policy memo and more like a clear operating procedure.

---

## 1. Understand the Difference Between SOTA and Weaker Models

### SOTA models tolerate ambiguity

A top model can often infer:

* what matters most;
* which instruction overrides another;
* what the user meant by an informal phrase;
* how to preserve structure across long outputs;
* when a rule applies and when it does not;
* how to repair underspecified templates;
* how to avoid over-fragmenting a task;
* how to produce reasonable intermediate abstractions.

A weaker model may not.

It may:

* follow the most recent instruction rather than the most important one;
* overfit to examples;
* ignore constraints buried in prose;
* collapse multiple phases into one;
* skip validation;
* imitate formatting but miss semantics;
* produce verbose filler instead of controlled reasoning;
* treat every bullet as equal;
* split too much or merge too much;
* fail to distinguish "derive," "generate," "validate," and "revise."

So the prompt must compensate.

---

## 2. Core Principle: Reduce Inference Load

The weaker the model, the less implicit reasoning you should require.

Bad for weaker models:

```markdown
Analyze the project and produce a good user story decomposition.
```

Better:

```markdown
First identify the major user-facing capabilities.
Then group behaviors by user-recognizable intent.
Then create candidate user stories from those groups.
Each user story must describe one user-initiated interaction and one observable outcome.
Do not create separate stories that differ only by minor operation variants.
```

The second version gives the model a route through the task.

Weaker models often fail not because they lack knowledge, but because they lack reliable **task control**.

---

## 3. Use Natural Language, But Make the Control Flow Explicit

For weaker models, natural language is fine. In fact, overly legalistic prompts can hurt them.

But the natural language must be procedural.

Instead of:

```markdown
The model should produce a semantically coherent and minimally sufficient decomposition.
```

Use:

```markdown
Group things together when a user would reasonably think of them as one capability.
Split things only when they represent different user goals, different access paths, or clearly different usage contexts.
```

That is easier to execute.

The ideal style is:

* plain;
* direct;
* imperative;
* low-abstraction;
* concrete;
* repetitive where necessary;
* structured in execution order.

---

## 4. Prefer Simple Verbs Over Abstract Directives

Weaker models respond better to action verbs.

Use:

* identify;
* list;
* group;
* split;
* compare;
* revise;
* remove;
* keep;
* check;
* output.

Avoid relying heavily on:

* synthesize;
* reason about;
* optimize;
* ensure coherence;
* elaborate;
* reconcile;
* operationalize;
* semantically align.

Those words are not bad, but weaker models often treat them as decorative rather than executable.

Better:

```markdown
Check each bullet. If it repeats the whole input, rewrite it smaller. If it is only a low-level task, merge it into a larger capability.
```

Worse:

```markdown
Ensure each capability is appropriately scoped and semantically cohesive.
```

---

## 5. Put the Main Objective Early and Plainly

Weak models often anchor on the first strong instruction. Use that.

Bad:

```markdown
You are an expert software architect...
[long role description]
[long context]
[definitions]
[constraints]
Your task is to produce...
```

Better:

```markdown
Your task is to identify the main user-facing capabilities described by the target description.

Do not create user stories yet.
Do not create implementation tasks.
Do not copy the whole input as one capability.
```

Then add role/context after that if needed.

For weaker models, the first 5-10 lines matter disproportionately.

---

## 6. Use Negative Instructions Sparingly, But Explicitly

Weak models often violate unstated exclusions. So exclusions are useful.

However, too many negative rules can create noise.

Prefer a small number of high-value prohibitions.

Example:

```markdown
Do not create user stories yet.
Do not list implementation tasks.
Do not make one bullet that merely restates the whole input.
Do not split every small operation into its own capability.
```

This is much better than a broad abstract rule like:

```markdown
Avoid premature decomposition and implementation-level granularity.
```

The latter may be elegant, but weaker models may not operationalize it.

---

## 7. Separate "What to Think About" From "What to Output"

This is one of the most important techniques.

Weaker models often dump their intermediate reasoning directly into the final answer. If you want internal steps but clean output, say so plainly.

Example:

```markdown
Work in this order:

1. Draft a preliminary set of capability bullets.
2. Check whether the bullets are too broad, too narrow, duplicated, or implementation-focused.
3. Revise the set.
4. Output only the final table.

Do not output the preliminary set.
Do not output the review notes.
```

This helps the model do a process without bloating the response.

For very weak models, though, you may want visible intermediate output because hidden self-checking is unreliable:

```markdown
First output a preliminary bullet list.
Then output a revised final table.
```

Use visible steps when you want auditability. Use hidden steps when you want compact output.

---

## 8. Give the Model a Small Decision Procedure

Weak models need decision rules, not just criteria.

Bad:

```markdown
Create cohesive capability anchors.
```

Better:

```markdown
For each possible capability, ask:

1. Would a user recognize this as something they can do, use, access, or rely on?
2. Is it broader than a single low-level action?
3. Is it narrower than the whole product?
4. Does it group behaviors that belong together from the user's point of view?

Keep the capability only if all answers are yes.
```

This turns a fuzzy quality standard into a checklist.

---

## 9. Use "Keep / Split / Merge / Remove" Classification

For decomposition tasks, weaker models benefit from explicit revision moves.

Example:

```markdown
When reviewing the preliminary set, classify each bullet as:

- Keep - it is clear and properly scoped.
- Split - it contains different user goals or usage contexts.
- Merge - it overlaps strongly with another bullet.
- Remove - it is only an implementation detail or duplicate.
- Rewrite - it is valid but badly phrased.
```

This helps prevent the common failure where the model says "looks good" without actually revising.

---

## 10. Avoid Overloaded Terms Unless You Define Them Operationally

Terms like these are useful but dangerous:

* capability;
* feature;
* user story;
* semantic coverage;
* abstraction level;
* cohesion;
* MVP;
* scope;
* actor;
* acceptance criteria.

A strong model usually understands the intended methodology. A weaker model may not.

Define terms in behavioral language.

Example:

```markdown
A capability is a high-level thing the end user would care about, use, or rely on.

It is not:
- a code module;
- a UI widget;
- a single button click;
- a technical implementation task;
- a copy of the full project description.
```

That is much more robust than a formal definition alone.

---

## 11. Use Examples Carefully

Examples are powerful but dangerous for weaker models.

They may imitate the example too literally.

### Good example design

Use examples that illustrate the boundary, not the domain-specific answer.

Example:

```markdown
Example of good capability wording:
- Manage saved notes
- Search and filter records
- Export results for later use

Example of bad capability wording:
- Build a React component
- Add a submit button
- Use PostgreSQL
- Implement all app functionality
```

### Avoid examples that are too close to the target

If your target is an RPN calculator, an example about calculators may cause copying.

Better to use unrelated examples unless you explicitly want analogical transfer.

---

## 12. Prefer Output Schemas Over Freeform Answers

Weak models are much more reliable when the output shape is fixed.

Example:

```markdown
Output exactly this table:

| Number | Capability | Supporting Keywords |
| --- | --- | --- |
| 1 | ... | ... |
```

Even better:

```markdown
Rules for the table:
- Number capabilities starting from 1.
- Capability must be a terse noun phrase or short verb phrase.
- Supporting Keywords must contain only words or phrases from the original description.
- Do not add a notes section after the table.
```

A weak model may still add commentary, but this reduces drift.

---

## 13. Put Format Rules Near the Output Template

Do not define the format far above the place where the model outputs it.

Weak models forget.

Better:

```markdown
Return the final answer using exactly this format:

## Capability Anchors

| Number | Capability | Supporting Keywords |
| --- | --- | --- |
| 1 | ... | ... |

Do not include any other sections.
```

The closer the constraint is to the output point, the better.

---

## 14. Use Redundant Constraints Intentionally

For weaker models, some repetition is good.

Example:

```markdown
Do not create user stories yet.

This step is only about high-level capabilities.

The final output must not include user stories, acceptance criteria, implementation tasks, or feature breakdowns.
```

A SOTA model may find that repetitive. A weaker model may need it.

The trick is to repeat only critical constraints, not every detail.

---

## 15. Avoid Deep Nesting

Weak models often lose nested hierarchy.

Bad:

```markdown
For each stage, perform the following validation except when operating in compact mode unless the prior stage generated a provisional artifact...
```

Better:

```markdown
Use this order every time:

1. Create preliminary candidates.
2. Review the candidates.
3. Revise them.
4. Output the final result.

There is no compact mode for this task.
```

Deep exception structures are especially fragile.

---

## 16. Avoid Too Many "Unless" Clauses

Weak models struggle with conditional instruction stacks.

Bad:

```markdown
Do X unless Y, except when Z, in which case do A unless B has already occurred.
```

Better:

```markdown
Default rule: Do X.

Exception:
Use A only when Z is explicitly provided.

Do not infer Z.
```

For subpar models, make exceptions rare, named, and isolated.

---

## 17. Use Priority Ordering Explicitly

Weak models do not reliably infer which rules matter most.

Use priority tiers.

Example:

```markdown
Instruction priority:

1. Follow the active task only.
2. Follow the required output format.
3. Preserve user-facing meaning from the target description.
4. Improve wording only as much as needed.
5. Avoid extra commentary.
```

This is especially useful when a prompt contains both creativity and constraints.

---

## 18. Keep Each Prompt Stage Narrow

A common mistake is asking weaker models to do the full pipeline at once:

```markdown
Derive capabilities, create user stories, generate shared semantics, synthesize features, produce roadmap, and validate everything.
```

A strong model may handle it. A weak model will likely collapse or hallucinate structure.

Instead, stage it:

1. Capability anchors.
2. Candidate user stories.
3. Expanded user stories.
4. Shared semantics.
5. Feature grouping.
6. Roadmap.
7. Validation.

Each stage should have:

* one purpose;
* one output shape;
* one validation checklist;
* clear entry and exit criteria.

---

## 19. Use "Active Task" Isolation

When prompts are long, weaker models may execute the wrong part.

Add a clear active-task marker.

Example:

```markdown
## Active Task

For this response, only perform Capability Decomposition.

Do not perform User Story Decomposition.
Do not generate Features.
Do not generate a Roadmap.
```

This helps when the prompt contains a broader workflow but only one phase should run.

---

## 20. Use Checklists That Require Concrete Confirmation

Weak models often claim compliance without checking. Still, checklists help if concrete.

Bad:

```markdown
Validate the output.
```

Better:

```markdown
Before finalizing, check:

- Does each capability describe something a user would care about?
- Is any capability just a technical implementation task?
- Is any capability merely a copy of the whole input?
- Are capabilities ordered from most general to most specific?
- Are supporting keywords taken from the original description?

Revise before output if any answer fails.
```

If visible validation is desired:

```markdown
After the table, output:

Validation:
- User-facing: Pass/Fail
- Not implementation-only: Pass/Fail
- Not whole-input restatement: Pass/Fail
- Ordered general-to-specific: Pass/Fail
```

For production prompts, I often prefer visible validation during development and hidden validation once the prompt is stable.

---

## 21. Use Small, Local Quality Tests

Instead of abstract global standards, include local tests.

Example for capability bullets:

```markdown
A good capability bullet should pass this sentence:

"This program helps users [capability]."
```

Example:

```markdown
"This program helps users perform RPN calculations." ✅
"This program helps users use React state." ❌
"This program helps users build the whole calculator." ❌
```

This gives weak models a linguistic test they can actually apply.

---

## 22. Use "Too Broad / Too Narrow" Boundaries

Weak models often produce either:

* one giant bullet; or
* too many tiny bullets.

So name both failure modes.

Example:

```markdown
Avoid both extremes:

Too broad:
- Build a complete RPN calculator

Too narrow:
- Press the add button
- Press the subtract button
- Press the multiply button

Better:
- Perform stack-based arithmetic operations
```

This is highly effective.

---

## 23. Tell the Model What Counts as "Same Kind of Thing"

For grouping tasks, weak models need grouping axes.

Example:

```markdown
Group behaviors together when they share the same user intent and mental model.

For example:
- add, subtract, multiply, and divide may belong together as arithmetic operations;
- undo may be separate because it is about correcting prior actions;
- desktop packaging may be separate because it is about accessing the app in a different environment.
```

This prevents arbitrary grouping by implementation or lexical similarity.

---

## 24. Use "Original Description Grounding"

Weak models hallucinate adjacent capabilities. Ground them.

Example:

```markdown
Only derive capabilities from:
- explicit behavior in the target description;
- behavior strongly implied by making that explicit behavior usable;
- access or delivery requirements explicitly mentioned.

Do not add unrelated capabilities just because similar applications often have them.
```

This matters especially for sparse project seeds.

---

## 25. Separate Explicit, Implied, and Added Content

If the task involves elaboration, make the provenance explicit.

Example:

```markdown
Classify each item as:

- Explicit - directly stated in the target description.
- Strongly implied - required to make stated behavior usable.
- Optional addition - plausible but not required.

Do not include optional additions in the final scope unless the user asks for expansion.
```

Weak models otherwise tend to overbuild.

---

## 26. Avoid Asking for "Best" Without Defining the Objective

Weaker models often interpret "best" as "most comprehensive" or "most polished."

Instead of:

```markdown
Give the best feature breakdown.
```

Use:

```markdown
Give a small feature breakdown optimized for:
1. user-visible value;
2. minimal implementation increments;
3. easy validation;
4. avoiding tiny operation-by-operation stories.
```

The optimization target must be explicit.

---

## 27. For Weaker Models, Prefer "Least Necessary Sophistication"

A prompt for weak models should not sound dumber, but it should require less abstraction.

Compare:

```markdown
Derive a capability model that functions as a semantic reference for downstream user story boundary validation.
```

Versus:

```markdown
Before writing user stories, list the main things the user would care about.

These bullets will be used later to check whether the user stories cover the right areas and are not split in strange ways.
```

The second is less formal but more robust.

---

## 28. Avoid Prompt Architecture That Requires Meta-Reasoning

Weak models struggle with instructions like:

```markdown
This framework may be activated as a system prompt, developer prompt, slash command, reusable skill, or embedded workflow. Activation method MUST NOT change semantics.
```

That may be useful for advanced agents, but it burdens weaker models.

For runner-up models, prefer:

```markdown
Use the following workflow whenever the user gives a target description.
```

If you need activation semantics, put them in a separate wrapper prompt, not in the task prompt.

---

## 29. Use Short Sections With Strong Headings

Weak models are sensitive to section boundaries.

Good headings:

```markdown
## Task
## Definitions
## Process
## Output Format
## Rules
## Final Check
```

Less good:

```markdown
## Methodological Considerations
## Semantic Operating Model
## Generative Constraints
```

For weaker models, headings should say exactly what the section does.

---

## 30. Prefer Numbered Steps for Processes

Use numbered steps when order matters.

Use bullets when order does not matter.

Example:

```markdown
Process:

1. Read the target description.
2. Identify candidate capabilities.
3. Remove implementation-only items.
4. Merge overlapping items.
5. Sort from most general to most specific.
6. Output the final table.
```

This is much more reliable than prose paragraphs.

---

## 31. Put "Do Not Continue" Gates in Multi-Phase Workflows

If you want human approval, say:

```markdown
After outputting the candidate table, stop.

Do not continue to the next phase until the user explicitly accepts the table.
```

Weak models otherwise continue eagerly.

For even more reliability:

```markdown
The final line of your response must be:

Please reply `accepted` to continue, or provide corrections.
```

This creates a hard conversational checkpoint.

---

## 32. Use "One Response = One Deliverable"

For weaker models, do not ask for multiple artifacts unless necessary.

Bad:

```markdown
Output the candidate user stories, detailed acceptance criteria, semantic audit, revised stories, and feature roadmap.
```

Better:

```markdown
In this response, output only the candidate user story summary table.
```

Then continue in later turns.

This is especially important when you need schema fidelity.

---

## 33. Avoid Long Lists of Equal Rules

A 40-rule prompt may look rigorous, but weaker models will not preserve all rules.

Group rules into a few practical clusters:

```markdown
Rules:

1. Stay user-facing.
2. Stay grounded in the description.
3. Avoid both over-broad and over-small bullets.
4. Sort from general to specific.
5. Use the required table format.
```

If you need many rules, turn them into staged checks instead of one large rule list.

---

## 34. Make Error Correction Explicit

Weak models are often poor at self-correction unless told exactly how to revise.

Example:

```markdown
If a capability is too broad, split it into smaller user-facing capabilities.
If a capability is too narrow, merge it with a related capability.
If a capability is implementation-only, remove it.
If two capabilities overlap, keep the clearer one and merge the supporting keywords.
```

This gives the model concrete repair actions.

---

## 35. Use Lexical Anchors From the Input

For extraction or grounding tasks, ask the model to preserve keywords.

Example:

```markdown
For each final capability, extract supporting keywords from the original description.
Use only words or short phrases that appear in the original description.
```

This has three benefits:

1. It reduces hallucination.
2. It makes the abstraction auditable.
3. It forces the model to connect output to input.

For weaker models, this is a major robustness improvement.

---

## 36. Use "Supporting Keywords" as a Traceability Mechanism

For your specific kind of workflow, this is a very strong pattern.

Example:

```markdown
| Number | Capability | Supporting Keywords |
| --- | --- | --- |
| 1 | Perform stack-based calculations | RPN calculator, add, sub, mul, div |
| 2 | Use scientific math operations | sqr, sqrt, pow, abs, exp, ln |
| 3 | Correct mistakes during calculation | undo capability |
| 4 | Use the calculator in web and desktop environments | web app, packaged desktop version |
```

This table creates a lightweight semantic reference without requiring the model to generate full user stories too early.

---

## 37. Prefer "Advertising / User Need" Framing for Capability Discovery

Your own instinct here is good: asking what the program helps users do often produces better anchors than asking for "features."

For weaker models, this is effective:

```markdown
Your objective is to describe the key functionality in a way that would attract the widest relevant set of users.

Each bullet should answer:
"What user need does this satisfy?"
```

This prevents low-level decomposition.

It also discourages implementation-centric bullets.

---

## 38. Use "Most General First" Carefully

Sorting from most general to most specific is useful, but define what general means.

Example:

```markdown
Sort the bullets from most general to most specific.

"Most general" means the capability that would matter to the largest number of target users and covers the broadest central use of the program.

It does not mean vague.
```

Without that clarification, weaker models may put an empty umbrella statement first.

---

## 39. Avoid Excessive Formal Ontologies

A SOTA model may benefit from a taxonomy like:

* Core User Capability;
* Supporting Functional Capability;
* Non-Functional and Form-Factor Capability;
* Semantic Governance Capability.

A weaker model may turn that into bureaucratic noise.

For weaker models, use a simpler classification only when necessary:

```markdown
Classify each capability as one of:

- Main use - the primary thing users do with the program.
- Support - helps the main use work well.
- Access - how or where users use the program.
```

That is often enough.

---

## 40. Use "Capability" Before "User Story"

This is a good architecture for weaker models.

Why?

Because user stories require the model to make many decisions at once:

* actor;
* goal;
* value;
* boundary;
* interaction;
* state;
* acceptance criteria;
* exception behavior;
* ordering;
* granularity.

Capability anchors are easier.

They provide a reference layer.

A robust sequence is:

```markdown
1. Capability anchors
2. Candidate user stories
3. Revised user story set
4. Full user story details
5. Shared semantics
6. Feature grouping
```

Do not start with full user story materialization on weaker models.

---

## 41. Use Preliminary Generation + Critical Revision

Weaker models often produce a plausible but mediocre first answer. You can force a second pass.

Example:

```markdown
First create a preliminary bullet set.

Then review it critically. Assume the first version is probably too broad, too narrow, overlapping, or missing an important capability.

Revise it before producing the final answer.
```

This is one of the best techniques for runner-up models.

But keep the review concrete:

```markdown
Check for:
- whole-input restatement;
- operation-by-operation fragmentation;
- implementation-only bullets;
- missing access/delivery capability;
- poor ordering.
```

---

## 42. Use "Assume the First Draft Is Not Optimal"

This is surprisingly effective.

Example:

```markdown
Assume there is at least a 50% chance your first candidate set is not optimal. Review and revise it before output.
```

This prevents shallow first-pass compliance.

A strong model may not need it. A weaker one often does.

---

## 43. Do Not Over-Optimize for Polished Language

For weaker models, polished language can hide bad decomposition.

Tell the model:

```markdown
Prefer clear and accurate bullets over elegant wording.
```

Or:

```markdown
Use simple wording. Do not make the capabilities sound more advanced than the target description supports.
```

This reduces marketing fluff.

---

## 44. Use "Terse But Not Empty"

Weak models often interpret "terse" as vague.

Bad result:

```markdown
- Calculation
- Undo
- Desktop
```

Better instruction:

```markdown
Each capability must be terse but still meaningful.
Use 3-8 words when possible.
Avoid one-word labels unless the meaning is completely clear.
```

Good result:

```markdown
- Perform RPN calculations
- Correct calculation mistakes
- Use the app across environments
```

---

## 45. Avoid "Comprehensive" Unless You Really Mean It

The word "comprehensive" often causes weak models to overproduce.

Instead of:

```markdown
Give a comprehensive list of capabilities.
```

Use:

```markdown
Give a small set of the main capabilities. Prefer fewer bullets when one bullet can cover related behavior clearly.
```

For early-stage decomposition, "compact coverage" is usually better than "comprehensive detail."

---

## 46. Use Hard Cardinality Ranges When Helpful

Weak models may generate too many or too few items. Give a range.

Example:

```markdown
Produce 3-6 capability bullets.
Use fewer bullets if the target is simple.
Do not exceed 6 unless the description clearly contains more distinct capability areas.
```

For your RPN calculator example, this would avoid both one giant bullet and twelve operator-level bullets.

---

## 47. Avoid Requiring Perfect Exhaustiveness Too Early

At early stages, weaker models may overcompensate for "cover everything" by fragmenting everything.

Better:

```markdown
Cover the main capability areas. Details can be handled later when generating user stories.
```

This helps the model stay high-level.

---

## 48. Use "One Suitable Bullet Only" for Keyword Assignment

Your wording here is strong:

```markdown
Prefer having each extracted keyword matched with the most suitable bullet only.
```

This prevents the model from dumping every keyword into every row.

You can make it slightly stronger:

```markdown
Do not repeat a supporting keyword across multiple rows unless it truly supports more than one capability.
```

---

## 49. Use "Original Words" for Keywords, But Allow Short Phrases

This makes the output auditable.

Example:

```markdown
Supporting Keywords must be exact words or short phrases from the original description.
Do not invent new supporting keywords.
```

This reduces semantic drift.

---

## 50. Distinguish "Capability Name" From "Scope Signal"

For more advanced workflows, a capability bullet alone may be too terse. Add a scope signal.

Example:

```markdown
Capability: Perform RPN calculations
Scope signal: Covers stack-based operand entry and arithmetic evaluation.
```

For weaker models, this can be better than asking for a rich paragraph.

It gives just enough detail to check boundaries.

---

## 51. Use a Two-Level Output When Needed

If the output table becomes too compressed, use:

```markdown
| Number | Capability | Need Satisfied | Supporting Keywords |
```

Example:

```markdown
| Number | Capability | Need Satisfied | Supporting Keywords |
| --- | --- | --- | --- |
| 1 | Perform RPN calculations | Users can evaluate expressions using stack-based calculator behavior. | RPN calculator, add, sub, mul, div |
```

This improves clarity but increases output size.

For weaker models, it may reduce vague capability names.

---

## 52. Avoid Letting the Model Invent Actors Too Early

For user story work, weak models often overfit to the "As a user..." template and produce generic stories.

Before user stories, use:

```markdown
Do not write "As a user..." statements in this step.
```

When user stories are needed, define the actor simply:

```markdown
Use "calculator user" as the actor unless the target description clearly names another user type.
```

This prevents arbitrary personas.

---

## 53. Use Boundary Justification Only After Candidate Generation

Weak models can produce rationalizations instead of better candidates.

Sequence matters.

Better:

```markdown
First create the candidate set.
Then revise it.
Then provide a short boundary justification for each final item.
```

Do not ask for boundary justification before the model has a concrete set.

---

## 54. For Long Prompts, Add Local Recaps

Weak models lose context in long prompts. Add short recaps before important sections.

Example:

```markdown
Reminder: this phase only creates high-level capability anchors. It does not create user stories or implementation tasks.
```

Use this before the output format.

---

## 55. Use "Final Output Only" Blocks

At the end of a long prompt, restate exactly what the final answer should contain.

Example:

```markdown
Final output must contain only:

1. A table with columns Number, Capability, Supporting Keywords.
2. No explanatory paragraphs before or after the table.
```

This reduces drift.

---

## 56. Do Not Mix Prompt Development and Target Execution

If you are asking the model to improve a prompt, do not also include a target description unless you clearly separate them.

Weak models may execute the target task instead of editing the prompt.

Use explicit labels:

```markdown
## Prompt To Improve

...

## Your Task

Improve the prompt text lightly.
Do not execute the prompt.
Do not generate capability bullets for the example.
```

This is important in your use case.

---

## 57. Preserve Informal Language When It Works

For weaker models, informal direct language can outperform polished formal language.

Example of good informal instruction:

```markdown
What basic user needs would such a program satisfy?
```

That is clear.

A worse "professionalized" version might be:

```markdown
Derive an abstracted user-value capability model from the supplied product scope.
```

That sounds smarter but may perform worse.

When improving prompts for weaker models, do not automatically formalize. Improve only where the informal wording creates ambiguity.

---

## 58. Use "Light Editing" as a Constraint

When asking a model to improve a robust prompt, say:

```markdown
Make only light improvements.
Preserve the original structure, intent, and plain-language style.
Do not rewrite into a formal framework.
```

This prevents the model from destroying a working prompt.

You may also add:

```markdown
Prefer small wording changes over architectural changes.
```

This is especially useful when working with prompt snippets that already tested well.

---

## 59. Test on Failure Cases, Not Happy Paths

To optimize for weaker models, evaluate prompts against adversarial or ambiguous seeds.

For capability decomposition, test inputs like:

```markdown
Todo app with reminders and export.
```

```markdown
RPN calculator with undo and desktop packaging.
```

```markdown
I want a tool to compare CSV files and generate reports.
```

```markdown
A personal finance dashboard with bank import, budget categories, recurring expenses, and tax export.
```

Look for whether the model:

* produces too many bullets;
* produces one vague bullet;
* invents unsupported features;
* misses delivery/access requirements;
* confuses implementation with capability;
* fails to sort general-to-specific;
* duplicates keywords across rows;
* starts writing user stories prematurely.

---

## 60. Use Rubrics That Match the Prompt's Job

A good test rubric for weaker-model capability prompts:

```markdown
Score each output from 0 to 2:

1. User-facing capability wording
   0 = mostly implementation/tasks
   1 = mixed
   2 = clearly user-facing

2. Granularity
   0 = one giant item or many tiny items
   1 = somewhat uneven
   2 = compact, cohesive capability areas

3. Grounding
   0 = many invented items
   1 = some weak additions
   2 = grounded in the description

4. Ordering
   0 = arbitrary
   1 = partially sensible
   2 = most general/important first

5. Keyword traceability
   0 = invented or duplicated heavily
   1 = partially grounded
   2 = relevant keywords from original input
```

This helps you compare prompt variants.

---

## 61. Use Differential Testing Across Models

When optimizing for weaker models, test the same prompt across:

* SOTA model;
* runner-up model;
* cheap model;
* older model;
* small local model, if relevant.

A prompt is robust when the weaker model still produces acceptable structure.

Do not judge only by the best output from the best model.

---

## 62. Optimize for Median Output, Not Best Output

Prompt engineering for weaker models is about reducing variance.

A good prompt is not the one that occasionally produces a brilliant answer.

A good prompt is the one that usually produces a usable answer.

Prefer boring reliability over occasional elegance.

---

## 63. Introduce Constraints One at a Time

When improving a prompt, do not add ten new rules at once. You will not know what helped.

Use this loop:

1. Run baseline prompt.
2. Identify one recurring failure.
3. Add one targeted instruction.
4. Retest.
5. Keep only if it improves weak-model output.
6. Repeat.

Example:

Failure: model produces operator-level bullets.

Patch:

```markdown
Do not create separate bullets for minor operation variants when they serve the same user need.
```

Failure: model misses desktop packaging.

Patch:

```markdown
Include access, launch, delivery, or runtime requirements when they are explicitly mentioned.
```

---

## 64. Common Failure Modes and Fixes

### Failure: The model copies the input as one capability

Add:

```markdown
Do not use a single bullet that restates the whole target description.
Each capability must cover only one major user need.
```

### Failure: The model creates tiny task bullets

Add:

```markdown
Do not split by individual button, operator, field, screen, or implementation step unless it represents a different user need.
```

### Failure: The model invents common app features

Add:

```markdown
Do not add capabilities merely because similar apps often have them.
Use only explicit or strongly implied behavior from the description.
```

### Failure: The model misses non-functional delivery requirements

Add:

```markdown
If the target description mentions where or how users access the system, include that as a capability when it affects user experience.
```

### Failure: The model outputs explanations instead of the requested table

Add near the end:

```markdown
Output only the final table. Do not add explanation before or after it.
```

### Failure: The model does not revise

Add:

```markdown
You must create a preliminary set first, then revise it before final output. The final output should be the revised set.
```

### Failure: The model overuses generic labels

Add:

```markdown
Avoid vague one-word capabilities such as "Calculation," "Management," or "Support." Use short phrases that state the user need.
```

---

## 65. Recommended Prompt Pattern for Weaker Models

Here is a reusable pattern:

```markdown
Your task is to identify the main user-facing capabilities described by the target description.

A capability is a high-level thing an end user would care about, use, access, or rely on.

Do not create user stories.
Do not create implementation tasks.
Do not copy the whole input as one capability.
Do not split every small operation or button into a separate capability.

Process:

1. Read the target description.
2. Draft a preliminary set of capability bullets.
3. Review the bullets:
   - Is any bullet too broad?
   - Is any bullet too narrow?
   - Is any bullet implementation-only?
   - Do any bullets overlap?
   - Is any explicit access or delivery requirement missing?
4. Revise the bullets.
5. Sort the final bullets from most general/important to most specific.
6. For each final bullet, extract supporting keywords from the original description.

Supporting keywords must be exact words or short phrases from the original description.
Prefer assigning each keyword to only the most suitable capability.

Return only this table:

| Number | Capability | Supporting Keywords |
| --- | --- | --- |
| 1 | ... | ... |
```

This is compact, natural, and robust.

---

## 66. Slightly Stronger Version With Validation Output

Use this during development:

```markdown
Your task is to identify the main user-facing capabilities described by the target description.

A capability is a high-level thing an end user would care about, use, access, or rely on.

Avoid both extremes:
- too broad: one bullet that restates the whole target;
- too narrow: separate bullets for every small action, button, operation, or implementation step.

Process:

1. Create a preliminary capability set.
2. Review it critically. Assume the first version may be wrong.
3. Merge, split, remove, or rewrite bullets as needed.
4. Sort the final bullets from most general/important to most specific.
5. Extract supporting keywords from the original description.

Output:

## Capability Table

| Number | Capability | Supporting Keywords |
| --- | --- | --- |
| 1 | ... | ... |

## Validation

| Check | Result |
| --- | --- |
| Each capability is user-facing | Pass/Fail |
| No capability merely restates the whole input | Pass/Fail |
| No capability is only an implementation task | Pass/Fail |
| Related small operations are grouped | Pass/Fail |
| Explicit access/delivery requirements are covered | Pass/Fail |
| Capabilities are sorted general-to-specific | Pass/Fail |
```

This is better when you are debugging the prompt itself.

For production, remove the validation section if you want compact output.

---

## 67. Applying This to Your RPN Calculator Example

Target:

```markdown
I want to build an RPN calculator web app and have a packaged desktop version later on. I am thinking of supporting add, sub, neg, mul, div, sqr, sqrt, inv, pow, abs, exp, and ln functions. I also want to have undo capability.
```

A weak model may produce:

```markdown
1. Build RPN calculator
2. Add
3. Subtract
4. Negate
5. Multiply
6. Divide
7. Square root
8. Undo
9. Desktop
```

That is too fragmented.

A better capability set would be closer to:

```markdown
| Number | Capability | Supporting Keywords |
| --- | --- | --- |
| 1 | Perform RPN calculations | RPN calculator, add, sub, neg, mul, div |
| 2 | Use extended mathematical operations | sqr, sqrt, inv, pow, abs, exp, ln |
| 3 | Correct calculation mistakes | undo capability |
| 4 | Access the calculator as web and desktop app | web app, packaged desktop version |
```

Depending on the downstream use, you might split mathematical operations further:

```markdown
| Number | Capability | Supporting Keywords |
| --- | --- | --- |
| 1 | Perform RPN arithmetic calculations | RPN calculator, add, sub, neg, mul, div |
| 2 | Use power and root operations | sqr, sqrt, inv, pow |
| 3 | Use scientific exponential and logarithmic operations | exp, ln |
| 4 | Correct calculation mistakes | undo capability |
| 5 | Access the calculator as web and desktop app | web app, packaged desktop version |
```

The first version is better for very high-level capability anchoring.
The second version is better if the next step is user story boundary checking.

---

## 68. Prompt Design Rules of Thumb

Use this checklist when designing prompts for weaker models:

| Goal                    | Prompt Technique                       |
| ----------------------- | -------------------------------------- |
| Reduce ambiguity        | Define terms operationally             |
| Reduce task drift       | State active task early                |
| Reduce overproduction   | Give output-only format                |
| Reduce hallucination    | Require supporting keywords from input |
| Reduce bad granularity  | Explain too broad vs too narrow        |
| Improve grouping        | Provide grouping/splitting tests       |
| Improve ordering        | Define "most general"                  |
| Improve reliability     | Use preliminary draft + revision       |
| Improve schema fidelity | Put template at the end                |
| Improve phase control   | Use stop gates                         |
| Improve auditability    | Add validation table during testing    |

---

## 69. What Not to Do

Avoid these when targeting weaker models:

### Do not over-formalize

```markdown
Construct a semantically normalized capability ontology.
```

Use:

```markdown
List the main user-facing capabilities.
```

### Do not rely on implicit methodology

```markdown
Create good user stories.
```

Use:

```markdown
Each user story must describe one user action, the system response, and the observable user value.
```

### Do not combine too many phases

```markdown
Generate capabilities, stories, SSS, features, roadmap, and validation.
```

Use one phase at a time.

### Do not use examples that invite copying

If the target is a calculator, avoid calculator examples unless you want the model to mimic them.

### Do not bury critical constraints

Put "do not create user stories yet" near both the task statement and the output format.

---

## 70. Final Practical Heuristic

For SOTA models, you can often write prompts as **principles**.

For weaker models, write prompts as **procedures**.

For the weakest useful models, write prompts as **procedures plus examples plus output schema plus validation checklist**.

The weaker the model, the more your prompt should answer:

1. What exactly is the task?
2. What is not the task?
3. What order should the model follow?
4. What decisions must it make?
5. How should it repair bad candidates?
6. What should the final output look like?
7. What common mistakes must it avoid?

That is the core optimization strategy.
