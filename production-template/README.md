# Production Template

Scaffold for a production PPC pipeline repository. Copy this entire folder to create a new production repo.

## Usage

```bash
cp -r production-template/ ~/projects/my-pipeline
cd ~/projects/my-pipeline
git init
```

Then customize:

1. **`CLAUDE.md`**: Fill in system description, architecture, domain context
2. **`pyproject.toml`**: Replace `your-pipeline-name` with the actual project name
3. **`.env.example`**: Add any environment variables your pipeline needs
4. **`Makefile`**: Adjust paths if your source isn't in the default locations

## Structure

```
CLAUDE.md                 # Agent operating protocol for this system
Makefile                  # test, lint, format, typecheck, all
pyproject.toml            # Project config with ruff/mypy/pytest
.pre-commit-config.yaml   # Pre-commit hooks (ruff + mypy)
.env.example              # Environment variable template
.python-version           # Python version pin
.gitignore                # Python + tooling ignores

pipelines/                # Entry points: one file per scheduled job
clients/                  # API clients (Google Ads, BigQuery, etc.)
services/                 # Business logic (classification, optimization)
domain/                   # Data models, enums, constants
config/                   # Settings, environment variable loading
prompts/                  # LLM prompt templates
sql/
  pipeline/               # SQL used by pipelines (staging, merge)
  queries/                # Ad-hoc and reporting queries
  ddl/                    # Table creation and schema changes
tests/
  fixtures/               # Test data, mock responses
docs/                     # System documentation
logs/                     # Runtime logs (gitignored)
tasks/
  todo.md                 # Task tracking
```

## Quick start after setup

```bash
uv sync                   # Install dependencies
make all                  # Run tests, lint, format, typecheck
pre-commit install        # Set up git hooks
```
