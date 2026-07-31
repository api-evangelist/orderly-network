---
name: Read market data and positions
description: Fetch public market data and account positions/holdings from Orderly.
api: openapi/orderly-network-openapi-original.yml
operations:
  - GET /v1/public/futures
  - GET /v1/orderbook/{symbol}
  - GET /v1/public/market_trades
  - GET /v1/positions
  - GET /v1/client/holding
generated: '2026-07-20'
method: generated
---

# Read market data and positions

## Public market data (no auth)
1. **List markets** — `GET /v1/public/futures` for all `PERP_<TOKEN>_USDC` symbols with prices, funding, and open interest.
2. **Orderbook snapshot** — `GET /v1/orderbook/{symbol}` (note: `{symbol}` path form).
3. **Recent trades** — `GET /v1/public/market_trades?symbol=PERP_ETH_USDC`.

For AI-agent/analytics workloads Orderly also offers the zero-auth Public Info API (`POST /v1/public/query` with a `type` field) — market summary, orderbook, candles, account state, and platform positions in one endpoint. See https://orderly.network/docs/build-on-omnichain/public-info-api/overview

## Account data (signed)
4. **Positions** — `GET /v1/positions` (all) or `GET /v1/position/{symbol}`.
5. **Holdings/collateral** — `GET /v1/client/holding`.

All account calls require the ed25519 signing headers (`authentication/orderly-network-authentication.yml`). Query requests use `application/x-www-form-urlencoded`.
