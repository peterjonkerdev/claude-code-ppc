# Technical Patterns

> Patterns are added as they emerge from your work. Start with these foundations, let it grow.
> This file is a living reference. Update it when you discover something worth remembering.

---

## SQL

### Table selection
- `p_ads_*` tables + `WHERE segments_date BETWEEN` for date-range queries (full history)
- `ads_*` views + `WHERE _DATA_DATE = _LATEST_DATE` for current state only
- Never mix the two in the same query

### Cost and bid fields
- All cost and bid fields are in micros. Always divide by 1,000,000:
  ```sql
  SAFE_DIVIDE(SUM(metrics_cost_micros), 1000000) AS cost
  ```

### Ratios and aggregation
- Use `SAFE_DIVIDE()` for all ratio calculations
- Aggregate before calculating ratios:
  ```sql
  -- Correct
  SAFE_DIVIDE(SUM(metrics_cost_micros), SUM(metrics_clicks)) AS avg_cpc_micros

  -- Wrong: averaging pre-calculated ratios skews toward low-volume days
  AVG(SAFE_DIVIDE(metrics_cost_micros, metrics_clicks)) AS avg_cpc_micros
  ```

### String comparisons
- Use `LOWER()` for all string comparisons and matching
- Campaign name filters: `LOWER(campaign_name) LIKE '%brand%'`

---

## Python Pipelines

### Standard flags
Every pipeline CLI should support:
- `--dry-run`: log what would happen without executing writes or mutations
- `--limit N`: process only N records (for testing and development)

### Staging-to-production pattern
For BigQuery write pipelines:
1. INSERT results into a `staging_*` table (append-only)
2. MERGE from staging into the production table:
   ```sql
   MERGE `project.dataset.production_table` AS prod
   USING `project.dataset.staging_table` AS stg
   ON prod.id = stg.id
   WHEN MATCHED THEN UPDATE SET
     prod.field = COALESCE(stg.field, prod.field)
   WHEN NOT MATCHED THEN INSERT (...)
     VALUES (...)
   ```
3. Production table is what dashboards and downstream systems read

### Config hierarchy
- `.env` for secrets (API keys, project IDs). Never committed.
- `config/settings.py` for runtime settings (batch sizes, rate limits, timeouts)
- Pipeline-specific configs extend the base settings

### Error handling
- Batching: don't process everything in one API call
- Rate limiting: respect API quotas
- Retry logic: exponential backoff for transient failures
- Error handling: log failures with context, don't silently skip
- Logging: enough to debug without re-running

### Type hints
- Type hints on all functions in production code
- `mypy --strict` in production repos
- Prototyping code: type hints encouraged but not required

---

## LLM Prompts

### Storage
- All prompts live as `.md` files in the project's `prompts/` folder
- Never hardcode prompt strings in Python scripts
- Version prompts with git so you can diff changes

### Structured output
- Use Pydantic models for output validation
- Define the expected schema, parse the LLM response into it, catch validation errors
- This catches hallucinated fields, wrong types, and missing required data

### Temperature
- Classification tasks: 0.0-0.2 (deterministic)
- Creative tasks (ad copy, descriptions): 0.5-0.8
- Analysis and summarization: 0.0-0.3

### Batch processing
- Process in chunks of 50-100 items per LLM call
- Log each batch's results separately for debugging
- Track token usage per batch to estimate costs

### Web search enrichment pattern
For classification or enrichment tasks where the LLM needs external context:
1. First pass: classify with LLM using only the input data
2. Identify ambiguous results (low confidence, generic labels)
3. Web search for ambiguous items to get additional context
4. Second pass: re-classify ambiguous items with the enriched context

---

## Testing Patterns

### Three layers
1. **E2E tests**: Run the full pipeline with known input, verify the output matches expected results
2. **Unit tests**: Test individual functions (parsing, transformation, business logic)
3. **Evals**: For LLM outputs, compare against a gold set of human-verified labels

### Mocking strategy
- Never call real APIs in tests (no BigQuery queries, no Google Ads API calls)
- Mock at the client boundary: replace the API client with a fake that returns fixture data
- Store fixtures in `tests/fixtures/` as JSON or CSV files

### Gold set approach
For LLM output quality:
- Maintain a curated set of 100-500 input/output pairs with human-verified correct labels
- Run evals against this set after any prompt change
- Track accuracy by category, not just overall
- Flag regressions: any category that drops more than 2% needs investigation

---

## Google Ads API Patterns

### Read vs write
- **Read data via BigQuery**: Data Transfer tables have everything you need for analysis and reporting
- **Write data via API**: Use the Google Ads API only for mutations (bid changes, label assignments, campaign settings)

### Mutation logging
- Log every mutation with: execution ID, timestamp, entity ID, old value, new value, reason
- Store mutation logs in BigQuery for audit trail
- Always support `--dry-run` for testing mutations without applying them

### Rate limits
- Google Ads API has per-account and per-developer-token limits
- Batch mutations where possible (up to 5000 operations per mutate request)
- Implement exponential backoff for rate limit errors

---

_Add your own patterns here as they emerge._
