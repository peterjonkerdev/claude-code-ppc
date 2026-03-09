# Channel Taxonomy

> Defines your channel hierarchy and default filters for BigQuery queries.
> Replace the example values with your actual channel structure during setup.

---

## Example Structure

This is an example. Your actual channels, subchannels, and stage labels will depend on how your Google Ads account is structured and how your data team has named things in BigQuery.

### Channels (top level)

| Channel | Description | Default filter |
|---------|-------------|---------------|
| Paid Search Non-Brand (PSNB) | Generic search — primary growth driver | _(your filter)_ |
| Paid Search Brand (PSB) | Brand-name queries — usually excluded from main analysis | _(your filter)_ |
| Paid Shopping | Product listing ads | _(your filter)_ |
| Performance Max | Google's automated campaign type | _(your filter)_ |
| Display | Banner and image ads | _(your filter)_ |
| Video | YouTube and video campaigns | _(your filter)_ |

### Subchannels (example)

If you segment campaigns by product line, region, or audience, document those segments here:

| Subchannel | Parent channel | Description |
|-----------|---------------|-------------|
| _(Product Line A)_ | Paid Search Non-Brand | _(description)_ |
| _(Product Line B)_ | Paid Search Non-Brand | _(description)_ |
| _(Region — US)_ | Paid Search Non-Brand | _(description)_ |

### Funnel Stages (example)

If you use funnel stage labels in your campaign naming or data:

| Stage | Description | Typical campaigns |
|-------|-------------|------------------|
| Attract | Top of funnel — awareness | Broad match, display, video |
| Engage | Mid funnel — consideration | Retargeting, DSA |
| Convert | Bottom of funnel — revenue | Brand, exact match, Shopping |

---

## SQL Filter Patterns

> Add your actual filter logic here once you know your campaign naming conventions.

```sql
-- Example: filter to Non-Brand search only
WHERE LOWER(campaign_name) NOT LIKE '%brand%'
  AND campaign_advertising_channel_type = 'SEARCH'

-- Example: filter to a specific product line
WHERE LOWER(campaign_name) LIKE '%product-line-a%'

-- Example: filter by funnel stage label in campaign name
WHERE LOWER(campaign_name) LIKE '%_convert_%'
```

---

## Important Notes

- Use `LOWER()` for all string comparisons against campaign names
- Brand campaigns are usually excluded from performance analysis by default — flag this explicitly in any query that includes them
- If your account uses labels instead of naming conventions, document the label IDs here
- Update this file to match your actual naming conventions after setup
