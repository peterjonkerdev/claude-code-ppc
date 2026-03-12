# CLAUDE.md

> Read automatically by Claude Code at the start of every session.

---

## First session

Read `setup/context.md`. If it's empty, read `setup/SETUP.md` and run the setup interview before doing anything else. Once setup is complete, come back here and proceed normally.

---

## How to Assist

This is a discovery repo. Bias toward trying things quickly over planning extensively. Save thorough planning for production repos.

### Step 1: Understand intent

Read the request. Is it exploration or a structured build?
- **Exploration** (data questions, quick analysis, testing ideas) → just do it, iterate fast
- **Structured build** (multi-file pipeline, complex system) → suggest plan mode and `/model opus`
- **Execution** (SQL, scripting, running queries) → Sonnet or Haiku is fine

### Step 2: Find relevant docs

Read `docs/index.md` first. Only load what's needed for the current task.
- Always read a project's `GOAL.md` before working on it
- For data questions, start with the relevant schema doc
- Re-check `docs/index.md` when the topic shifts

### Step 3: Execute and iterate

For exploration: run the query, show results, suggest next steps. Keep the loop tight.
For builds: present a quick plan, get confirmation, then execute autonomously. Include a validation step: "how will we know this worked?" Only pause if something unexpected changes direction.

### Step 4: Keep docs current

After meaningful work, propose doc updates: what changed, what's outdated, what's new.

---

## Rules

**Self-improvement:** After any correction, propose a concrete edit to CLAUDE.md or the relevant doc that prevents the same mistake recurring.

**Verify before done:** Never mark a step complete without showing the result: sample output, row counts, or a test command.

**Thoughtful speed:** Move fast on exploration. For builds that might graduate to production, write them well the first time: clear names, error handling, type hints. Refactoring a messy prototype costs more than getting it right upfront.

**Prompts:** All LLM prompts live as `.md` files in the project's `prompts/` folder. Never hardcode them in scripts.

**BigQuery (if applicable):**
- `p_ads_*` tables + `segments_date` for date-range queries
- `ads_*` views + `_DATA_DATE = _LATEST_DATE` for current state
- `SAFE_DIVIDE()` for all ratios

**Pipelines:** Batching, error handling, logging. Keep it simple; don't abstract prematurely. For anything heading to production: write end-to-end tests with known input/output, then iterate until they pass.

**Specs:** For features heading to production, write a spec in `specs/` using the template. Define expected output, acceptance criteria, and verification method before building.

---

## Working Style

### Subagent Strategy
Use subagents for parallel, independent tasks: reading multiple docs, exploring approaches, running queries while writing code. One task per subagent. Keep the main thread for orchestration and decisions.

### Autonomous Bug Fixing
Hit a bug? Fix it. Explain what broke and what you changed, then keep moving. Only pause if the fix changes the scope of the original task.

### Demand Elegance
For non-trivial changes, pause: "Is there a more elegant way?" Consider readability, reuse, and whether it'll make sense in a month. Don't over-engineer, but don't settle for the first thing that works.

### Task Tracking
Active work in `tasks/todo.md`. For multi-step work, plan as checkable items before executing.
After corrections, capture the lesson in `tasks/lessons.md` with date + context + rule.
