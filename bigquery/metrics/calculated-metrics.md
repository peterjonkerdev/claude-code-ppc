# Calculated Metrics — Canonical Definitions

> Single source of truth for all business metric formulas.
> Always use `SAFE_DIVIDE()` to prevent division-by-zero errors.
> Update this file to match your actual business definitions during setup.

---

## Advertising Metrics

### ROAS (Return on Ad Spend)
```sql
-- Basic ROAS
SAFE_DIVIDE(SUM(metrics_conversions_value), SUM(metrics_cost_micros) / 1000000)

-- As percentage
SAFE_DIVIDE(SUM(metrics_conversions_value), SUM(metrics_cost_micros) / 1000000) * 100
```
Interpretation: 4.0 = £4 revenue per £1 spend

### CPA (Cost Per Acquisition)
```sql
SAFE_DIVIDE(SUM(metrics_cost_micros) / 1000000, SUM(metrics_conversions))
```

### CPC (Cost Per Click)
```sql
SAFE_DIVIDE(SUM(metrics_cost_micros) / 1000000, SUM(metrics_clicks))
```

### CTR (Click-Through Rate)
```sql
SAFE_DIVIDE(SUM(metrics_clicks), SUM(metrics_impressions))
```

### CVR (Conversion Rate — Click to Conversion)
```sql
SAFE_DIVIDE(SUM(metrics_conversions), SUM(metrics_clicks))
```

---

## Business Metrics

> These use fields from your backend data. Update field names to match your actual tables.

### CAC (Customer Acquisition Cost)
```sql
SAFE_DIVIDE(SUM(costs), SUM(new_customers))
```
Use this when optimizing toward new customer acquisition rather than all conversions.

### LTV:CAC
```sql
SAFE_DIVIDE(SUM(lifetime_value), SUM(costs))
```
Interpretation:
- < 1.0: losing money per acquired customer
- 1.0–3.0: breaking even or marginal
- > 3.0: healthy
- > 5.0: excellent

### Average LTV
```sql
SAFE_DIVIDE(SUM(lifetime_value), SUM(new_customers))
```

### Revenue per Click
```sql
SAFE_DIVIDE(SUM(revenue), SUM(clicks))
```

### Margin-Adjusted ROAS
```sql
SAFE_DIVIDE(SUM(gross_margin), SUM(costs))
```
Use when you have margin data in BigQuery and want to optimize toward profit rather than revenue.

---

## Aggregation Rules

- Always aggregate (`SUM`) before calculating ratios — never average ratios
- Correct: `SAFE_DIVIDE(SUM(conversions_value), SUM(cost))`
- Wrong: `AVG(roas_column)` — this gives a misleading weighted average

---

## Notes

- Add your own business metrics here as you define them
- If a metric formula is used in multiple pipelines, it should be defined here once and referenced
- Include the SQL for any metric that has a non-obvious calculation (e.g. LTV using a predictive model)
