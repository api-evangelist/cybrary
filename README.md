# Cybrary

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
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Cybrary is a cybersecurity and IT skills-development platform used by individuals and
enterprise teams for hands-on training, certification preparation and workforce
upskilling — certification prep paths, role-based career paths, skill paths, virtual
labs, assessments and security awareness training.

- Website: https://www.cybrary.it/
- Help center: https://help.cybrary.it/hc/en-us
- GitHub: https://github.com/Cybrary

## The public API surface

Cybrary publishes **one** documented API.

**Cybrary Completions Export API** — `https://app.cybrary.it/courses/api`
([docs](https://help.cybrary.it/completions-export-integration)). Three read-only
operations that return daily Course / Lab / Assessment / Career Path completion events
for a Cybrary for Teams organization as **xAPI (Experience API) statements**, for
ingestion into a customer LMS, HRIS or reporting warehouse. Auth is OAuth 2.0 client
credentials with the `use-integrations` scope; credentials are issued by Cybrary to the
customer rather than self-service.

Enterprise identity is standards-based but not a public API: **SAML 2.0** single sign-on
and **SCIM 2.0** automated provisioning are documented for Okta, OneLogin and Microsoft
Entra ID, with a per-tenant SCIM base URL and bearer token. Because that base URL is
customer-specific and unpublished, SCIM is recorded in `conformance/` and
`authentication/` rather than listed as an API.

## Not published by Cybrary

Recorded as verified absence, checked 2026-08-04:

- **No machine-readable specification.** The API is documented in prose only. The
  OpenAPI in `openapi/` was authored by API Evangelist from that prose and is stamped
  `x-provenance: published_by_provider: false`. Every path, verb, scope and payload
  field in it is transcribed from the provider's own documentation.
- **No SDKs or client libraries** in any public registry (npm, PyPI, Packagist,
  RubyGems, Maven Central, NuGet, crates.io, pkg.go.dev). The GitHub org is real but
  publishes forks and internal tooling.
- **No MCP server, no A2A agent card, no AsyncAPI, no webhooks.** `mcp/` holds an
  API Evangelist-derived *candidate* tool list only, and carries no `MCPServer` pointer.
- **No `/.well-known/` documents.** `app.cybrary.it` is a single-page app that answers
  HTTP 200 with an HTML shell for every `/.well-known/*` path; those are logged as
  false positives in `well-known/`.
- **No API versioning, deprecation policy, changelog or working status page.**
  `status.cybrary.it` resolves but returns 502; incidents are announced as a banner on
  the marketing site.
- **No published rate-limit policy.** The `X-RateLimit-*` values in `rate-limits/` were
  observed on live responses, not documented.
- **No published compliance program or trust center.**

## Published and captured

- `llms.txt` — Cybrary serves a real one at https://www.cybrary.it/llms.txt, saved
  verbatim to `llms/`. It points AI systems at learning hubs and the blog; it does not
  mention the API.
- **Vulnerability disclosure** — https://www.cybrary.it/responsible-disclosure-program
  plus a HackerOne program, with safe-harbor language and `report@cybrary.it`.
