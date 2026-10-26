# Aptera Motors

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

Aptera Motors Corp. (Nasdaq: SEV) is a Carlsbad, California solar electric vehicle manufacturer founded by Steve Fambro and Chris Anthony, building a three-wheeled, two-seat enclosed autocycle covered in roughly 700 watts of integrated solar cells.

- Website — https://aptera.us/
- Discovery Center (FAQ) — https://aptera.us/discovery-center/
- Reserve / configurator — https://aptera.us/reserve/
- Updates — https://aptera.us/updates/
- Investor relations — https://ir.aptera.us/
- Store — https://shop.aptera.us/
- Secondary market — https://forgeglobal.com/aptera-motors_stock/

## API posture

Aptera Motors publishes **no developer program**: no developer portal, no API
documentation, no OpenAPI/AsyncAPI definition, no SDKs, no status page, no
`security.txt`, and no A2A agent card. `api.aptera.us`, `developer.aptera.us` and
`docs.aptera.us` do not resolve.

What the enrichment pass did find is a real, live, **anonymous agent surface on the
merchandise storefront** — Shopify platform infrastructure served from Aptera's own
hostname:

- **MCP server** — `https://shop.aptera.us/api/mcp`, protocol `2025-06-18`, five
  tools verified via `tools/list` (`search_catalog`, `get_product_details`,
  `get_cart`, `update_cart`, `search_shop_policies_and_faqs`)
- **UCP merchant profile** — `/.well-known/ucp`, Universal Commerce Protocol
  `2026-04-08`, eight capabilities
- **OAuth 2.0 / OpenID Connect discovery** — `/.well-known/openid-configuration`,
  `/.well-known/oauth-authorization-server`, `/.well-known/oauth-protected-resource`
- **Agent instructions** — `/llms.txt` and `/agents.md`, both linked from `robots.txt`

A second MCP endpoint exists on the corporate WordPress site
(`/wp-json/mcp/mcp-adapter-default-server`) but is auth-gated (HTTP 401) and
undocumented; it is recorded as detected only.

See `mcp/`, `well-known/`, `authentication/`, `scopes/`, `conventions/`,
`conformance/`, `skills/`, `llms/` and `security/` for the captured artifacts.
