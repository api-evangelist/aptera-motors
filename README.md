# Aptera Motors

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
