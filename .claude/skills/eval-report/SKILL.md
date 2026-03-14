# Eval Report

> Run evaluation pipelines and report on prompt/model accuracy.

## Trigger phrases

- "run evals"
- "check accuracy"
- "compare prompt versions"
- "how is the classifier doing"

## Steps

### 1. Find the eval pipeline

Look in the active project for:
- An eval script (e.g., `scripts/eval.py`, `scripts/run_evals.py`)
- A gold set or test fixtures (e.g., `tests/fixtures/gold_set.csv`, `eval_data/`)
- Prompt versions in `prompts/`

If no eval pipeline exists, offer to create one using the project's existing pipeline and a sample of known-good input/output pairs.

### 2. Run the evaluation

```bash
uv run python scripts/eval.py --output eval_results.json
```

If the pipeline supports it, run with `--limit` first to verify it works before a full run.

### 3. Present results

Report in this format:

**Overall accuracy:** X/Y correct (Z%)

| Category | Correct | Total | Accuracy | Change vs last run |
|----------|---------|-------|----------|--------------------|
| ... | ... | ... | ... | ... |

### 4. Regression check

Compare against the previous eval run (if results exist):
- Flag any category where accuracy dropped more than 2%
- Highlight categories that improved
- Note any new categories not in the previous run

### 5. Failure analysis

For incorrect results:
- Show the 5 most confident wrong answers (highest confidence, wrong label)
- Group failures by pattern (e.g., "all brand terms misclassified as generic")
- Suggest specific prompt edits that might fix the pattern

### 6. Suggest improvements

Based on the failure analysis:
- Propose concrete prompt edits
- Suggest additional gold set examples for weak categories
- Recommend whether to iterate on the prompt or try a different approach

## Arguments

- No arguments: run the eval for the current active project
- Project name: run evals for a specific project
- `--compare v1 v2`: compare two prompt versions side by side
