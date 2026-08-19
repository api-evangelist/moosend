---
name: Report on Moosend campaign performance
description: Pull a campaign's summary, activity by type, link clicks and geography — working within the two-calls-per-minute statistics rate limit.
api: openapi/moosend-openapi.yml
operations:
  - getAllCampaigns
  - getCampaignsByPageAndPagesize
  - gettingCampaignDetails
  - campaignSummary
  - getCampaignStatistics
  - linkActivity
  - activityByLocation
  - aBTestCampaignSummary
---

# Report on Moosend campaign performance

Base URL `https://api.moosend.com/v3`, `?apikey=<KEY>`, `.json` on every path.

## The constraint that shapes this whole workflow

`getCampaignStatistics` is limited to **2 calls per 60 seconds per API key** —
the tightest limit in the API. A per-recipient report across several activity
types will take minutes. Plan for it: fetch the summary first, and only drill
into detailed activity for the campaigns that warrant it.

## Steps

### 1. Find the campaign

`getAllCampaigns` (`GET /campaigns.json?apikey=KEY`), or page with
`getCampaignsByPageAndPagesize`
(`GET /campaigns/{Page}/{PageSize}.json?apikey=KEY&SortBy=...&SortMethod=...`).
`gettingCampaignDetails` (`GET /campaigns/{CampaignID}/view.json?apikey=KEY`)
confirms `Status` and `DeliveredOn` — statistics are only meaningful once the
campaign has been delivered.

### 2. Start with the summary — it is not rate limited like the detail

`campaignSummary` (`GET /campaigns/{CampaignID}/view_summary.json?apikey=KEY`)
returns the aggregate counts: sent, opens, unique opens, clicks, unique clicks,
bounces, unsubscribes, forwards. For most reporting this is the whole answer and
costs one call.

For a split test, `aBTestCampaignSummary`
(`GET /campaigns/{CampaignID}/view_ab_summary.json?apikey=KEY`) gives the per-variant
breakdown and the winner.

### 3. Drill into recipient-level activity only when you need names

`getCampaignStatistics` (`GET /campaigns/{CampaignID}/stats/{Type}.json?apikey=KEY`)
where `{Type}` is the activity class — `sent`, `opened`, `unique_opens`,
`linkclicked`, `unique_linkclicked`, `bounced`, `unsubscribed`, `forwards`.

Page and date-filter with `Page`, `PageSize`, `From` and `To`.

**Budget it:** 2 calls / 60 seconds. Each `{Type}` is a separate call, and each
page is a separate call. Serialise them and sleep 30 seconds between calls; do not
run them concurrently.

### 4. Link and geography breakdowns

`linkActivity` (`GET /campaigns/{CampaignID}/stats/links.json?apikey=KEY`) —
clicks per URL in the campaign.

`activityByLocation` (`GET /campaigns/{CampaignID}/stats/countries.json?apikey=KEY`) —
opens and clicks by country.

## Error handling and pacing

Every response is HTTP 200; read `Code` in the body. `Code: 429` with
`Error: "RATE-LIMITING"` means the statistics budget is spent. There is **no**
`Retry-After` or `RateLimit-Remaining` header, so you cannot see how much budget
is left — track your own call timestamps and sleep the full 60-second window
before retrying. See `rate-limits/moosend-rate-limits.yml`.

Statistics reads are safe to retry: they are GETs with no side effects.
