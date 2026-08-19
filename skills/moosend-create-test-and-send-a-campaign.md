---
name: Create, test and send a Moosend campaign
description: Draft a campaign against a verified sender and a mailing list, send a test to yourself, then schedule or send it — with the irreversibility and rate-limit guardrails the Moosend v3 API does not enforce for you.
api: openapi/moosend-openapi.yml
operations:
  - gettingAllYourSenders
  - gettingSenderDetails
  - creatingADraftCampaign
  - updatingADraftCampaign
  - cloningAnExistingCampaign
  - testingACampaign
  - schedulingACampaign
  - unschedulingACampaign
  - sendingACampaign
  - gettingCampaignDetails
---

# Create, test and send a Moosend campaign

Base URL `https://api.moosend.com/v3`, `?apikey=<KEY>` on every call, `.json`
extension on every path, `Accept: application/json`.

## The one irreversible step

`sendingACampaign` delivers to a real audience and **there is no recall
operation in the API**. Treat it as human-confirmed. Everything before it is
reversible; that step is not.

## Steps

### 1. Pick a verified sender

`gettingAllYourSenders` (`GET /senders/find_all.json?apikey=KEY`) returns the
sender addresses on the account. Use one of these — an unverified address will
not send. `gettingSenderDetails`
(`GET /senders/find_one.json?apikey=KEY&Email=...`) checks a specific one.

### 2. Create the draft

`creatingADraftCampaign` (`POST /campaigns/create.json?apikey=KEY`) with the
campaign `Name`, `Subject`, sender details, the `MailingLists` to target and
optionally `Segments`. `Context` in the response is the new `CampaignID`.

To start from something that already works, use `cloningAnExistingCampaign`
(`POST /campaigns/{CampaignID}/clone.json?apikey=KEY`) instead — safer than
rebuilding content by hand.

Revise with `updatingADraftCampaign`
(`POST /campaigns/{CampaignID}/update.json?apikey=KEY`). Only drafts are
updatable.

### 3. Read it back before you touch send

`gettingCampaignDetails` (`GET /campaigns/{CampaignID}/view.json?apikey=KEY`) and
check `Status`, `Subject`, `MailingLists` and `Segments`. Confirm the audience is
the one you intended — the API will not warn you that a list is larger than you
expected.

### 4. Send a test

`testingACampaign` (`POST /campaigns/{CampaignID}/send_test.json?apikey=KEY`)
delivers to named test recipients only. Do this every time. It is the only
rehearsal the API offers.

### 5. Schedule, or send

Prefer `schedulingACampaign`
(`POST /campaigns/{CampaignID}/schedule.json?apikey=KEY`) with the delivery
datetime — it is reversible with `unschedulingACampaign`
(`POST /campaigns/{CampaignID}/unschedule.json?apikey=KEY`) right up until it
fires, which gives you an undo that `sendingACampaign` does not.

Use `sendingACampaign` (`POST /campaigns/{CampaignID}/send.json?apikey=KEY`) only
on explicit human confirmation.

## Error handling

Every response is HTTP 200. Branch on the body:

```json
{"Code": 0, "Error": null, "Context": "..."}
```

`Code: 0` succeeded. `Code: 429` with `Error: "RATE-LIMITING"` means you exceeded
the per-key budget for that endpoint. Do **not** retry a send on a timeout — there
is no idempotency key, so a retry can send twice. Re-read with
`gettingCampaignDetails` and check `Status` first.
