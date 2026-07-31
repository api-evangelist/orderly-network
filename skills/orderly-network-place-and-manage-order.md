---
name: Place and manage a perpetual-futures order
description: Authenticate with an Orderly Key and create, inspect, and cancel a perp order.
api: openapi/orderly-network-openapi-original.yml
operations:
  - POST /v1/order
  - GET /v1/order/{order_id}
  - GET /v1/orders
  - DELETE /v1/order
generated: '2026-07-20'
method: generated
---

# Place and manage a perpetual-futures order

Use the Orderly EVM REST API to place and manage an order on a `PERP_<TOKEN>_USDC` market.

## Prerequisites
- A registered account and an Orderly Key (ed25519). See the onboarding skill.
- Every private request MUST carry the four signing headers: `orderly-account-id`, `orderly-key` (`ed25519:<base58>`), `orderly-signature`, `orderly-timestamp` (ms). Sign the normalized string `<timestamp><METHOD><path?query><json-body>`. Requests whose timestamp is >300s from server time are rejected. Details: `authentication/orderly-network-authentication.yml`.

## Steps
1. **Create the order** — `POST /v1/order` with `symbol` (e.g. `PERP_ETH_USDC`), `order_type`, `side`, `order_quantity`, and `order_price` for limit orders. Optionally supply `client_order_id` for your own reconciliation (a duplicate submission returns error `-1007` / HTTP 409). Body is `application/json`.
2. **Confirm** — read the returned `order_id`, or query `GET /v1/order/{order_id}`.
3. **List open orders** — `GET /v1/orders` (query is `application/x-www-form-urlencoded`; page-number pagination via `page`/`size`).
4. **Cancel** — `DELETE /v1/order` with `order_id` (or `client_order_id`) and `symbol`. Cancelling an already-cancelled order returns `-1006`.

## Error handling
- `-1102` order value too small, `-1103` price violation, `-1104` quantity violation, `-1101` insufficient margin. Full list: `errors/orderly-network-error-codes.yml`.
- `-1003` / HTTP 429 = rate limited; back off and retry.
