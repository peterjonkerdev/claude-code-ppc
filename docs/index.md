# Documentation Index

> Navigation layer for the AI agent. Read this first to find what exists and where.
> Replace template entries with your actual projects and documentation once initialized.

---

## Projects

| Project | Goal File | Overview |
|---------|-----------|---------|
| _(add your first project here)_ | `projects/your-project/GOAL.md` | _(description)_ |

---

## BigQuery & Data

| Document | Location | Use When |
|----------|----------|---------|
| BigQuery Decision Tree | `bigquery/README.md` | Figuring out which table to query |
| Google Ads Transfer | `bigquery/schemas/google_ads_transfer.md` | Raw Google Ads entity + stats data |
| Calculated Metrics | `bigquery/metrics/calculated-metrics.md` | Metric formulas (CAC, LTV:CAC, ROAS, etc.) |
| Channel Taxonomy | `bigquery/metrics/channel-breakdown.md` | Channel / subchannel / stage definitions |

---

## Technical Reference

| Document | Location | Use When |
|----------|----------|---------|
| Technical Patterns | `docs/technical-patterns.md` | SQL conventions, pipeline patterns, prompt structure |

---

## Skills

| Skill | Location | Use When |
|-------|----------|---------|
| repo-health | `.claude/skills/repo-health/SKILL.md` | Auditing repo structure and catching missing docs |
| visualize-flow | `.claude/skills/visualize-flow/SKILL.md` | Generating pipeline architecture diagrams |
| weekly-update | `.claude/skills/weekly-update/SKILL.md` | Weekly progress summary from git history |
| learning-opportunity | `.claude/skills/learning-opportunity/SKILL.md` | Understanding a concept or piece of code in depth |

---

## By Question Type

| "I want to..." | Start Here | Then Read |
|----------------|------------|-----------|
| Query Google Ads performance data | `bigquery/README.md` | `bigquery/schemas/google_ads_transfer.md` |
| Build a new project | `projects/_template/GOAL.md` | `docs/index.md` (add it here after) |
| Check if the repo is healthy | `.claude/skills/repo-health/SKILL.md` | — |
| Understand a pattern or concept | `.claude/skills/learning-opportunity/SKILL.md` | — |

---

_This index is updated by the agent after each session where new documentation is added._
