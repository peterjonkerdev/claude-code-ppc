# claude-code-ppc

A plug-and-play template for building agentic PPC systems with Claude Code or any AI coding agent.

Clone it, point your agent at it, and start talking. The agent walks you through setup and starts building immediately.

**Based on:** [Agentic Coding for PPC: Claude Code Setup](https://peterjonker.dev/posts/agentic-coding-ppc-claude-code/)

---

## How it works

This repository contains the documentation layer your AI agent needs to understand your advertising setup. It is not code — it is structured context. Your agent reads it, asks you questions, learns your stack, and then builds the code.

On first open, your agent will:
1. Read `CLAUDE.md` (automatic in Claude Code)
2. Detect the repo is not yet initialized
3. Interview you about your environment, BigQuery, Google Ads account, and business context
4. Write your answers to `setup/context.md`
5. Clean up any template content that doesn't apply to you
6. Ask what you want to build first, and start building

Every session after that, the agent reads your context and picks up where you left off.

---

## Prerequisites

Install these before cloning:

| Tool | Purpose | Install |
|------|---------|---------|
| Claude Code | AI coding agent | [docs.anthropic.com/en/docs/claude-code](https://docs.anthropic.com/en/docs/claude-code) |
| Python 3.9+ | Running pipelines | [python.org](https://python.org) |
| Google Cloud SDK | BigQuery + auth | [cloud.google.com/sdk/docs/install](https://cloud.google.com/sdk/docs/install) |
| bq CLI | Query BigQuery from terminal | Included with Cloud SDK |

You will also need:
- A GCP project with BigQuery enabled
- Google Ads Data Transfer set up (recommended — gives you full account data in BigQuery)
- API keys for whatever LLM you plan to use (Gemini, Claude, OpenAI, etc.)

Optional but useful:
- Google Ads API developer token (for writing changes back to your accounts)
- A web search API key like Tavily (for search term enrichment workflows)

---

## Quick start

```bash
# Clone the repo
git clone https://github.com/peterjonkerdev/claude-code-ppc.git
cd claude-code-ppc

# Copy the environment variable template
cp .env.example .env
# Edit .env with your API keys (never commit this file)

# Open in Claude Code
claude
# The agent reads CLAUDE.md automatically and starts the setup interview
```

If you use a different agent (Cursor, Windsurf, Copilot, etc.), point it at `CLAUDE.md` and tell it: "Read this file and follow the initialization instructions."

---

## What you can build

| Use case | What it does |
|----------|-------------|
| Search term classification | AI classifies thousands of search terms, routes to the right campaign and ad group, surfaces negatives for review |
| Budget optimization | Predictive budget allocation across campaigns based on historical ROI and performance trends |
| Automated reporting | Weekly and monthly performance reports generated directly from BigQuery |
| Ad copy automation | AI-generated headline and description variants, rotated based on performance signals |
| LTV or margin-aware bidding | Optimize toward predicted customer value or product margin rather than last-click ROAS |
| Data chat | Natural language interface to your BigQuery data — ask questions, get answers |
| Forecasting | Demand and conversion forecasts down to SKU or campaign level |

---

## Repository structure

```
CLAUDE.md                    # Agent operating instructions — read automatically
README.md                    # This file

setup/
  context.md                 # Your setup, filled in during the first session

docs/
  index.md                   # Navigation layer — tells the agent what docs exist and where

bigquery/
  README.md                  # Decision tree: which table to use for which question
  schemas/
    google_ads_transfer.md   # Full reference for Google Ads Data Transfer tables
    _template.md             # Template for documenting any new table
  metrics/
    calculated-metrics.md    # Canonical metric formulas (CAC, LTV:CAC, ROAS, etc.)
    channel-breakdown.md     # Channel and subchannel taxonomy

projects/
  _template/
    GOAL.md                  # Template for documenting a new project

.claude/
  skills/
    repo-health/             # Audits the repo for missing docs and dead links
    visualize-flow/          # Generates Mermaid diagrams of pipeline architectures
    weekly-update/           # Weekly progress summary from git history
    learning-opportunity/    # Teaching mode — explains concepts using your actual code

.env.example                 # Environment variable template
```

---

## Skills

Skills are reusable routines the agent follows when you invoke them. They're implemented as Markdown files in `.claude/skills/` — not code.

| Skill | How to trigger | What it does |
|-------|---------------|-------------|
| `repo-health` | "run repo-health" | Audits the repo: checks every project has a GOAL.md, all docs are indexed, no dead links |
| `visualize-flow` | "visualize the [project] flow" | Generates a Mermaid architecture diagram for any pipeline or data flow |
| `weekly-update` | "give me a weekly update" | Compiles progress from git history, proposes task status changes |
| `learning-opportunity` | "explain this to me" | Teaching mode — explains any concept at three depth levels using your actual code |

Skills are templates. Adapt them to your stack (add a Slack webhook to weekly-update, connect weekly-update to your task manager, etc.).

---

## The architecture

```
BigQuery (data warehouse)
    ↓
Python pipelines (orchestration + logic)
    ↓
LLM APIs — Gemini, Claude, OpenAI (reasoning + classification)
    ↓
Google Ads API (execution — push changes back to accounts)
```

Your agent manages the code. You provide the domain expertise and strategic direction.

---

## Part of a larger system

This is the first repository in a growing collection of agentic PPC infrastructure templates. Each repo covers a specific layer of the stack and is designed to work together.

More templates coming:
- Search term classification pipeline
- Budget optimization and forecasting
- Automated reporting
- Ad copy automation

Follow [peterjonker.dev](https://peterjonker.dev) for updates.
