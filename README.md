# Moosend (moosend)

Moosend is an email marketing and automation platform with a REST API for managing mailing lists, campaigns, subscribers, automations, and tracking email performance metrics. The API uses HTTPS with API key authentication and provides programmatic access to email lists, subscriber management, campaign creation and scheduling, audience segmentation, transactional emails, and real-time reporting.

APIs.json: https://raw.githubusercontent.com/api-evangelist/moosend/refs/heads/main/apis.yml

Naftiko: https://github.com/naftiko/fleet?utm_source=api-evangelist&utm_medium=readme&utm_campaign=moosend-api-evangelist&utm_content=repo

## Tags

- Email Marketing
- Marketing Automation
- Campaigns
- Mailing Lists
- Subscribers
- Transactional Email
- SMTP
- Segmentation
- Analytics

## APIs

**Moosend API**
The Moosend REST API (v3) enables programmatic access to email marketing and automation capabilities including managing email lists, subscribers, campaigns, segments, and transactional emails. Authentication is via API key passed as a query string parameter.

- Base URL: https://api.moosend.com/v3
- Documentation: https://docs.moosend.com/developers/api-documentation/en/index-en.html
- API Reference (Apiary): https://moosendapp.docs.apiary.io/
- GitHub Org (SDKs): https://github.com/moosend

API resources include:

- Email Lists — create, retrieve, update, delete lists and custom data fields
- Subscribers — add up to 1,000 subscribers, manage custom data, retrieve by ID or email, unsubscribe
- Campaigns — create, clone, test, schedule, and send campaigns; retrieve performance metrics
- Segments — create, update, delete, and retrieve audience segments
- Transactional Campaigns — send real-time automated transactional emails

## Plans / Rate Limits / FinOps

- Plans and Pricing: [plans/moosend-plans-pricing.yml](plans/moosend-plans-pricing.yml)
- Rate Limits: [rate-limits/moosend-rate-limits.yml](rate-limits/moosend-rate-limits.yml)
- FinOps: [finops/moosend-finops.yml](finops/moosend-finops.yml)

Moosend pricing is contact-based. The Pro plan starts at $9/month for 500 contacts and scales up to $5,840/month for 1,000,000 contacts. Annual billing saves 20%, biannual billing saves 15%. Email credits (pay-as-you-go) are available and never expire. Rate limits are enforced per API key per endpoint, ranging from 2 requests per 60 seconds (campaign stats) to 500 requests for transactional campaign management. HTTP 429 is returned when limits are exceeded.

## Timestamps

- Created: 2026-06-13
- Modified: 2026-06-13

## Common

| Type | URL |
|------|-----|
| Website | https://moosend.com/ |
| Documentation | https://docs.moosend.com/developers/api-documentation/en/index-en.html |
| GitHub Org | https://github.com/moosend |
| LinkedIn | https://www.linkedin.com/company/moosend |
| Blog | https://moosend.com/blog/ |
| Pricing | https://moosend.com/pricing/ |
| Status Page | https://status.moosend.com/ |
| X | https://x.com/moosend |

## Maintainers

- **Kin Lane** — kin@apievangelist.com
