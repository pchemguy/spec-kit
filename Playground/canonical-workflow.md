## Canonical Full Core Spec Kit Workflow

|  Step | Agent / Command | Input Template             | Produced/Amended Artifacts                                                                                                                              |
| ----: | :-------------- | -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1** | `constitution`  | `constitution-template.md` | `constitution.md`                                                                                                                                       |
| **2** | `specify`       | `spec-template.md`         | `nnn-feature-branch-base`<br>`specs/nnn-feature-dir-base/`<br>`FEATURE_DIR/spec.md`                                                                     |
| **3** | `clarify`       |                            | `FEATURE_DIR/spec.md`                                                                                                                                   |
| **4** | `plan`          | `plan-template.md`         | `FEATURE_DIR/plan.md`<br>`FEATURE_DIR/research.md`<br>`FEATURE_DIR/data-model.md`<br>`FEATURE_DIR/quickstart.md`<br>`FEATURE_DIR/contracts/[domain].md` |
| **5** | `checklist`     | `checklist-template.md`    | `FEATURE_DIR/checklists/[domain].md`<br>`FEATURE_DIR/spec.md`                                                                                           |
| **6** | `tasks`         | `tasks-template.md`        | `FEATURE_DIR/tasks.md`                                                                                                                                  |
| **7** | `analyze`       |                            |                                                                                                                                                         |
| **8** | `taskstoissues` |                            |                                                                                                                                                         |
| **9** | `implement`     |                            |                                                                                                                                                         |
