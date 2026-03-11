# Tools & Workflows

Claude Code doesn't just generate code. It executes tools, reads output, spots problems, fixes them, and iterates. This is the difference between getting suggestions and having an agent that actually operates your systems.

---

## CLI Tools

### `bq` (BigQuery CLI)

Query BigQuery directly from the terminal. Claude writes SQL, runs it, reads the results, identifies issues, and iterates until the output is correct. Zero context token overhead (unlike the BigQuery MCP, which costs ~15-20K tokens at session start).

```bash
bq query --use_legacy_sql=false 'SELECT COUNT(*) FROM `your-project.your_dataset.your_table`'
```

Works across any GCP project you have IAM access to. Always prefer `bq` over MCP in Claude Code.

### `gcloud`

Google Cloud authentication, project switching, service account management. Essential for BigQuery access.

```bash
gcloud auth application-default login
gcloud config set project your-project-id
```

### `git`

Version control, diffs, branch management, history. Claude uses git history to understand changes over time. The weekly-update skill reads `git log` to generate progress reports.

### `python` / `uv`

Execute scripts, manage dependencies, run full pipelines. `uv` handles package management with lock files so environments are reproducible. Prefer `uv` over `pip` for new projects.

```bash
uv init my-pipeline
uv add pandas google-cloud-bigquery
uv run python my_script.py
```

### `gh` (GitHub CLI)

Pull requests, issues, code review, all from the terminal. Faster than switching to the browser.

```bash
gh pr create --title "Add search term pipeline" --body "Initial implementation"
gh issue list --label "bug"
```

### `ruff`

Linting and formatting in one tool. Catches style issues and basic code problems before they compound. Fast enough to run on every save.

```bash
ruff check .
ruff format .
```

### `mypy`

Static type checking. Verifies that type hints are consistent across the codebase. Particularly useful for catching AI hallucinations in code: when Claude generates a function call with the wrong argument types, mypy catches it before runtime.

```bash
mypy src/ --ignore-missing-imports
```

### `pytest`

Unit tests for logic, integration tests for API calls, BigQuery dry runs for SQL validation. Tests are how you know something actually works.

```bash
pytest tests/ -v
pytest tests/test_pipeline.py -k "test_batch_processing"
```

---

## Workflows

### Plan mode

For any non-trivial build, plan mode is where you should spend most of your time. Type `/plan` or press `Shift+Tab` twice. Claude enters read-only research mode: it can explore your codebase, read docs, and query schemas, but doesn't write files or execute commands yet. It's forced to think before acting.

**The 70/30 rule:** For complex builds, spend ~70% of the time in plan mode and ~30% on execution and validation. The more context you provide during planning, the better the output during execution.

**When to use it:**
- Before any build that touches multiple files or systems
- When the architecture has multiple valid approaches
- When you need to understand existing code before modifying it
- Essentially: any task that isn't a quick one-liner

For this discovery repo, most tasks won't need plan mode. Quick analyses, data exploration, one-off scripts: just ask and iterate. Save plan mode for when you're building something that will graduate to production.

### Model routing

Switch models mid-conversation with `/model`. Not every task needs the most capable model.

| Task type | Model | Why |
|-----------|-------|-----|
| Architecture, scoping, complex debugging | **Opus** | Deep reasoning, handles ambiguity |
| SQL from a clear spec, building scripts, docs | **Sonnet** | Strong execution with good judgment |
| Running pipelines, file edits, formatting | **Haiku** | Fast, reliable, follows instructions precisely |

```bash
/model opus    # Planning and architecture
/model sonnet  # Execution requiring judgment
/model haiku   # Execution against a clear spec
```

Start in Opus for planning, drop to Sonnet or Haiku once the plan is confirmed.

### The execute-validate-iterate loop

This is the core power of Claude Code. Claude doesn't just write a query. It runs it, validates the output, and fixes problems autonomously.

**Example:** "What was the average CPC last week, and was it significantly different from the previous 52 weeks?"

1. Claude checks BigQuery schema docs for the right table and columns
2. Writes a SQL query for last week's CPC and the 52-week average
3. Runs it via `bq query`
4. Reads the output, notices a column name doesn't match
5. Queries `INFORMATION_SCHEMA` to discover actual column names
6. Fixes the query, re-runs
7. Extends with standard deviation and z-score
8. Returns the analysis with context

No copy-pasting between tools. No switching to the BigQuery console. The agent handles the iteration loop that a human would do manually across multiple browser tabs.

This extends to multi-step analysis. Claude can query one dataset to identify a problem, query a second to find the cause, query a third to validate the hypothesis, all autonomously within a single conversation turn.

### Spec-driven development

Plan mode is powerful but ephemeral: the conversation disappears when the session ends. For features that matter, write a lightweight spec before building. The spec captures what "done" looks like (expected output, acceptance criteria, verification steps) as a durable markdown file that outlives the session.

Specs live in `specs/` using the template at `specs/TEMPLATE.md`. The workflow:

```
roadmap → spec (what does "done" look like?) → plan → build → verify against spec → commit
```

Not everything needs a spec. Bug fixes, config tweaks, and quick explorations don't. Specs help most when the definition of "done" is ambiguous: new pipelines, prompt changes, schema migrations, features heading to production.

For more on this approach, see [Spec-Driven Development with AI](https://peterjonker.dev/posts/spec-driven-development-ai-coding/).

---

_This file covers the tools referenced in the [blog post](https://peterjonker.dev/posts/agentic-coding-ppc-claude-code/). Add your own tools and workflows as your setup evolves._
