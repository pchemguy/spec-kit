---
url: https://chatgpt.com/g/g-p-69ca8410ab7c819198782233666b1069-spec-kit/c/69e7ad37-1748-83eb-964f-6761750ec443
---
## Canonical Full Core Spec Kit Workflow

|  Step | Agent / Command | Required                    | Primary Inputs                                                 | Produced / Amended Artifacts                                                                                  |
| ----: | --------------- | --------------------------- | -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| **1** | `constitution`  | Yes                         | `constitution-template.md`                                     | `constitution.md`                                                                                             |
| **2** | `specify`       | Yes                         | `spec-template.md`                                             | `FEATURE_BRANCH`<br>`FEATURE_DIR/`<br>`spec.md`                                                               |
| **3** | `clarify`       | No<br>*spec clarity*        | `FEATURE_DIR/spec.md`                                          | `spec.md` (refined)                                                                                           |
| **4** | `plan`          | Yes                         | `plan-template.md`<br>`spec.md`                                | `plan.md`<br>`research.md`<br>`data-model.md`<br>`quickstart.md`<br>`contracts/*.md`                          |
| **5** | `checklist`     | No<br>*spec quality*        | `checklist-template.md`<br>`spec.md`<br>`plan.md` (optional)   | `checklists/*.md`<br>`spec.md` (clarifications)                                                               |
| **6** | `tasks`         | Yes                         | `tasks-template.md`<br>`plan.md` (+ design artifacts)          | `tasks.md`                                                                                                    |
| **7** | `analyze`<br>   | No<br>*package consistency* | `spec.md`<br>`plan.md`<br>`tasks.md` (+ all related artifacts) | Amendments to spec/plan/tasks                                                                                 |
| **8** | `taskstoissues` | No                          | `tasks.md`                                                     | GitHub Issues                                                                                                 |
| **9** | `implement`     | Yes                         | `tasks.md` + all prior artifacts                               | Source code<br>Tests<br>Docs updates (depends on user prompts)<br>Progress tracking (depends on user prompts) |

**Notes**:

- All feature artifacts are relative to `FEATURE_DIR`.
- Every command accepts a user prompt.
