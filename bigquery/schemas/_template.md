# [Table Name] — Schema Reference

> Template: copy this file and rename it to document any new BigQuery table.
> Add the new file to `bigquery/README.md` and `docs/index.md`.

**Dataset:** `your-project.your-dataset`
**Table:** `table_name`
**Updated:** YYYY-MM-DD
**Source:** _(where does this data come from? Google Ads Transfer, internal pipeline, third-party, etc.)_

---

## What this table is

_(One sentence: what data it contains and what it's used for.)_

---

## Key fields

| Field | Type | Description |
|-------|------|-------------|
| `field_name` | STRING | _(what it means, any gotchas)_ |
| `date_field` | DATE | _(is this a partition field? what range is available?)_ |
| `cost_field` | INT64 | _(if in micros, note: divide by 1,000,000)_ |

---

## Default filters

_(Any filters that should almost always be applied. Examples:)_
- `WHERE status = 'ACTIVE'`
- `WHERE _DATA_DATE = _LATEST_DATE` (for dimension views)
- `WHERE date BETWEEN ... AND ...`

---

## Common query patterns

```sql
-- Basic query
SELECT
  field_1,
  field_2,
  SUM(metric) AS total_metric
FROM `your-project.your-dataset.table_name`
WHERE date >= DATE_SUB(CURRENT_DATE(), INTERVAL 30 DAY)
GROUP BY 1, 2
ORDER BY total_metric DESC
```

---

## How it joins to other tables

| Join to | Join keys | Notes |
|---------|-----------|-------|
| _(table name)_ | `field_a = field_b` | _(when and why)_ |

---

## Notes

- _(Anything important that isn't obvious from the schema)_
- _(Data quality issues or known gaps)_
- _(Refresh frequency)_
