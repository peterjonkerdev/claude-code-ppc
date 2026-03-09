# CLAUDE.md — Assistant Operating Protocol

## How to Assist

### Step 1: Understand Intent

Read the user's message carefully. Identify what they need.

After understanding intent, suggest whether this task is **planning-heavy** (use Opus) or **execution-heavy** (use Sonnet/Haiku) so the user can switch models if needed.

### Step 2: Find Relevant Documentation

Read `docs/index.md` to find relevant docs. **Only load what's needed.**

- Always read a project's `GOAL.md` before doing any work on it
- For BigQuery, start with `bigquery/README.md`, then load only the specific table doc
- For technical patterns (SQL, batch processing), check `docs/technical-patterns.md`
- Re-check `docs/index.md` when the topic shifts mid-conversation

### Step 3: Plan, Then Execute

**Planning:** Present the full plan with all steps. For substantive work, include a **Validation** section: "How will we know this worked?" Wait for confirmation before building anything.

**Execution:** Once the plan is confirmed, execute autonomously — write code, run queries, iterate, fix errors. Don't stop for confirmation between steps. Only pause if something unexpected changes the plan.

**Mid-task questions:** Answer the question, recap where we are, ask if we should continue or adjust.

### Step 4: Update Your Own Context

After completing work, review what changed and present a plan to update the repo's documentation. This ensures the next session starts with accurate, current context. Wait for confirmation before making changes.

The plan should cover:

- **Update** docs, READMEs, schemas, or index files that are now out of date because of what we built, changed, or removed
- **Remove** references to things that no longer exist — dead links, deprecated workflows, old table names, outdated instructions
- **Add** documentation for anything new that a future session would need to understand
- **Update priorities** when items are completed or new next steps are discussed
- `git add` and commit — always ask before pushing to remote

Skip if nothing structural changed (quick fixes, informational conversations).

---

## Critical Rules

### Priorities & Backlog

When asked to show priorities — read and display your priorities/backlog file in full. Don't add dates or priority rankings; items are intentionally unscheduled.

### Execution & Documentation

Execute directly: write code, run queries, test, validate, iterate. Only document queries/code in dedicated reference files after they're validated and needed for future use.

### Verification Before Done

Never mark a step complete without demonstrating the result:

- SQL: expected output columns or sample rows
- Code/scripts: expected input → output or test commands
- Data pipeline: row counts or non-null checks
- Config changes: before/after comparison

### Self-Improvement Loop

After any correction or mistake, propose a concrete edit to the relevant `CLAUDE.md` that prevents the same mistake recurring.

### Prompts

All prompt files must be Markdown (`.md`), stored in the project's `prompts/` directory, using proper formatting (headers, tables, lists, code blocks).
