# Documentation Index

Use this file to find the right documentation for any task. Read only what you need.

## Projects

| Project | Goal File | Status |
|---------|-----------|--------|
| _(add your first project here)_ | `projects/your-project/GOAL.md` | _(status)_ |

Always read the project `GOAL.md` before any work.
For project priorities: `projects/<name>/tasks/todo.md`.
For the portfolio view: `README.md`.

## Tools & Workflows

| Document | Location | Use When |
|----------|----------|---------|
| Tools & Workflows | `docs/tools.md` | Understanding available CLI tools, plan mode, model routing, the execute-validate-iterate loop |
| Graduation Guide | `docs/graduation-guide.md` | Moving a project from prototype to production |

## BigQuery & Data

| Document | Location | Use When |
|----------|----------|---------|
| BigQuery Decision Tree | `bigquery/README.md` | First stop: which table, which date filter to use |
| Google Ads Transfer | `bigquery/schemas/google_ads_transfer.md` | Entity + stats tables, partition patterns, fields |
| Calculated Metrics | `bigquery/metrics/calculated-metrics.md` | CAC, LTV:CAC, ROAS: exact formulas |
| Channel Taxonomy | `bigquery/metrics/channel-breakdown.md` | Channel / subchannel / stage definitions |

## Technical Reference

| Document | Location | Use When |
|----------|----------|---------|
| Technical Patterns | `docs/technical-patterns.md` | SQL conventions, pipeline patterns, prompt structure, testing, Google Ads API |
| Lessons | `lessons.md` | Past corrections and rules to prevent recurrence |

## Production

| Document | Location | Use When |
|----------|----------|---------|
| Production Template | `production-template/README.md` | Scaffolding a new production repo |
| Production CLAUDE.md | `production-template/CLAUDE.md` | Agent protocol template for production systems |

## Skills

| Skill | Location | Use When |
|-------|----------|---------|
| Repo Health | `.claude/skills/repo-health/SKILL.md` | Auditing repo structure, finding orphaned docs |
| Visualize Flow | `.claude/skills/visualize-flow/SKILL.md` | Diagrams or data flow charts for any project |
| Weekly Update | `.claude/skills/weekly-update/SKILL.md` | Generating weekly status updates from git history |
| Learning Opportunity | `.claude/skills/learning-opportunity/SKILL.md` | Understanding a concept or piece of code in depth |
| Eval Report | `.claude/skills/eval-report/SKILL.md` | Running evals, checking accuracy, comparing prompt versions |
| Schema Validator | `.claude/skills/schema-validator/SKILL.md` | Validating BigQuery schemas against actual tables |

## Learning

| Document | Location | Use When |
|----------|----------|---------|
| Learning directory | `learning/` | Concept explanations saved by the learning-opportunity skill |

---

_This index is updated by the agent after each session where new documentation is added._
