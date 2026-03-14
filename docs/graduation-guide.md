# Graduation Guide

How to move a project from the prototyping repo to its own production repository.

---

## When to graduate

A project is ready for its own repo when any of these are true:

- It needs to run on a schedule (cron, Cloud Scheduler, Airflow)
- Other people need to access, review, or contribute to the code
- It needs CI/CD, deployment pipelines, or infrastructure-as-code
- It has dependencies that shouldn't affect other projects
- You're spending more time maintaining it than prototyping new things

If none of these apply, keep it in the prototyping repo. There's no rush.

---

## Process

### 1. Create the production repo

Copy `production-template/` to a new repository. It has the full scaffold: `CLAUDE.md`, `Makefile`, `pyproject.toml`, pre-commit hooks, and directory structure.

```bash
cp -r production-template/ ~/projects/my-pipeline
cd ~/projects/my-pipeline
git init && git add . && git commit -m "Initial scaffold from production-template"
```

### 2. Write the production CLAUDE.md

The template in `production-template/CLAUDE.md` is a starting point. Customize it with:

- What this system does (one sentence)
- Architecture overview (data flow, key components)
- Domain context table (terms the agent needs to know)
- Specific rules for this codebase (naming conventions, error handling patterns)

### 3. Move and restructure code

Prototyping code is usually flat scripts. Production code needs structure:

```
pipelines/        # Entry points: one file per scheduled job
clients/          # API clients (Google Ads, BigQuery, etc.)
services/         # Business logic (classification, optimization, etc.)
domain/           # Data models, enums, constants
config/           # Settings, environment variable loading
prompts/          # LLM prompt templates
sql/
  pipeline/       # SQL used by pipelines (staging inserts, production merges)
  queries/        # Ad-hoc and reporting queries
  ddl/            # Table creation and schema changes
tests/
  fixtures/       # Test data, mock responses
docs/             # System documentation
```

### 4. Set up quality gates

These should all pass before any merge to main:

```bash
make all          # Runs: test, lint, format, typecheck
```

The `Makefile` in the template has targets for each. The `.pre-commit-config.yaml` runs `ruff` and `mypy` on every commit.

### 5. Set up staging-to-production data pattern

For BigQuery pipelines, use the two-table pattern:

1. Pipeline writes to a `staging_*` table (INSERT, always append)
2. Pipeline runs a MERGE from staging into the production table
3. MERGE uses `COALESCE(staging.field, production.field)` for safe upserts
4. Production table is what dashboards and downstream systems read

This gives you a rollback point (staging table has the raw inserts) and prevents partial writes from corrupting production data.

### 6. Add documentation

At minimum:
- `README.md` with setup instructions, architecture diagram, and how to run locally
- Docstrings on public functions
- SQL comments on non-obvious joins or filters

### 7. Set up CI/CD

Typical setup for a Python + BigQuery pipeline:

- GitHub Actions runs `make all` on every PR
- Merge to main triggers deployment (Cloud Run, Cloud Functions, or a scheduled VM)
- Environment variables via secrets management (GitHub Secrets, Secret Manager)

---

## Checklist

Before considering graduation complete:

- [ ] Production repo created from `production-template/`
- [ ] `CLAUDE.md` customized for this system
- [ ] Code restructured into `pipelines/`, `clients/`, `services/`
- [ ] `make all` passes (tests, lint, format, typecheck)
- [ ] Pre-commit hooks working
- [ ] Staging-to-production data pattern implemented (if applicable)
- [ ] CI/CD pipeline running on PRs
- [ ] README with setup and architecture docs
- [ ] Environment variables documented in `.env.example`
- [ ] First successful production run logged

---

## What stays in the prototyping repo

After graduation:

- Move the project folder to `projects/_archive/` with a note in `GOAL.md` pointing to the production repo
- Keep shared BigQuery schema docs in `bigquery/` (they're used across projects)
- Keep any reusable prompts or patterns in the prototyping repo's docs
- The production repo should be fully self-contained: it shouldn't depend on files in the prototyping repo
