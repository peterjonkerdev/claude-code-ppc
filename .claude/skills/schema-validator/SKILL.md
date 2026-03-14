# Schema Validator

> Compare documented BigQuery schemas against actual tables. Find mismatches, missing tables, and undocumented columns.

## Trigger phrases

- "validate schemas"
- "discover tables"
- "check docs against BigQuery"
- "what tables do we have"
- "are our schema docs up to date"

## Steps

### 1. Read context

Get GCP project and dataset names from `setup/context.md` and `bigquery/README.md`.

If no project/dataset is configured, ask the user before proceeding.

### 2. Discover actual tables

Query `INFORMATION_SCHEMA` for all tables in the configured datasets:

```sql
SELECT table_name, table_type, creation_time
FROM `your-project.your_dataset.INFORMATION_SCHEMA.TABLES`
ORDER BY table_name
```

### 3. Discover actual columns

For each table (or tables the user asks about):

```sql
SELECT table_name, column_name, data_type, is_nullable
FROM `your-project.your_dataset.INFORMATION_SCHEMA.COLUMNS`
WHERE table_name = 'target_table'
ORDER BY ordinal_position
```

### 4. Compare against docs

Read all schema docs in `bigquery/schemas/`. For each documented table:

- **Exists in BigQuery?** Flag if documented but not found (renamed? deleted?)
- **Column match:** Compare documented columns vs actual columns
- **Missing from docs:** Columns in BigQuery but not documented
- **Extra in docs:** Columns documented but not in BigQuery (stale docs)

### 5. Present results

**Schema Health Summary**

| Table | Documented | Exists | Columns Match | Issues |
|-------|-----------|--------|---------------|--------|
| ... | ... | ... | ... | ... |

**Undocumented tables:** List any tables in BigQuery with no corresponding schema doc.

**Column mismatches:** For each table with issues, show a diff:
- Added (in BQ, not in docs)
- Removed (in docs, not in BQ)
- Type changed

### 6. Offer to fix

For each issue found, offer to:
- Update the schema doc to match reality
- Create a new schema doc from `bigquery/schemas/_template.md` for undocumented tables
- Remove stale entries from docs

Always confirm with the user before making changes.

## Arguments

- No arguments: validate all documented schemas
- Dataset name: validate only tables in that dataset
- Table name: deep-dive on a single table
