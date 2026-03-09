---
name: repo-health
description: Audit the repository structure for scalability and correctness. Run periodically to catch orphaned projects, dead links, bloated files, and missing documentation.
allowed-tools: Bash(ls *), Bash(wc *), Bash(find *), Read, Grep, Glob
---

# Repository Health Check

Audit the repository structure and report issues. All checks are dynamic — they discover what exists rather than checking a hardcoded list.

## Step 1: Discover the repo

Discover all projects:
```bash
ls -d projects/*/
```

Discover all BigQuery schema directories:
```bash
find bigquery/schemas -type d -mindepth 1
```

## Step 2: Run checks

### Check 1: GOAL.md presence
Every directory under `projects/` must have a `GOAL.md` file.

### Check 2: docs/index.md coverage
Read `docs/index.md`. Every project under `projects/` must appear in the documentation index.

### Check 3: README.md coverage
Read `README.md`. Every project under `projects/` must appear in the project listing.

### Check 4: Dead links in docs/index.md
For every file path referenced in `docs/index.md`, verify the file actually exists on disk. Report any paths that point to non-existent files.

### Check 5: CLAUDE.md line count
Count lines in the root `CLAUDE.md`. Warn if over 100 lines (sweet spot is 30-100).

### Check 6: Orphaned docs
Find all `.md` files under `projects/*/docs/` and check that each is referenced from `docs/index.md`. Report any that are not linked.

### Check 7: BigQuery schema coverage
For each directory under `bigquery/schemas/` that contains a `README.md`, read it and check that every table listed has a corresponding schema doc in that directory. Also check that every `.md` file (other than README.md) is referenced in the README.

### Check 8: Python project structure
For any `scripts/` or Python package directory found under `projects/`, check:
- Has a README.md explaining what it does
- Has a `pyproject.toml` or `requirements.txt` if it contains `.py` files
- Entry point scripts have a top-of-file docstring

### Check 9: Project CLAUDE.md consistency
Find all `CLAUDE.md` files in the repo. For any project-level CLAUDE.md, verify it doesn't contradict the root CLAUDE.md.

## Step 3: Present results

Output a summary table:

```
| Check                        | Status | Details              |
|------------------------------|--------|----------------------|
| GOAL.md presence             | PASS   | 6/6 projects         |
| docs/index.md coverage       | PASS   | 6/6 projects         |
| README.md coverage           | FAIL   | Missing: project-x   |
| Dead links in docs/index.md  | PASS   | 0 dead links         |
| CLAUDE.md line count         | PASS   | 55 lines             |
| Orphaned docs                | PASS   | 0 orphaned           |
| BigQuery schema coverage     | PASS   | All schemas covered  |
| Python project structure     | WARN   | 1 missing docstring  |
| Project CLAUDE.md consistency| PASS   | No conflicts         |
```

Then list details for any FAIL or WARN results, with specific file paths and suggested fixes.

## Arguments

- No arguments: run all checks
- `$ARGUMENTS` can be: "quick" (checks 1-5 only), or a specific check number like "4" (dead links only)
