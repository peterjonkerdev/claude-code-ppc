# CLAUDE.md

> Read automatically by Claude Code at the start of every session.

---

## First session

Read `setup/context.md`. If it's empty, read `setup/SETUP.md` and run the setup interview before doing anything else. Once setup is complete, come back here and proceed normally.

---

## Before Starting Work

1. Read the project's `GOAL.md` — what it does, current state, key references
2. Read the project's `tasks/todo.md` — what's active, what's blocked
3. For data questions, start with the relevant schema doc in `bigquery/`
4. For cross-project navigation: `docs/index.md`

## After Completing Work

1. Update the project's `tasks/todo.md` — mark done, add new items
2. Update `GOAL.md` current state — if status fundamentally changed
3. Update `README.md` project table — only if status changed

Skip if nothing structural changed.

## How to Work

- **Exploration** (data questions, quick analysis, testing ideas) → just do it, iterate fast
- **Structured build** (multi-file pipeline, complex system) → suggest plan mode and `/model opus`
- **Execution** (SQL, scripting, running queries) → Sonnet or Haiku is fine

For builds: present a quick plan with validation criteria, get confirmation, then execute autonomously.

---

## Rules

**Self-improvement:** After any correction, propose a concrete edit to CLAUDE.md or the relevant doc that prevents the same mistake recurring.

**Verify before done:** Never mark a step complete without showing the result: sample output, row counts, or a test command.

**Thoughtful speed:** Move fast on exploration. For builds that might graduate to production, write them well the first time: clear names, error handling, type hints.

**No bloat:** Don't create files or folders outside of `projects/<name>/` without discussing it first. If something doesn't fit an existing project, ask whether it needs a new one.

**Prompts:** All LLM prompts live as `.md` files in the project's `prompts/` folder. Never hardcode them in scripts.

**BigQuery (if applicable):**
- `p_ads_*` tables + `segments_date` for date-range queries
- `ads_*` views + `_DATA_DATE = _LATEST_DATE` for current state
- `SAFE_DIVIDE()` for all ratios

**Pipelines:** Batching, error handling, logging. Keep it simple; don't abstract prematurely. For anything heading to production: write end-to-end tests with known input/output, then iterate until they pass.

**Specs:** For features heading to production, write a spec in `specs/` using the template. Define expected output, acceptance criteria, and verification method before building.

---

## Working Style

**Subagents:** Use for parallel, independent tasks. One task per subagent. Main thread for orchestration.

**Bug fixing:** Fix it, explain what broke, keep moving. Only pause if the fix changes scope.

**Elegance:** For non-trivial changes, pause: "Is there a more elegant way?" Don't over-engineer, but don't settle for the first thing that works.

**Task tracking:** Active work in each project's `tasks/todo.md`. Multi-step work as checkable items. After corrections, capture the lesson in `lessons.md`.
