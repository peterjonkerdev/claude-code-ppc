# Google Ads Data Transfer — Schema Reference

The Google Ads Data Transfer gives you the most granular Google Ads data available in a queryable format. Daily snapshots of every entity in your account, pushed to BigQuery automatically.

**Dataset location:** `your-project.your-dataset` _(update during setup)_

---

## Table Naming Convention

Every entity has two table variants:

| Pattern | Type | Use for |
|---------|------|---------|
| `ads_EntityName_XXXXXXXXXX` | View — latest snapshot only | Dimension lookups, current state |
| `p_ads_EntityName_XXXXXXXXXX` | Partitioned table — full history | Date-range analysis, trend queries |

The `XXXXXXXXXX` suffix is your Google Ads customer ID (digits only, no dashes).

---

## Entity Tables (Dimension Data)

Use these with `WHERE _DATA_DATE = _LATEST_DATE` for current state.

### Campaign
```sql
SELECT
  campaign_id,
  campaign_name,
  campaign_status,                                              -- ENABLED, PAUSED, REMOVED
  campaign_advertising_channel_type,                           -- SEARCH, SHOPPING, DISPLAY, VIDEO
  campaign_bidding_strategy_type,                              -- TARGET_ROAS, TARGET_CPA, MAXIMIZE_CONVERSIONS, etc.
  campaign_maximize_conversion_value_target_roas,              -- tROAS target (as a ratio, e.g. 4.0 = 400%)
  campaign_target_cpa_target_cpa_micros / 1000000 AS target_cpa,
  campaign_budget_amount_micros / 1000000 AS daily_budget
FROM `your-project.your-dataset.ads_Campaign_XXXXXXXXXX`
WHERE _DATA_DATE = _LATEST_DATE
```

Key fields:
- `campaign_maximize_conversion_value_target_roas` — tROAS target for Smart Bidding campaigns
- `campaign_target_cpa_target_cpa_micros` — tCPA in micros, divide by 1,000,000 for real value
- Cost fields are in micros — always divide by 1,000,000

### AdGroup
```sql
SELECT
  ad_group_id,
  ad_group_name,
  ad_group_status,
  campaign_id,
  ad_group_cpc_bid_micros / 1000000 AS max_cpc
FROM `your-project.your-dataset.ads_AdGroup_XXXXXXXXXX`
WHERE _DATA_DATE = _LATEST_DATE
```

### Keyword
```sql
SELECT
  criterion_id,
  ad_group_criterion_keyword_text,
  ad_group_criterion_keyword_match_type,  -- BROAD, PHRASE, EXACT
  ad_group_criterion_status,
  ad_group_id,
  campaign_id
FROM `your-project.your-dataset.ads_Keyword_XXXXXXXXXX`
WHERE _DATA_DATE = _LATEST_DATE
```

### Ad (RSA)
```sql
SELECT
  ad_id,
  ad_name,
  ad_type,
  ad_final_urls,
  ad_strength,
  ad_group_id,
  campaign_id
FROM `your-project.your-dataset.ads_Ad_XXXXXXXXXX`
WHERE _DATA_DATE = _LATEST_DATE
```

---

## Stats Tables (Performance Metrics)

Use `p_ads_*` tables with `segments_date` for any date-range query.

### CampaignBasicStats
Impressions, clicks, cost at campaign level.

```sql
SELECT
  campaign_id,
  segments_date,
  metrics_impressions,
  metrics_clicks,
  metrics_cost_micros / 1000000 AS cost,
  SAFE_DIVIDE(metrics_cost_micros, metrics_clicks) / 1000000 AS avg_cpc,
  SAFE_DIVIDE(metrics_clicks, metrics_impressions) AS ctr
FROM `your-project.your-dataset.p_ads_CampaignBasicStats_XXXXXXXXXX`
WHERE segments_date BETWEEN DATE_SUB(CURRENT_DATE(), INTERVAL 30 DAY) AND CURRENT_DATE()
```

### CampaignConversionStats
Conversions and conversion value at campaign level.

```sql
SELECT
  campaign_id,
  segments_date,
  metrics_conversions,
  metrics_conversions_value,
  metrics_cost_micros / 1000000 AS cost,
  SAFE_DIVIDE(metrics_conversions_value, metrics_cost_micros / 1000000) AS roas,
  SAFE_DIVIDE(metrics_cost_micros / 1000000, metrics_conversions) AS cpa
FROM `your-project.your-dataset.p_ads_CampaignConversionStats_XXXXXXXXXX`
WHERE segments_date BETWEEN DATE_SUB(CURRENT_DATE(), INTERVAL 30 DAY) AND CURRENT_DATE()
```

### SearchQueryStats
Search term report — raw queries that triggered your ads.

