# claude-code-ppc

A starting point for building agentic PPC systems with an AI coding agent.

**Based on:** [Agentic Coding for PPC: Claude Code Setup](https://peterjonker.dev/posts/agentic-coding-ppc-claude-code/)

---

## New to Claude Code?

Claude Code is a terminal-based AI coding agent. It reads your files, writes code, runs queries, and iterates — it doesn't just suggest snippets in a chat window. It's the difference between getting advice and having someone actually build the thing with you.

For PPC, that means:

- Ask it to analyze your search term data and it writes the SQL, runs it against BigQuery, reads the output, and gives you the analysis
- Tell it to build a classification pipeline and it writes the Python, tests it, fixes the errors, and validates the output
- Ask for a weekly performance report and it queries your data, writes the narrative, and formats the output

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

## Already using Claude Code?

Use this repo as inspiration. The patterns that matter:

- **`CLAUDE.md`** — operating instructions your agent reads on every session. Keep it under 80 lines. Every line costs context.
- **`docs/index.md`** — a navigation layer so the agent finds the right doc without loading everything at once
- **`projects/*/GOAL.md`** — one file per project describing what it is, current state, what's next. Prevents re-briefing every session.
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

## Part of a larger system

This repo is the foundation. More templates coming:

- Search term classification pipeline
- Budget optimization and forecasting
- Automated reporting
- Ad copy automation

Follow [peterjonker.dev](https://peterjonker.dev) for updates.
