# Data template — Social Media
# Source: Meta Marketing API / Instagram Graph API (future integration)
# Used by: campaign-simulator to generate realistic campaign data

## Schema

```
date_range:
  from: YYYY-MM-DD
  to: YYYY-MM-DD

pages:
  - platform: FACEBOOK | INSTAGRAM | LINKEDIN
    page_id: string
    page_name: string
    followers_total: integer
    followers_gained_period: integer
    followers_lost_period: integer

    posts:
      - id: string
        published_at: YYYY-MM-DDTHH:MM:SS
        type: IMAGE | VIDEO | REEL | CAROUSEL | TEXT | STORY
        content_preview: string      # first 100 chars of caption
        boosted: boolean
        boost_spend_eur: number      # 0 if not boosted

        metrics:
          reach: integer             # unique accounts reached
          impressions: integer       # total views including repeats
          engagement_rate_pct: number
          reactions: integer
          comments: integer
          shares: integer
          saves: integer
          link_clicks: integer
          profile_visits: integer

    page_metrics:
      total_reach: integer
      total_impressions: integer
      avg_engagement_rate_pct: number
      total_link_clicks: integer
      industry_avg_engagement_rate_pct: number

    stories:
      count: integer
      avg_views: integer
      avg_exit_rate_pct: number      # % who swiped away early

    top_performing_post:
      content_preview: string
      reach: integer
      engagement_rate_pct: number
      type: string

    worst_performing_post:
      content_preview: string
      reach: integer
      engagement_rate_pct: number
      type: string

issues:
  - platform: string
    severity: HIGH | MEDIUM | LOW
    description: string
    recommended_action: string

recommendations:
  - priority: integer
    platform: string
    action: string
    expected_impact: string
```

## Simulator instructions

When generating data for this template:
- Include only platforms relevant to the business (local B2C → Facebook + Instagram; B2B → LinkedIn)
- For newly started accounts: followers 100–500, growing 10–30/week
- For existing accounts picked up by Mia: use existing follower base from business_profile.md, add 5–15% growth
- Reels and image posts outperform text posts — reflect this in the metrics
- Always include one strong-performing post and one weak one to give Aida real improvement points
- `industry_avg_engagement_rate_pct`: 3.5% for Facebook, 5% for Instagram, 2% for LinkedIn
- If a post was boosted, its reach should be 3–8× higher than organic
