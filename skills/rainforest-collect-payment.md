---
name: Collect a payment (payin) with Rainforest
description: Configure a payin and process it, either via the embedded Component (session token) or server-to-server API.
api: openapi/rainforest-payments-openapi.yaml
operations: [create_payin_config, create_session, create_payin, get_payin, capture_payin]
---

# Collect a payment

The merchant must be `ACTIVE`.

1. **Create a payin config** — `POST /v1/payin_configs` (`create_payin_config`) with `merchant_id`, `amount`, `currency`, and an **`idempotency_key`** (makes the create safe to retry). Optionally set `allow_partial_authorization`.
2. **Choose a flow:**
   - **Component (recommended):** mint a session token with `POST /v1/sessions` (`create_session`) scoped to the payin config, hand it to the Rainforest Payment Component in the browser to collect card/ACH and process the payin. No card data touches your servers.
   - **Direct API (requires Rainforest approval / PCI scope):** `POST /v1/payins` (`create_payin`) with the payin config id and full PCI card/bank data server-to-server.
3. **Auth-then-capture (optional):** if authorized only, `POST /v1/payins/{payin_id}/capture` (`capture_payin`).
4. **Confirm** — `GET /v1/payins/{payin_id}` (`get_payin`) or subscribe to payin webhooks (`payin.processing`, etc.).

Sandbox: use approved test cards (e.g. Visa `4111 1111 1111 1111`) for PROCESSING, or decline cards like `4000 0000 0000 0002` (`DECLINED`). See `sandbox/rainforest-sandbox.yml`.
