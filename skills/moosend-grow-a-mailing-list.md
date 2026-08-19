---
name: Grow a Moosend mailing list
description: Create a mailing list with custom fields, then add or upsert subscribers into it — single and in bulk — while respecting Moosend's per-key rate limits and body-carried error envelope.
api: openapi/moosend-openapi.yml
operations:
  - creatingAMailingList
  - creatingACustomField
  - gettingAllActiveMailingLists
  - gettingMailingListDetails
  - addingSubscribers
  - addingMultipleSubscribers
  - gettingSubscribers
---

# Grow a Moosend mailing list

Base URL `https://api.moosend.com/v3`. Every path ends in `.{Format}` — use `.json`.
Every call needs `?apikey=<KEY>`; there is no header alternative.

## Before you start: three rules that break naive clients

1. **The HTTP status is always 200.** Success and failure both return 200. Read
   `Code` in the body — `0` means success, anything else is a failure. `429` in
   `Code` with `Error: "RATE-LIMITING"` is the rate-limit response.
2. **There is no idempotency key.** If a call times out you cannot safely retry a
   create. Re-read with `gettingAllActiveMailingLists` before retrying
   `creatingAMailingList`.
3. **Set `Accept: application/json`** as well as using the `.json` extension.

## Steps

### 1. Find or create the list

Call `gettingAllActiveMailingLists` (`GET /lists.json?apikey=KEY`) and check
`Context.MailingLists[]` for one whose `Name` matches. If none matches, call
`creatingAMailingList` (`POST /lists/create.json?apikey=KEY`) with `Name` (required)
and optionally `ConfirmationPage` and `RedirectAfterUnsubscribePage`.

`Context` on the create response is the new list's UUID. Hold it as `MailingListID`.

### 2. Define custom fields before importing

Custom-field values are supplied by **name**, so the field must exist first. For
each one call `creatingACustomField`
(`POST /lists/{MailingListID}/customfields/create.json?apikey=KEY`) with `Name`,
and `IsRequired` / `Type` where the field is not a plain string.

Confirm with `gettingMailingListDetails`
(`GET /lists/{MailingListID}/details.json?apikey=KEY`) and read
`Context.CustomFieldsDefinition`.

### 3. Add subscribers

For one person, `addingSubscribers`
(`POST /subscribers/{MailingListID}/subscribe.json?apikey=KEY`) with `Email`,
optional `Name` and a `CustomFields` array. This is an **upsert** — an existing
address in the list is updated rather than rejected.

*Rate limit: 10 calls / 10 seconds per API key.*

For a batch, `addingMultipleSubscribers`
(`POST /subscribers/{MailingListID}/subscribe_many.json?apikey=KEY`) with a
`Subscribers` array.

*Rate limit: 2 calls / 10 seconds per API key.* At two calls per ten seconds this
is the throughput ceiling for any import — size the batches, do not parallelise
the calls.

### 4. Read the partial-failure signal

`addingMultipleSubscribers` **silently skips invalid addresses** and still returns
`Code: 0`. The skipped addresses appear as prose in `Error`, e.g.

```
"Error": "2 items with invalid email address were ignored: email@email, email2@email"
```

Always inspect `Error` even when `Code` is `0`, and reconcile the intended count
against the list totals rather than trusting the response.

### 5. Verify

Call `gettingSubscribers`
(`GET /lists/{MailingListID}/subscribers/Subscribed.json?apikey=KEY&Page=1&PageSize=100`)
and page with `Paging.TotalPageCount`, or read `ActiveMemberCount` from
`gettingMailingListDetails`.

## Backoff

No `Retry-After` or `RateLimit-*` header is returned. On `Code: 429` sleep for the
full window of the endpoint you hit — 10 seconds for the subscriber operations,
60 seconds for campaign statistics — then retry once. See
`rate-limits/moosend-rate-limits.yml`.
