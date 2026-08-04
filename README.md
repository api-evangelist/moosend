# Moosend (moosend)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
