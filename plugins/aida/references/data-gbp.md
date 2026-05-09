# Data template — Google Business Profile
# Source: Google Business Profile API (future integration)
# Used by: campaign-simulator to generate realistic campaign data

## Schema

```
date_range:
  from: YYYY-MM-DD
  to: YYYY-MM-DD

profile:
  business_name: string
  place_id: string
  status: VERIFIED | UNVERIFIED | SUSPENDED
  completeness_score_pct: integer  # 0–100

  fields:
    - name: string
      status: COMPLETE | PARTIAL | MISSING
      value_preview: string        # first 80 chars if present

search_performance:
  total_views: integer
  views_by_surface:
    google_search: integer
    google_maps: integer
  views_by_search_type:
    direct: integer                # searched for business name directly
    discovery: integer             # found through category/keyword search
    branded: integer               # found through branded keyword

customer_actions:
  website_clicks: integer
  direction_requests: integer
  phone_calls: integer
  messages: integer
  bookings: integer                # if booking integration active

top_search_queries:
  - query: string
    impressions: integer

photos:
  total_count: integer
  views_period: integer
  owner_photos: integer
  customer_photos: integer
  categories_present:
    - EXTERIOR | INTERIOR | PRODUCT | TEAM | MENU | AT_WORK

posts:
  - published_at: YYYY-MM-DD
    type: WHATS_NEW | OFFER | EVENT | PRODUCT
    content_preview: string
    views: integer
    clicks: integer

reviews:
  total_count: integer
  average_rating: number           # 1.0–5.0
  rating_distribution:
    five_star: integer
    four_star: integer
    three_star: integer
    two_star: integer
    one_star: integer
  responded_count: integer
  response_rate_pct: number
  recent_reviews:
    - rating: integer
      excerpt: string
      date: YYYY-MM-DD
      responded: boolean

issues:
  - severity: HIGH | MEDIUM | LOW
    field: string
    description: string
    recommended_action: string

recommendations:
  - priority: integer
    action: string
    expected_impact: string
```

## Simulator instructions

When generating data for this template:
- Set `completeness_score_pct` to 60–75% for a newly optimised profile (improving from whatever was in business_profile.md)
- `views_by_search_type.discovery` should be the largest share (60–70%) — this is what GBP optimisation improves
- `top_search_queries` should include the business's actual service keywords from business_profile.md
- Always leave 2–3 fields as PARTIAL or MISSING so Leo has clear next actions
- `reviews.response_rate_pct`: set to 0% if no prior responses observed in business_profile.md, or carry over the existing rate
- Include 1–2 `issues` with HIGH severity (e.g. missing services list, no photos of a key category)
- Posts from Leo should show `views` proportional to profile traffic (roughly 5–10% of total profile views)
