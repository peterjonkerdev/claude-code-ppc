# claude-code-ppc

A personal assistant repository for building agentic PPC systems with an AI coding agent.

**Based on:** [Agentic Coding for PPC: Claude Code Setup](https://peterjonker.dev/posts/agentic-coding-ppc-claude-code/)

---

## What is this?

This is a **personal assistant repo**: a single repository where you manage multiple PPC projects side by side. You prototype pipelines, run analyses, test ideas, and build out your data layer, all in one place with your AI coding agent.

When a project matures and needs scheduling, team access, or deployment, you extract it into its own dedicated repository with its own agent instructions, tests, and CI/CD. This repo is the lab. Production repos are the output.

**The architecture this is built around:**
```
Data warehouse (BigQuery or similar)
    ↓
Python pipelines — orchestration and logic
    ↓
LLM APIs — Gemini, Claude, OpenAI — for reasoning and classification
    ↓
Ad platform APIs — push changes back to Google Ads, Meta, etc.
```

Your agent manages the code. You provide the domain expertise and strategic direction.

**Install Claude Code:** [docs.anthropic.com/en/docs/claude-code](https://docs.anthropic.com/en/docs/claude-code)

---

## The patterns that matter

- **`CLAUDE.md`** — operating instructions your agent reads on every session. Keep it under 80 lines. Every line costs context.
- **`docs/index.md`** — a navigation layer so the agent finds the right doc without loading everything at once.
- **`projects/*/GOAL.md`** — one file per project describing what it is, current state, what's next. Prevents re-briefing every session.
- **`roadmap/`** — priorities and backlog across all projects. Your agent reads this to understand what's active and what's next.
- **Skills** — reusable routines in `.claude/skills/`. If you explain the same thing to your agent twice, write a skill for it.

Clone it, strip out what doesn't apply, add what does.

---

## Quick start

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

---

## What's in here

```
CLAUDE.md                    # Agent operating instructions — loaded every session
README.md                    # This file

setup/
  context.md                 # Your setup — filled in during the first session
  SETUP.md                   # Setup interview — runs once on first session

docs/
  index.md                   # Navigation layer — what docs exist and where

bigquery/                    # Optional — only relevant if you use BigQuery
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

.claude/
  skills/
    repo-health/             # Audits the repo for missing docs and dead links
    visualize-flow/          # Generates Mermaid diagrams of data flows
    weekly-update/           # Weekly progress summary from git history
    learning-opportunity/    # Teaching mode — explains concepts using your code

.env.example                 # Environment variable template
```

The BigQuery docs are the most opinionated part of this repo. If you're on a different stack — Snowflake, Redshift, Looker, spreadsheets — ignore that folder and build your own data layer docs using `bigquery/schemas/_template.md` as a starting point.

---

## Skills

Reusable routines your agent follows on demand. Trigger them conversationally.

| Skill | How to trigger | What it does |
|-------|---------------|-------------|
| `repo-health` | "run repo-health" | Checks every project has a GOAL.md, all docs are indexed, no dead links |
| `visualize-flow` | "visualize the X flow" | Mermaid architecture diagram for any pipeline |
| `weekly-update` | "give me a weekly update" | Progress summary from git history |
| `learning-opportunity` | "explain this to me" | Explains any concept at three depth levels using your actual code |

Skills are Markdown files — not code. Read them, adapt them, write new ones for your own workflows.

---

## From personal assistant to production

This repo is your starting point. As projects mature, extract them into dedicated repos with:

- Their own `CLAUDE.md` with system-specific instructions
- Test suites and CI/CD pipelines
- Dependency management (`uv.lock` for reproducibility)
- Eval frameworks for AI pipeline accuracy
- Pre-commit hooks (linting, type checking, tests)

The same principles apply at every scale: persistent context, structured documentation, plan-first execution, and automated code quality checks. Only the scope changes.

Follow [peterjonker.dev](https://peterjonker.dev) for updates.
