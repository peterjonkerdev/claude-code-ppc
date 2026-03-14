# [Project Name]

> Copy this file to `projects/your-project-name/GOAL.md` and fill it in.
> The agent reads this file before doing any work on this project.

---

## What this project is

_(One paragraph: the problem this solves, the value it delivers, and who benefits.)_

---

## Current status

| Item | Status |
|------|--------|
| Overall | 🔵 In planning / 🟡 In progress / 🟢 Live / ⏸ Paused |
| Last updated | YYYY-MM-DD |

---

## Architecture

_(How data flows through this project. What goes in, what comes out, what systems are involved.)_

```
Input (BigQuery table / API) → Processing (Python / LLM) → Output (BigQuery / Google Ads API / Report)
```

---

## Completed

- _(List what's done)_

---

## In progress

- _(What's actively being built)_

---

## Next up

- _(What's planned but not started)_

---

## Validation

How will we know this works?

- _(e.g. Row count check on output table)_
- _(e.g. Accuracy against a test set)_
- _(e.g. Compare output to manual baseline)_

---

## Key files

| File | What it is |
|------|-----------|
| `projects/[name]/scripts/` | Python pipelines |
| `projects/[name]/prompts/` | LLM prompt files |
| `projects/[name]/docs/` | Additional documentation |

---

## Notes

_(Anything else the agent should know before working on this project: constraints, decisions made, things to avoid, context that isn't obvious from the code.)_

---

## Graduation Criteria

When this project is ready for its own production repo, check these boxes:

- [ ] Needs to run on a schedule
- [ ] Other people need access or will contribute
- [ ] Needs CI/CD or deployment pipelines
- [ ] Has its own dependencies that shouldn't affect other projects

If two or more are checked, follow `docs/graduation-guide.md` and use `production-template/` as the scaffold.
