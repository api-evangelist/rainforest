---
name: Void or refund a payin with Rainforest
description: Fully void a processing payin, or fully/partially refund a succeeded payin, and void a refund if eligible.
api: openapi/rainforest-payments-openapi.yaml
operations: [get_payin, void_or_refund_payin, get_refund, void_refund]
---

# Void or refund a payin

1. **Check state** — `GET /v1/payins/{payin_id}` (`get_payin`). Inspect `status`, `refundable_amount`, and `non_refundable_reason_code`.
2. **Void or refund** — `POST /v1/payins/{payin_id}/void_or_refund` (`void_or_refund_payin`):
   - status `Processing` → the **full** amount can be voided (no partial voids).
   - status `Succeeded` → full or partial **refund** up to `refundable_amount`.
3. **Track the refund** — `GET /v1/refunds/{refund_id}` (`get_refund`) or subscribe to refund webhooks.
4. **Void a refund (if eligible)** — `POST /v1/refunds/{refund_id}/void` (`void_refund`); check `non_voidable_reason_code` first.

Idempotency and metadata: use `update_refund_metadata` (API key only) to attach up to 8 KB of key-value metadata.
