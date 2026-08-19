---
name: Segment a Moosend audience
description: Build a segment on a mailing list from matching criteria, read its membership, and target a campaign at it.
api: openapi/moosend-openapi.yml
operations:
  - gettingSegments
  - creatingANewEmptySegment
  - addingCriteriaToSegments
  - updatingSegmentCriteria
  - gettingSegmentDetails
  - gettingSegmentSubscribers
  - updatingASegment
  - deletingASegment
---

# Segment a Moosend audience

Base URL `https://api.moosend.com/v3`, `?apikey=<KEY>`, `.json` on every path.

A segment belongs to **one mailing list**. It is a set of criteria evaluated at
read time, not a stored membership list — so membership can change between the
call that builds it and the campaign that uses it.

## Steps

### 1. List what already exists

`gettingSegments` (`GET /lists/{MailingListID}/segments.json?apikey=KEY`) to avoid
creating a duplicate. There is no uniqueness constraint on segment names and no
idempotency key, so a retried create leaves you with two.

### 2. Create the segment shell

`creatingANewEmptySegment`
(`POST /lists/{MailingListID}/segments/create.json?apikey=KEY`) with `Name` and
`MatchType` (all criteria must match, or any). `Context` is the new `SegmentID`.

### 3. Add criteria one at a time

`addingCriteriaToSegments`
(`POST /lists/{MailingListID}/segments/{SegmentID}/criteria/add.json?apikey=KEY`)
with the field, comparison operator and value. Repeat for each rule.

Correct a rule with `updatingSegmentCriteria`
(`POST /lists/{MailingListID}/segments/{SegmentID}/criteria/{CriteriaID}/update.json?apikey=KEY`)
rather than deleting and re-adding — that keeps the `CriteriaID` stable for
anything referencing it.

Rename or change the match type with `updatingASegment`
(`POST /lists/{MailingListID}/segments/{SegmentID}/update.json?apikey=KEY`).

### 4. Check what it actually matches — before you send to it

`gettingSegmentDetails`
(`GET /lists/{MailingListID}/segments/{SegmentID}/details.json?apikey=KEY`)
returns the definition; `gettingSegmentSubscribers`
(`GET /lists/{MailingListID}/segments/{SegmentID}/members.json?apikey=KEY`)
returns the people. Read the members and sanity-check the count against
`ActiveMemberCount` on the list. An over-broad criterion silently matches the
whole list and the API will not flag it.

### 5. Target a campaign

Pass the `SegmentID` in the `Segments` field of `creatingADraftCampaign` or
`updatingADraftCampaign` — see `skills/moosend-create-test-and-send-a-campaign.md`.

### Cleanup

`deletingASegment`
(`DELETE /lists/{MailingListID}/segments/{SegmentID}/delete.json?apikey=KEY`).
This removes the segment definition only; subscribers are untouched.

## Error handling

HTTP status is always 200 — read `Code` in the body. `0` is success; `429` with
`Error: "RATE-LIMITING"` is a rate limit. Custom-field criteria reference fields
by the name defined on the list, so confirm the field exists via
`gettingMailingListDetails` → `Context.CustomFieldsDefinition` before adding a
criterion that uses it.
