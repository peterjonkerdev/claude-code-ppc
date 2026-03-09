# CLAUDE.md

> Read automatically by Claude Code at the start of every session.

---

## First session

Read `setup/context.md`. If it's empty, read `setup/SETUP.md` and run the setup interview before doing anything else. Once setup is complete, come back here and proceed normally.

---

## How to Assist

### Step 1: Understand intent

Read the request. Is it planning-heavy or execution-heavy?
- **Planning** (architecture, system design, debugging) → suggest `/model opus`
- **Execution** (SQL, scripting, running queries) → Sonnet or Haiku is fine

### Step 2: Find relevant docs

Read `docs/index.md` first. Only load what's needed for the current task.
- Always read a project's `GOAL.md` before working on it
- For data questions, start with the relevant schema doc
- Re-check `docs/index.md` when the topic shifts

### Step 3: Plan, then execute

Present the full plan before writing code or running queries. Include a validation step: "how will we know this worked?" Wait for confirmation, then execute autonomously. Only pause if something unexpected changes the plan.

### Step 4: Keep docs current

After any meaningful work, propose doc updates: what changed, what's now outdated, what needs to be added.

---

## Rules

**Self-improvement:** After any correction, propose a concrete edit to CLAUDE.md or the relevant doc that prevents the same mistake recurring.

**Verify before done:** Never mark a step complete without showing the result — sample output, row counts, or a test command.

**Code:** Every pipeline needs batching, error handling, logging. Keep it simple; don't abstract prematurely.

**Prompts:** All LLM prompts live as `.md` files in the project's `prompts/` folder. Never hardcode them in scripts.

**BigQuery (if applicable):**
- `p_ads_*` tables + `segments_date` for date-range queries
- `ads_*` views + `_DATA_DATE = _LATEST_DATE` for current state
- `SAFE_DIVIDE()` for all ratios
