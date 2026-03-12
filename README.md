# claude-code-ppc

Your personal assistant for PPC. A scratchpad where ideas come to life, get validated, and evolve into production systems.

**Based on:** [Agentic Coding for PPC: Claude Code Setup](https://peterjonker.dev/posts/agentic-coding-ppc-claude-code/)

---

## The strategy

Everything starts in one place. You prototype pipelines, test analyses, iterate on prompts, and explore data, all in a single repository with your AI coding agent.

This is one half of a two-tier approach:

1. **This repo (personal assistant):** Discovery, prototyping, quick iteration. Move fast, but write code you'd be comfortable graduating. Try ideas, validate them, iterate.
2. **Dedicated repos (production):** When something needs scheduling, team access, CI/CD, or to run without your supervision, it graduates to its own repository with its own CLAUDE.md, tests, and deployment pipeline.

The beauty is that Claude can read both repositories simultaneously. When you're prototyping here and need context from a production system you built earlier, the agent sees both.

---

## Tools you'll use

Your agent executes real tools, not just generates code. Here's what's available and why each matters.

| Tool | What it does |
|------|-------------|
| **`bq`** | Query BigQuery directly. Zero context overhead vs MCP. Claude writes SQL, runs it, reads results, iterates. |
| **`gcloud`** | Authentication, project switching, service account management. |
| **`git`** | Version control, diffs, history. Skills like weekly-update read `git log` to generate progress reports. |
| **`python` / `uv`** | Script execution, reproducible environments. `uv` handles packages with lock files. |
| **`gh`** | GitHub CLI for PRs, issues, code review. Faster than the browser. |
| **`ruff`** | Linting and formatting. Catches issues before they compound. |
| **`mypy`** | Static type checking. Catches AI hallucinations in code before runtime. |
| **`pytest`** | Unit tests, integration tests, BigQuery dry runs. |

For details on each tool, plan mode, model routing, and the execute-validate-iterate loop, see `docs/tools.md`.

---

## Getting started

```bash
git clone https://github.com/peterjonkerdev/claude-code-ppc.git
cd claude-code-ppc

cp .env.example .env
# Add your API keys to .env

claude
# Claude reads CLAUDE.md automatically.
# It will detect setup hasn't run and walk you through it.
```

**Using a different agent?** (Cursor, Windsurf, Copilot, etc.)
Point it at `CLAUDE.md` and say: *"Read this file and follow the initialization instructions before doing anything else."*

**Install Claude Code:** [docs.anthropic.com/en/docs/claude-code](https://docs.anthropic.com/en/docs/claude-code)

---

## What's inside

```
CLAUDE.md                    # Agent operating instructions, loaded every session
README.md                    # This file

setup/
  context.md                 # Your setup, filled in during the first session
  SETUP.md                   # Setup interview, runs once on first session

docs/
  index.md                   # Navigation layer: what docs exist and where
  tools.md                   # Tools, workflows, and how to use them
  technical-patterns.md      # Patterns that emerge from your work

bigquery/                    # Example: how to document data sources for the agent
  README.md                  # Decision tree: which table for which question
  schemas/
    google_ads_transfer.md   # Full reference for Google Ads Data Transfer tables
    _template.md             # Template for documenting any table
  metrics/
    calculated-metrics.md    # CAC, LTV:CAC, ROAS and other metric formulas
    channel-breakdown.md     # Channel and subchannel taxonomy template

projects/
  _template/
    GOAL.md                  # Template for documenting a new project

roadmap/
  overview.md                # Current focus and active priorities
  priorities.md              # Full backlog: active + upcoming per project

tasks/
  todo.md                    # Active work items, tracked as checkable items
  lessons.md                 # Rules from past corrections, grows over time

.claude/
  skills/
    repo-health/             # Audits the repo for missing docs and dead links
    visualize-flow/          # Generates Mermaid diagrams of data flows
    weekly-update/           # Weekly progress summary from git history
    learning-opportunity/    # Teaching mode: explains concepts using your code

.env.example                 # Environment variable template
```

The `bigquery/` folder is the most opinionated part. It's also a good example of how to document any data source so the agent can use it effectively. If you're on a different stack (Snowflake, Redshift, spreadsheets), use `bigquery/schemas/_template.md` as a starting point for your own data docs.

---

## Skills

Reusable routines your agent follows on demand. Trigger them conversationally.

| Skill | How to trigger | What it does |
|-------|---------------|-------------|
| `repo-health` | "run repo-health" | Checks every project has a GOAL.md, all docs are indexed, no dead links |
| `visualize-flow` | "visualize the X flow" | Mermaid architecture diagram for any pipeline |
| `weekly-update` | "give me a weekly update" | Progress summary from git history |
| `learning-opportunity` | "explain this to me" | Explains any concept at three depth levels using your actual code |

Skills are Markdown files, not code. Read them, adapt them, write new ones for your own workflows.

---

## When something graduates

Signs a project is ready for its own repo:

- It needs to run on a schedule without your supervision
- Other people need access or will contribute
- It needs CI/CD, deployment pipelines, or infrastructure
- It has its own dependencies that shouldn't affect other projects
- You're spending more time maintaining it than prototyping new things

The graduation path: extract the project folder, give it its own `CLAUDE.md` with system-specific instructions, add tests, CI/CD, dependency management (`uv.lock`), and pre-commit hooks. The same principles apply at every scale. Only the scope changes.

Follow [peterjonker.dev](https://peterjonker.dev) for updates.
