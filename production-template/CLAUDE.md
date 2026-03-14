# CLAUDE.md

> Production operating protocol. Read at the start of every session.

---

## How to Assist

You are maintaining a production pipeline. Reliability and correctness come first, speed second.

1. Read `tasks/todo.md` for current work
2. Understand the change before making it: read relevant code, tests, and SQL
3. Run `make all` before and after any change
4. Never push directly to main. Always use a feature branch and PR.

## Before Work

- Read the relevant pipeline file and its tests
- Check `docs/` for architecture and data flow context
- For SQL changes, check `sql/ddl/` for the current schema

## After Work

- Run `make all` (must pass: tests, lint, format, typecheck)
- Update `tasks/todo.md`
- Commit with a clear message: what changed and why

---

## Architecture

<!-- Fill in: one-paragraph system description -->
<!-- Fill in: data flow diagram (text or mermaid) -->

```
Input → Pipeline → Output
```

## Code Quality

- Type hints on all functions (`mypy --strict`)
- `ruff` for linting and formatting
- Tests for every pipeline: at least one E2E test with known input/output
- No `# type: ignore` without a comment explaining why

## Pipeline Rules

- Every pipeline has `--dry-run` and `--limit N` flags
- Staging-to-production: INSERT staging, MERGE production
- All API mutations logged with execution ID and timestamp
- Batch processing: chunks of 50-100, never unbounded
- Retry with exponential backoff for transient failures
- Log enough to debug without re-running

## Domain Context

<!-- Fill in terms the agent needs to know for this system -->

| Term | Meaning |
|------|---------|
| <!-- e.g. tROAS --> | <!-- target return on ad spend --> |

## What NOT to Do

- Don't modify production tables directly. Always go through the pipeline.
- Don't skip tests to save time. If tests are slow, make them faster.
- Don't hardcode credentials. Everything comes from environment variables.
- Don't merge without `make all` passing.
- Don't add dependencies without justification. Keep the dependency tree small.
