# CLAUDE.md — PPC Agent Operating Instructions

> Read automatically by Claude Code at the start of every session.
> Defines how to operate in this repository.

---

## Step 0: Initialization Check

**Before doing anything else, read `setup/context.md`.**

If the file is empty or `Status` is `not_initialized`, run the setup interview below. Do not proceed with any other work until setup is complete and written to `setup/context.md`.

If `Status: initialized` — skip to [How to Assist](#how-to-assist).

---

## Setup Interview

Run this as a conversation. Ask questions one section at a time. Confirm answers before continuing. When all sections are done, write everything to `setup/context.md`, set `Status: initialized`, and then populate `bigquery/README.md` and `docs/index.md` based on what you learned.

Finish the interview by asking: "What do you want to build first?" Then create the first project folder under `projects/` using the template at `projects/_template/GOAL.md`.

### Post-Setup Cleanup

After writing `setup/context.md` and creating the first project, clean up the repo so it reflects the user's actual setup — not the template defaults. This keeps the repo lean from the start.

**Remove or replace:**
- Any placeholder content in `docs/index.md` that doesn't match the user's real projects
- Any BigQuery schema docs in `bigquery/schemas/` that aren't relevant to the user's actual tables
- The `projects/_template/` directory — it was only needed to bootstrap
- Any channel taxonomy or metric definitions in `bigquery/metrics/` that don't match the user's business

**Update with real content:**
- `bigquery/README.md` — rewrite the decision tree with actual table names and project IDs
- `docs/index.md` — replace template entries with the user's real projects and documentation paths
- `bigquery/metrics/calculated-metrics.md` — keep only the metrics that actually apply

The goal: after the first session, the repo should look like it was built for this user from day one. No leftover template noise.

### Section 1: Environment

- What OS are you on?
- Is Python installed? Run: `python3 --version`
- Is the `bq` CLI installed? Run: `bq version`
- Is `gcloud` authenticated? Run: `gcloud auth list`
- Do you have a Google Ads API developer token? (needed to push changes back to accounts)
- What API keys do you have available? (Gemini, OpenAI, Anthropic, Tavily, etc.)

If anything is missing, help them install it before continuing. Links:
- Google Cloud SDK (includes bq): https://cloud.google.com/sdk/docs/install
- Claude Code: https://docs.anthropic.com/en/docs/claude-code

### Section 2: BigQuery Setup

- What is your GCP project ID?
- What dataset contains your Google Ads Data Transfer tables? (default: `Transfers`)
- Do you have any other BigQuery datasets? For each one:
  - Dataset name
  - What data it contains (performance aggregates, orders, LTV, product catalog, etc.)
  - How it connects to your Google Ads data (shared customer ID, GCLID, product ID, etc.)
- Run a discovery query to confirm access:
  ```bash
  bq query --nouse_legacy_sql 'SELECT table_name FROM `YOUR_PROJECT.YOUR_DATASET.INFORMATION_SCHEMA.TABLES` LIMIT 10'
  ```
  Replace with their actual project and dataset. Fix any auth or permission issues before continuing.

### Section 3: Google Ads Account

- Single account or MCC (manager account with multiple sub-accounts)?
- What are your customer IDs? (10-digit format: XXX-XXX-XXXX)
- What campaign types do you run? (Search, Shopping, Performance Max, Display, Video, Demand Gen)
- Do you separate branded and non-brand campaigns?
- How are campaigns structured? (by product line, region, funnel stage, audience, etc.)
- What bid strategies are active? (tROAS, tCPA, Maximize Conversions, manual CPC, etc.)

### Section 4: Business Context

- What industry or vertical?
- Describe the product or service offering briefly.
- What are the primary optimization KPIs? (ROAS, LTV:CAC, margin, revenue, new customers, CPA)
- Do you have backend data in BigQuery beyond Google Ads? (orders, revenue, LTV, margins, returns, inventory)
  - If yes: what tables, what fields, and how does it join to ad platform data?
- Who else is on the team that would use reports or dashboards?

### Section 5: First Project

What's the most valuable thing to build first? Common starting points:

| Project | What it does |
|---------|-------------|
| Search term classification | AI classifies new search terms, routes to campaigns/ad groups, surfaces negatives |
| Budget optimization | Allocates budget across campaigns based on historical performance and forecasts |
| Automated reporting | Weekly or monthly performance reports generated from BigQuery |
| Ad copy automation | AI generates headline/description variants, rotates based on performance |
| LTV or margin-aware bidding | Optimize toward real business value rather than last-click ROAS |
| Data chat / analytics agent | Natural language interface to your BigQuery data |

---

## How to Assist

### Step 1: Understand Intent

Read the request. Identify whether it is planning-heavy or execution-heavy:
- **Planning-heavy** (system design, architecture, complex debugging) → suggest `/model opus`
- **Execution-heavy** (SQL, scripting, queries, formatting) → Sonnet or Haiku

### Step 2: Find Relevant Documentation

Read `docs/index.md` to find what documentation exists. Only load what's needed.

- Always read a project's `GOAL.md` before doing any work on that project
- For BigQuery work, start with `bigquery/README.md`, then the specific schema doc
- For technical patterns, check `docs/technical-patterns.md` if it exists
- Re-check `docs/index.md` when the topic shifts mid-conversation

### Step 3: Plan, Then Execute

Present the full plan before writing code or running queries. Include:
- All steps in sequence
- Which tables, APIs, and scripts are involved
- A Validation section: "How will we know this worked?"

Wait for confirmation. Once confirmed, execute autonomously — write code, run queries, iterate, fix errors. Only pause if something unexpected changes the plan.

### Step 4: Update Documentation

After completing work, propose doc updates:
- Update anything now out of date
- Remove references to things that no longer exist
- Add docs for anything new (tables, scripts, pipelines, patterns)
- Commit with a clear message

---

## Critical Rules

### Self-Improvement Loop

After any correction or mistake, propose a concrete edit to CLAUDE.md or the relevant doc that would prevent the same mistake recurring. Every friction point should become a permanent fix.

### Verification Before Done

Never mark a step complete without demonstrating the result:
- SQL: show expected output columns or sample rows
- Code: show expected input/output or test command
- Data pipeline: show row counts or non-null checks

### BigQuery

- Use `p_ads_*` partitioned tables with `segments_date` for any date-range stats query
- Use `ads_*` views with `_DATA_DATE = _LATEST_DATE` for dimension lookups (current state only)
- Use `SAFE_DIVIDE()` for all ratio calculations — never the bare division operator
- Use `LOWER()` for string comparisons
- Use the `bq` CLI for all BigQuery work in Claude Code — not the BigQuery MCP

### Code Quality

- Every Python pipeline gets a Mermaid diagram in its README
- Every pipeline must include: batching, rate limiting, retry logic, error handling, logging
- Start simple, evolve intentionally — no premature abstraction
- Write code for humans: clear variable names, comments on non-obvious logic

### Prompts

All LLM prompts live as Markdown files in the project's `prompts/` directory. Never hardcode prompt strings inside Python scripts.
