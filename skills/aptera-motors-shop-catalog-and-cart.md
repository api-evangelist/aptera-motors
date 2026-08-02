---
name: Search the Aptera store and build a cart
description: Find Aptera Motors merchandise, inspect a product variant, and assemble a cart against the anonymous MCP server on the Aptera storefront — stopping short of checkout, which requires human approval.
api: mcp/aptera-motors-mcp.yml
surface: https://shop.aptera.us/api/mcp
generated: '2026-08-02'
method: generated
source: tools/list on https://shop.aptera.us/api/mcp (verbatim capture at mcp/aptera-motors-shop-mcp-tools.json)
operations:
  - search_catalog
  - get_product_details
  - update_cart
  - get_cart
  - search_shop_policies_and_faqs
---

# Search the Aptera store and build a cart

Aptera Motors (Nasdaq: SEV) sells branded merchandise from a Shopify storefront at
`https://shop.aptera.us`. That storefront exposes a Model Context Protocol server at
`https://shop.aptera.us/api/mcp` that answers **anonymously** — no token, no API key.
This skill covers catalog search through cart assembly. It deliberately stops before
checkout.

> Aptera publishes no vehicle API. Nothing in this skill reaches vehicle data,
> reservations, or the `aptera.us` corporate site.

## Transport

`POST https://shop.aptera.us/api/mcp`

```
Content-Type: application/json
Accept: application/json, text/event-stream
```

JSON-RPC 2.0. `initialize` reports `protocolVersion: 2025-06-18` and
`serverInfo: {name: storefront-renderer, version: 0.1.0}`. `resources/list` and
`prompts/list` both return empty arrays — tools are the only surface.

## Steps

### 1. Find products — `search_catalog`

Takes a `catalog` object; `catalog.query` is the free-text query. Pass
`catalog.context` (`address_country`, `address_region`, `postal_code`, `currency`)
so pricing and availability localize correctly. At least one of query or filters
must be provided.

Results are paginated. Read `pagination.cursor` off the response and pass it back
only when the user asks for more results — do not pre-fetch pages.

The response conforms to the UCP catalog search capability
`dev.ucp.shopping.catalog.search`.

### 2. Inspect a variant — `get_product_details`

Required: `product_id`. Optional: `options`, `country`, `language`.

Without `options` the first available variant is returned. Pass `options` to pin the
exact variant (size, colour) the buyer chose — do this before adding to a cart, or
you will add the wrong variant.

### 3. Build the cart — `update_cart`

Every cart mutation goes through this one tool: `add_items`, `update_items`,
`remove_line_ids`, `buyer_identity`, `delivery_addresses_to_add`,
`delivery_addresses_to_replace`, `selected_delivery_options`, `discount_codes`,
`gift_card_codes`, `note`. Omit `cart_id` to create a cart; pass it to mutate one.

There is **no documented idempotency key** on this surface. Do not blind-retry a
successful-but-unacknowledged `update_cart` — re-read the cart with `get_cart` and
reconcile before retrying, or you will duplicate line items.

### 4. Read the cart — `get_cart`

Required: `cart_id`. Returns line items, shipping options, discount info and the
checkout URL.

### 5. Answer policy questions — `search_shop_policies_and_faqs`

Required: `query`. Use it for shipping, returns, refunds and store-service questions
instead of guessing or scraping the policy pages.

## Stop here: checkout requires a human

Aptera's `robots.txt` and `/agents.md` both state the rule plainly: checkouts are for
humans. Do not complete checkout, payment, or order placement automatically — no
scripted form fills, no browser automation, no end-to-end flow that finalizes payment
without an explicit, contemporaneous human approval step.

Hand the buyer the `checkout url` from `get_cart` and let them finish, or route the
purchase through the Shopify shopping skill (`https://shop.app/SKILL.md`), which
enforces buyer approval before payment.

## Errors and limits

- Errors come back as JSON-RPC 2.0 error objects.
- The endpoint is rate-limited per IP. Back off on `429`.
- `https://shop.aptera.us/api/ucp/mcp` — the versioned UCP endpoint carrying the
  checkout capabilities — returns `422` to a plain `tools/list` and requires UCP
  version negotiation (`2026-04-08` or `2026-01-23`). See
  `../well-known/aptera-motors-shop-ucp.json`.

## Related

- `../mcp/aptera-motors-mcp.yml` — server manifest and verified tool list
- `../conventions/aptera-motors-conventions.yml` — pagination, rate limits, human-in-the-loop
- `../authentication/aptera-motors-authentication.yml` — the customer-account OAuth/OIDC profile
- `aptera-motors-shop-agents.md` — the provider's own agent instructions, verbatim
