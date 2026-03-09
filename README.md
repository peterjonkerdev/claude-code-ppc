# Claude Code for PPC

Starter kit for building agentic PPC systems with [Claude Code](https://docs.anthropic.com/en/docs/claude-code). Contains the CLAUDE.md operating protocol and reusable skills referenced in [this guide](https://peterjonker.dev/posts/agentic-coding-ppc-claude-code/).

## What's here

```
CLAUDE.md                              # Agent operating protocol
.claude/skills/
  learning-opportunity/SKILL.md        # Three-level teaching mode
  repo-health/SKILL.md                # Repository structure audit
  visualize-flow/SKILL.md             # Mermaid diagram generator
  weekly-update/SKILL.md              # Git-based progress summaries
```

## Quick start

1. Clone this repo
2. Open the directory in Claude Code
3. Start building — Claude reads `CLAUDE.md` automatically and follows the operating protocol

The `CLAUDE.md` defines how Claude operates in your repo: how it finds documentation, plans before executing, validates results, and keeps its own context updated. Customize it for your stack.

## Skills

Skills are reusable routines Claude can invoke. Trigger them with `/skill-name` in Claude Code.

| Skill | Trigger | What it does |
|-------|---------|-------------|
| **learning-opportunity** | `/learning-opportunity` | Explains any concept at three depth levels (core, mechanics, deep dive) |
| **repo-health** | `/repo-health` | Audits repo structure: missing docs, dead links, orphaned files, schema coverage |
| **visualize-flow** | `/visualize-flow` | Generates Mermaid diagrams of data flows and pipeline architectures |
| **weekly-update** | `/weekly-update` | Summarizes the week's git activity and proposes task status updates |

## The architecture this supports

```
BigQuery (data warehouse) → Python (orchestration) → LLM APIs (reasoning) → Google Ads API (execution)
```

This repo provides the agent scaffolding. You bring your data, business logic, and domain expertise. Claude builds the pipelines.

## Full guide

Read the complete setup guide: [Agentic Coding for PPC: Claude Code Setup](https://peterjonker.dev/posts/agentic-coding-ppc-claude-code/)

## Coming soon

- Full project templates (search term classification, budget optimization, reporting)
- BigQuery schema documentation patterns
- Evaluation pipeline templates
- Example GOAL.md files for common PPC projects
