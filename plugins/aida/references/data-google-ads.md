# Data template — Google Ads
# Source: Google Ads API (future integration)
# Used by: campaign-simulator to generate realistic campaign data

## Schema

```
account_id: string
date_range:
  from: YYYY-MM-DD
  to: YYYY-MM-DD
currency: EUR

campaigns:
  - id: string
    name: string
    status: ACTIVE | PAUSED | REMOVED
    type: SEARCH | DISPLAY | SHOPPING | VIDEO
    budget_daily_eur: number
    start_date: YYYY-MM-DD

    metrics:
      impressions: integer
      clicks: integer
      ctr_pct: number              # clicks / impressions × 100
      avg_cpc_eur: number
      spend_eur: number
      conversions: number
      cost_per_conversion_eur: number
      conversion_rate_pct: number  # conversions / clicks × 100
      quality_score_avg: integer   # 1–10

    top_keywords:
      - keyword: string
        match_type: EXACT | PHRASE | BROAD
        impressions: integer
        clicks: integer
        ctr_pct: number
        avg_cpc_eur: number
        conversions: number
        quality_score: integer

    wasted_spend:
      pct_of_total: number         # % of spend on zero-conversion clicks
      top_irrelevant_queries:
        - query: string
          clicks: integer
          spend_eur: number

    ads:
      - headline_1: string
        headline_2: string
        headline_3: string
        description_1: string
        description_2: string
        final_url: string
        impressions: integer
        clicks: integer
        ctr_pct: number

issues:
  - severity: HIGH | MEDIUM | LOW
    description: string
    recommended_action: string

recommendations:
  - priority: integer              # 1 = highest
    action: string
    expected_impact: string
```

## Simulator instructions

When generating data for this template:
- Set `status: ACTIVE` for all newly launched campaigns
- Generate 1–3 campaigns depending on the strategy chosen
- Keep CTR between 3–8% for search campaigns (local businesses)
- Set quality score between 4–7 for new campaigns (improves over time)
- Always include 1–2 `issues` with realistic early-stage problems (e.g. missing negative keywords, low quality score)
- `wasted_spend.pct_of_total` should be 20–40% for new campaigns without refined negative keyword lists
