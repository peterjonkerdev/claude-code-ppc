# Google Ads Data Transfer

The [Google Ads Data Transfer](https://cloud.google.com/bigquery/docs/google-ads-transfer) pushes daily snapshots of your entire Google Ads account to BigQuery. Entity dimensions, performance stats, bid strategy settings, search terms, shopping products, all of it.

**Dataset location:** `your-project.your-dataset` _(update during setup)_

### Other BigQuery Data Transfers

Google Ads is the most common starting point, but the [BigQuery Data Transfer Service](https://cloud.google.com/bigquery/docs/transfer-service-overview) supports other platforms natively:

| Source | Transfer type | What you get |
|--------|--------------|-------------|
| **Display & Video 360** | Native Data Transfer | Impression, click, conversion, and reach data across programmatic buys |
| **Campaign Manager 360** | Native Data Transfer | Cross-channel attribution, Floodlight conversions, rich media events |
| **Search Ads 360** | Native Data Transfer | Cross-engine search management data (Google, Microsoft, Yahoo) |
| **Google Analytics 4** | Native BigQuery export | Event-level data, user properties, session data, ecommerce events |
| **YouTube** | Via DV360 transfer | Video ad performance (or YouTube Content Owner transfer for creators) |

For platforms without a native transfer (Meta, Microsoft Ads, TikTok, LinkedIn, Amazon Ads), use a connector tool like Fivetran, Supermetrics, Funnel, or Stitch, or build your own ingestion via the platform's API. The same documentation pattern applies: point Claude at the dataset, let it discover the schema, document tables you use frequently.

---

## How to use this

You don't need to memorize table schemas. Claude discovers them on demand via `INFORMATION_SCHEMA`. What you do need are the rules and gotchas that aren't obvious from the schema alone.

When you find yourself querying a table repeatedly, document it using `bigquery/schemas/_template.md` with the business context that INFORMATION_SCHEMA can't tell you (what micros means, which filters to apply by default, common join patterns).

---

## Critical Rules

### Two table types per entity

| Pattern | Type | Use for | Required filter |
|---------|------|---------|-----------------|
| `ads_EntityName_XXXXXXXXXX` | View, latest snapshot only | Dimension lookups, current state | `WHERE _DATA_DATE = _LATEST_DATE` |
| `p_ads_EntityName_XXXXXXXXXX` | Partitioned table, full history | Date-range analysis, trends | `WHERE segments_date BETWEEN ... AND ...` |

The `XXXXXXXXXX` suffix is your Google Ads customer ID (digits only, no dashes).

**Never use `ads_*` views for stats over a date range.** They only contain the latest day.

### Cost and bid fields are in micros

All monetary fields (cost, CPC bids, CPA targets, budgets) are stored as micros. Always divide by 1,000,000:

```sql
metrics_cost_micros / 1000000 AS cost
campaign_budget_amount_micros / 1000000 AS daily_budget
```

### Ratios

Always use `SAFE_DIVIDE()` and aggregate before dividing:

```sql
-- Correct
SAFE_DIVIDE(SUM(metrics_cost_micros), SUM(metrics_clicks)) / 1000000 AS avg_cpc

-- Wrong: averaging pre-calculated ratios skews toward low-volume days
AVG(SAFE_DIVIDE(metrics_cost_micros, metrics_clicks)) / 1000000 AS avg_cpc
```

### Data freshness

The Data Transfer runs once per day, usually overnight. Today's data isn't available until tomorrow.

---

## Schema Discovery

Point Claude at your dataset and it discovers everything:

```sql
-- List all tables in the Data Transfer dataset
SELECT table_name, table_type
FROM `your-project.your-dataset.INFORMATION_SCHEMA.TABLES`
ORDER BY table_name;

-- Inspect columns for a specific table
SELECT column_name, data_type, description
FROM `your-project.your-dataset.INFORMATION_SCHEMA.COLUMNS`
WHERE table_name = 'p_ads_CampaignBasicStats_XXXXXXXXXX'
ORDER BY ordinal_position;

-- Check available date ranges
SELECT MIN(segments_date) AS earliest, MAX(segments_date) AS latest
FROM `your-project.your-dataset.p_ads_CampaignBasicStats_XXXXXXXXXX`;
```

When you document a frequently-used table, use `bigquery/schemas/_template.md` and add it to the Custom Table Index in `bigquery/README.md`.

---

## Available Tables Reference

All tables below exist in both `ads_*` (latest view) and `p_ads_*` (partitioned history) variants.

### Entity Tables (Dimension Data)

| Table | What it contains |
|-------|-----------------|
| `Campaign` | Name, status, channel type, bidding strategy, tROAS/tCPA targets, daily budget |
| `AdGroup` | Name, status, CPC bid, campaign association |
| `Keyword` | Text, match type, status, bid, quality score |
| `Ad` | Type, final URLs, RSA headlines/descriptions, ad strength |
| `BidGoal` | Portfolio bid strategy settings and targets |
| `AdGroupAudience` | Audience targeting at ad group level |
| `CampaignCriterion` | Campaign-level targeting criteria (locations, languages, etc.) |
| `NegativeKeyword` | Negative keyword lists and associations |

### Stats Tables (Performance Metrics)

| Table | What it contains |
|-------|-----------------|
| `CampaignBasicStats` | Impressions, clicks, cost at campaign level |
| `CampaignConversionStats` | Conversions, conversion value, ROAS at campaign level |
| `AdGroupBasicStats` | Impressions, clicks, cost at ad group level |
| `AdGroupConversionStats` | Conversions, conversion value at ad group level |
| `KeywordBasicStats` | Impressions, clicks, cost at keyword level |
| `KeywordConversionStats` | Conversions, conversion value at keyword level |
| `SearchQueryStats` | Search term performance (the raw search query report) |
| `LandingPageStats` | Performance by landing page URL per campaign |
| `ShoppingProductStats` | Product-level performance for Shopping campaigns |
| `ProductGroupStats` | Performance by product group subdivision |
| `BidGoalStats` | Bid strategy performance metrics |
| `AdBasicStats` | Impressions, clicks, cost at ad level |

### Less Common But Useful

| Table | What it contains |
|-------|-----------------|
| `GeoStats` | Performance by geographic location |
| `PlacementStats` | Performance by Display/Video placement |
| `AgeRangeStats` | Performance by age demographic |
| `GenderStats` | Performance by gender demographic |
| `ParentalStatusStats` | Performance by parental status |
| `DeviceStats` | Performance by device type |
| `HourlyStats` | Hourly performance breakdowns |
| `BudgetStats` | Budget utilization metrics |

Full field reference: [Google Ads Transfer documentation](https://cloud.google.com/bigquery/docs/google-ads-transfer)

---

## Notes

- `customer_id` in these tables is the numeric Google Ads account ID without dashes
- tROAS is stored as a ratio (e.g., 4.0 = 400%)
- Campaign status values: `ENABLED`, `PAUSED`, `REMOVED`
- Keyword match types: `BROAD`, `PHRASE`, `EXACT`
