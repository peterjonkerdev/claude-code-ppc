# BigQuery Decision Tree

> Which table do I query for which question?
> This file is populated by the agent during setup with your actual project IDs and table names.

---

## Quick Reference

| Question type | Table | Schema doc |
|---------------|-------|-----------|
| Ad platform performance (clicks, cost, conversions) | Google Ads Transfer — stats tables | `schemas/google_ads_transfer.md` |
| Historical tROAS / tCPA changes | Google Ads Transfer — Campaign snapshots | `schemas/google_ads_transfer.md` |
| Campaign budget changes over time | Google Ads Transfer — Campaign dimension | `schemas/google_ads_transfer.md` |
| Search term performance | Google Ads Transfer — SearchQueryStats | `schemas/google_ads_transfer.md` |
| Keyword performance | Google Ads Transfer — KeywordBasicStats | `schemas/google_ads_transfer.md` |
| Shopping product performance | Google Ads Transfer — ShoppingProductStats | `schemas/google_ads_transfer.md` |
| _(add your custom tables here)_ | _(dataset.table_name)_ | _(schemas/your-table.md)_ |

---

## Project and Dataset Reference

> Replace these placeholders during setup.

```
GCP Project:       your-project-id
Google Ads Transfer dataset: your-dataset (e.g. Transfers)
Custom datasets:   add yours here
```

---

## Table Type Rules

Two table types exist for every Google Ads entity:

**`ads_*` views** — latest snapshot only
- Use for: dimension lookups, current campaign/keyword/ad state
- Filter: `WHERE _DATA_DATE = _LATEST_DATE`

**`p_ads_*` partitioned tables** — full history
- Use for: date-range analysis, trend queries, historical comparisons
- Filter: `WHERE segments_date BETWEEN 'YYYY-MM-DD' AND 'YYYY-MM-DD'`

Critical rule: **never use `ads_*` views for stats over a date range** — they only contain the latest day.

---

## Schema Discovery

If you encounter a table that isn't documented yet, run these to discover its structure:

```sql
-- List all tables in a dataset
SELECT table_name, table_type
FROM `your-project.your-dataset.INFORMATION_SCHEMA.TABLES`
ORDER BY table_name;

-- Inspect columns for a specific table
SELECT column_name, data_type, description
FROM `your-project.your-dataset.INFORMATION_SCHEMA.COLUMNS`
WHERE table_name = 'your_table'
ORDER BY ordinal_position;

-- Check available date ranges
SELECT MIN(segments_date) AS earliest, MAX(segments_date) AS latest, COUNT(*) AS total_rows
FROM `your-project.your-dataset.p_ads_CampaignBasicStats_XXXXXXXXXX`
WHERE segments_date >= DATE_SUB(CURRENT_DATE(), INTERVAL 365 DAY);
```

After discovery, document the table using the template at `bigquery/schemas/_template.md`.

---

## Custom Table Index

> Add entries here as you document your own BigQuery tables.

| Table | Dataset | What it contains | Schema doc |
|-------|---------|-----------------|-----------|
| _(none yet — add during setup)_ | — | — | — |
