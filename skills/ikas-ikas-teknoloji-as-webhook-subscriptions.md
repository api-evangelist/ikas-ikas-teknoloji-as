---
name: Manage webhook subscriptions
description: Register, list, and delete ikas store event webhooks via the GraphQL Admin API.
api: https://api.myikas.com/api/v1/admin/graphql
operations: [listWebhook, saveWebhook, deleteWebhook]
scopes: []
---

# Manage webhook subscriptions

Use the ikas Admin GraphQL API to subscribe to store events.

## Authenticate
Get an OAuth 2.0 access token from
`https://{store_name}.myikas.com/api/admin/oauth/token` and send
`Authorization: Bearer <access_token>` to
`https://api.myikas.com/api/v1/admin/graphql`.

## Steps
1. **Register** a subscription with the `saveWebhook` mutation
   (`saveWebhook(input: WebhookInput!): [Webhook!]`). Provide:
   - `scopes`: event scope strings of the form `store/<resource>/<event>`
     (e.g. `store/customer/created`, `store/customer/updated`).
   - `endpoint`: the HTTPS URL that webhooks are pushed to.
2. **List** current subscriptions with the `listWebhook` query.
3. **Remove** a subscription with the `deleteWebhook` mutation.

## Rules
- Your endpoint must return HTTP 200. If it is unreachable or returns any other
  status, ikas retries the delivery 3 times, then stops sending it.
- Responses are HTTP 200 even on error — inspect the GraphQL `errors` array.
