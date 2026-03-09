# Technical Patterns

> Reusable patterns and conventions for this repo.
> Add entries here as patterns emerge from the work — not upfront.

---

## SQL

- Use `SAFE_DIVIDE()` for all ratio calculations
- Use `LOWER()` for string comparisons
- Aggregate before calculating ratios: `SAFE_DIVIDE(SUM(a), SUM(b))` not `AVG(ratio_column)`

---

## Python Pipelines

Every pipeline should include:
- Batching (don't process everything in one API call)
- Rate limiting (respect API quotas)
- Retry logic (transient failures are normal)
- Error handling (log failures, don't silently skip)
- Logging (enough to debug without re-running)

---

## LLM Prompts

- Store all prompts as `.md` files in the project's `prompts/` folder
- Never hardcode prompt strings in Python scripts
- Temperature: low (0.0–0.2) for classification tasks, higher for creative tasks
- Output format: structured JSON for machine-readable outputs

---

_Add your own patterns here as they emerge._
