---
name: Process and fulfill orders
description: Read orders, create orders with transactions, fulfill line items, and refund on an ikas store via the GraphQL Admin API.
api: https://api.myikas.com/api/v1/admin/graphql
operations: [listOrder, createOrderWithTransactions, fulfillOrder, refundOrderLine, updateOrderLine]
scopes: [read_orders, write_orders]
---

# Process and fulfill orders

Use the ikas Admin GraphQL API to move orders through fulfillment.

## Authenticate
Get an OAuth 2.0 access token from
`https://{store_name}.myikas.com/api/admin/oauth/token` (client_credentials for
private apps) and send `Authorization: Bearer <access_token>` to
`https://api.myikas.com/api/v1/admin/graphql`.

## Steps
1. **List orders** with the `listOrder` query, filtering by status. Requires
   `read_orders`.
2. **Create an order with payment** using `createOrderWithTransactions` when
   ingesting external orders. Requires `write_orders`.
3. **Fulfill** the order (or line items) with `fulfillOrder`.
4. **Amend a line** with `updateOrderLine`; **refund** a line with
   `refundOrderLine`.

## Rules
- Responses are HTTP 200 even on error — check the GraphQL `errors` array.
- Order status and transaction status are separate enums; confirm both before
  fulfilling.
- No idempotency-key header exists; guard against duplicate
  `createOrderWithTransactions` calls in your own code.