```sql
SELECT
  search_term_view_search_term,
  campaign_id,
  ad_group_id,
  SUM(metrics_cost_micros) / 1000000 AS cost,
  SUM(metrics_clicks) AS clicks,
  SUM(metrics_impressions) AS impressions,
  SUM(metrics_conversions) AS conversions,
  SAFE_DIVIDE(SUM(metrics_conversions), SUM(metrics_clicks)) AS cvr
FROM `your-project.your-dataset.p_ads_SearchQueryStats_XXXXXXXXXX`
WHERE segments_date BETWEEN DATE_SUB(CURRENT_DATE(), INTERVAL 90 DAY) AND CURRENT_DATE()
GROUP BY 1, 2, 3
ORDER BY cost DESC
```

### KeywordBasicStats / KeywordConversionStats
Performance at keyword level. Same structure as campaign stats but at `criterion_id` granularity.

### AdGroupBasicStats / AdGroupConversionStats
Performance at ad group level.

### LandingPageStats
Performance by landing page URL, broken out per campaign.

```sql
SELECT
  expanded_landing_page_view_expanded_final_url AS landing_page,
  campaign_id,
  SUM(metrics_impressions) AS impressions,
  SUM(metrics_clicks) AS clicks,
  SUM(metrics_cost_micros) / 1000000 AS cost
FROM `your-project.your-dataset.p_ads_LandingPageStats_XXXXXXXXXX`
WHERE segments_date BETWEEN DATE_SUB(CURRENT_DATE(), INTERVAL 30 DAY) AND CURRENT_DATE()
GROUP BY 1, 2
ORDER BY cost DESC
```

### ShoppingProductStats
Product-level performance for Shopping campaigns. Join with your product catalog for margin-aware analysis.

```sql
SELECT
  segments_product_item_id AS product_id,
  segments_product_title AS product_title,
  campaign_id,
  SUM(metrics_impressions) AS impressions,
  SUM(metrics_clicks) AS clicks,
  SUM(metrics_cost_micros) / 1000000 AS cost,
  SUM(metrics_conversions) AS conversions,
  SUM(metrics_conversions_value) AS revenue
FROM `your-project.your-dataset.p_ads_ShoppingProductStats_XXXXXXXXXX`
WHERE segments_date BETWEEN DATE_SUB(CURRENT_DATE(), INTERVAL 30 DAY) AND CURRENT_DATE()
GROUP BY 1, 2, 3
ORDER BY cost DESC
```

---

## Common Patterns

### Track tROAS changes over time
```sql
WITH daily_settings AS (
  SELECT
    campaign_id, campaign_name,
    DATE(_PARTITIONTIME) AS snapshot_date,
    campaign_maximize_conversion_value_target_roas AS target_roas
  FROM `your-project.your-dataset.p_ads_Campaign_XXXXXXXXXX`
  WHERE _PARTITIONTIME >= TIMESTAMP(DATE_SUB(CURRENT_DATE(), INTERVAL 90 DAY))
    AND campaign_maximize_conversion_value_target_roas IS NOT NULL
    AND campaign_maximize_conversion_value_target_roas > 0
),
with_previous AS (
  SELECT *,
    LAG(target_roas) OVER (PARTITION BY campaign_id ORDER BY snapshot_date) AS prev_roas
  FROM daily_settings
)
SELECT
  campaign_name, snapshot_date,
  prev_roas AS old_troas,
  target_roas AS new_troas,
  ROUND(target_roas - prev_roas, 2) AS change
FROM with_previous
WHERE prev_roas IS NOT NULL AND target_roas != prev_roas
ORDER BY snapshot_date DESC
```

### Multi-level join (Campaign → Ad Group → Stats)
```sql
SELECT
  c.campaign_name,
  ag.ad_group_name,
  SUM(s.metrics_impressions) AS impressions,
  SUM(s.metrics_clicks) AS clicks,
  SUM(s.metrics_cost_micros) / 1000000 AS cost
FROM `your-project.your-dataset.p_ads_AdGroupBasicStats_XXXXXXXXXX` s
LEFT JOIN (
  SELECT ad_group_id, campaign_id, customer_id, ad_group_name
  FROM `your-project.your-dataset.ads_AdGroup_XXXXXXXXXX`
  WHERE _DATA_DATE = _LATEST_DATE
) ag USING (ad_group_id, campaign_id, customer_id)
LEFT JOIN (
  SELECT campaign_id, customer_id, campaign_name
  FROM `your-project.your-dataset.ads_Campaign_XXXXXXXXXX`
  WHERE _DATA_DATE = _LATEST_DATE
) c USING (campaign_id, customer_id)
WHERE s.segments_date BETWEEN DATE_SUB(CURRENT_DATE(), INTERVAL 30 DAY) AND CURRENT_DATE()
GROUP BY 1, 2
ORDER BY cost DESC
```

---

## Notes

- All cost and bid fields are in **micros** — divide by 1,000,000 to get real currency values
- Always use `SAFE_DIVIDE()` for ratios to avoid division-by-zero errors
- `customer_id` in these tables is the numeric Google Ads account ID without dashes
- The Data Transfer runs once per day, usually overnight — data for today isn't available until tomorrow
