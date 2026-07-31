---
name: Register an account and Orderly Key
description: Onboard a wallet to Orderly by registering an account and an ed25519 Orderly Key for API access.
api: openapi/orderly-network-openapi-original.yml
operations:
  - GET /v1/registration_nonce
  - POST /v1/register_account
  - POST /v1/orderly_key
  - GET /v1/client/key_info
generated: '2026-07-20'
method: generated
---

# Register an account and Orderly Key

Bootstrap API access for a wallet before any private trading calls.

## Steps
1. **Get a registration nonce** — `GET /v1/registration_nonce` (public).
2. **Register the account** — `POST /v1/register_account` with the wallet's EIP-712 signature over the registration payload (including the nonce), broker id, and chain id. This yields the `account_id`.
3. **Add an Orderly Key** — generate an ed25519 keypair, then `POST /v1/orderly_key` with an EIP-712 wallet signature authorizing the public key (`ed25519:<base58>`), its scope, and expiration. This key signs all subsequent private REST/WebSocket requests.
4. **Verify** — `GET /v1/client/key_info` to confirm the key is registered and active.

## Notes
- Optionally restrict a key to specific IPs with `POST /v1/client/set_orderly_key_ip_restriction`.
- Full signing scheme: `authentication/orderly-network-authentication.yml`. Onboarding flow: https://orderly.network/docs/build-on-omnichain/user-flows/wallet-authentication
