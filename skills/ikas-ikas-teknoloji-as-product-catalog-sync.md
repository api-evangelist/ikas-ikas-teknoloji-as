---
name: Sync the product catalog
description: Read and write products, variant prices, and stock levels on an ikas store via the GraphQL Admin API.
api: https://api.myikas.com/api/v1/admin/graphql
operations: [listProduct, searchProducts, saveProduct, saveVariantPrices, saveProductStockLocations]
scopes: [read_products, write_products, read_inventories, write_inventories]
---

# Sync the product catalog

Use the ikas Admin GraphQL API to keep a product catalog in sync.

## Authenticate
1. Obtain an OAuth 2.0 access token. For a private app, POST
   `grant_type=client_credentials`, `client_id`, `client_secret` (as
   `application/x-www-form-urlencoded`) to
   `https://{store_name}.myikas.com/api/admin/oauth/token`.
2. Send `Authorization: Bearer <access_token>` on every GraphQL request to
   `https://api.myikas.com/api/v1/admin/graphql`. Tokens expire after 4 hours
   (`expires_in: 14400`) — refresh before expiry.

## Steps
1. **List products** with the `listProduct` query (paginate through results) or
   `searchProducts` to find specific items. Requires `read_products`.
2. **Create or update a product** with the `saveProduct` mutation. Requires
   `write_products`.
3. **Update variant prices** with `saveVariantPrices`.
4. **Set inventory** per stock location with `saveProductStockLocations`.
   Requires `write_inventories`.

## Rules
- ikas GraphQL returns HTTP 200 even on failure — always inspect the top-level
  `errors` array, not just the status code.
- No idempotency-key header is supported; treat `saveProduct` as an upsert keyed
  by the product id you pass.
