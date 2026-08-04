---
name: Export Cybrary completions as xAPI statements
description: Authenticate against Cybrary with OAuth 2.0 client credentials, discover which daily completion exports exist, and pull the xAPI statements for a date or the latest day for ingestion into an LMS, HRIS or reporting warehouse.
api: openapi/cybrary-completions-export-openapi.yml
operations:
  - listCompletionExports
  - getLatestCompletionExport
  - getCompletionExportByDate
---

# Export Cybrary completions as xAPI statements

Use this when you need Cybrary for Teams completion events — Course, Lab, Assessment
or Career Path completions — pushed into another system.

## Before you start

- Credentials are **issued by Cybrary**, not self-service. If you do not have a client
  id and secret, the customer's Cybrary Customer Success Manager issues them. Do not
  attempt to register an application.
- Exports contain learner **names and email addresses**. Treat every response as PII.

## 1. Get an access token

`POST https://app.cybrary.it/auth/oauth/token`

- Grant: `client_credentials`
- Scope: `use-integrations`
- Client authentication: HTTP **Basic** `Authorization` header (client id / secret),
  not form-body credentials.

A malformed request returns the RFC 6749 shape:
`{"error":"invalid_request","error_description":"...","hint":"Check the `client_id` parameter"}`.
Read `hint` first — it names the offending parameter.

## 2. Discover what exists — `listCompletionExports`

`GET /courses/api/integrations/completions`

Returns an array of `{ "date": "01_25_2020", "url": "..." }`. Note the date format is
`MM_DD_YYYY` in the list, while the generated filename uses `DD_MM_YYYY`
(`xapi_completion_export_DD_MM_YYYY.json`). Do not assume they match — always take the
`date` value from this response rather than formatting one yourself.

**Always call this before requesting a date.** There is no published retention window,
so the earliest available export is only knowable from this list.

## 3. Pull the data

- Incremental daily sync → `getLatestCompletionExport`
  (`GET /courses/api/integrations/completions/latest`)
- Backfill or re-run a specific day → `getCompletionExportByDate`
  (`GET /courses/api/integrations/completions/{date}`), using a `date` returned in step 2.

One export covers one **UTC** day. An export **may include completions for past dates**,
so downstream ingestion must be idempotent on your side: deduplicate on the tuple
`(actor.account.name, object.id, timestamp)`. The API itself offers no idempotency key
and no cursor.

## 4. Read the payload

Each item is an ADL xAPI statement:

- `actor.account.name` — the Cybrary user id (the stable join key)
- `actor.mbox` — learner email as a `mailto:` IRI
- `verb.id` — currently always `http://adlnet.gov/expapi/verbs/completed`; do not
  branch on it today, but do not hard-code it away either
- `object.id` — URL of the activity page; `object.definition.type` is the ADL activity
  type (course, lab, assessment)
- `object.definition.extensions` — Cybrary values you will want:
  `https://www.cybrary.it/contentDescriptionId` (Cybrary activity id),
  `.../continuingEducationUnits`, `.../learningHours`

## 5. Handle failures

- **`500` with `{"message": "Server Error"}` is the response to an unauthenticated
  request.** This API does not return `401` when the token is missing. Re-check the
  token before treating a 500 as an outage.
- `404` on `{date}` means no export exists for that day — re-run step 2.
- Responses carry `X-RateLimit-Limit` / `X-RateLimit-Remaining` but **no**
  `X-RateLimit-Reset` and no `Retry-After`. Back off on a fixed schedule; you cannot
  compute a reset time from the response.
- There is no status page (`status.cybrary.it` returns 502), so failures cannot be
  cross-checked against a provider incident feed.

## Cross-references

- Auth profile: `authentication/cybrary-authentication.yml`
- Scopes: `scopes/cybrary-scopes.yml`
- Errors: `errors/cybrary-problem-types.yml`
- Conventions: `conventions/cybrary-conventions.yml`
- Rate limits: `rate-limits/cybrary-rate-limits.yml`
- Data model: `data-model/cybrary-data-model.yml`
