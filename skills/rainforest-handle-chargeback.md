---
name: Respond to a chargeback with Rainforest
description: Read a chargeback, attach evidence, and either submit a dispute response or accept the loss.
api: openapi/rainforest-payments-openapi.yaml
operations: [get_chargeback, update_chargeback_evidence, submit_chargeback, accept_chargeback, simulate_chargeback]
---

# Respond to a chargeback

Chargebacks arrive via `chargeback.*` webhooks.

1. **Read it** — `GET /v1/chargebacks/{chargeback_id}` (`get_chargeback`) for amount, reason, phase, and response deadline.
2. **Decide:**
   - **Dispute:** attach evidence with `PATCH /v1/chargebacks/{chargeback_id}/evidence/{chargeback_evidence_id}` (`update_chargeback_evidence`) — only supplied fields are updated; set a field to `null` to remove it — then `POST .../submit` (`submit_chargeback`) to respond with the evidence.
   - **Accept:** `POST /v1/chargebacks/{chargeback_id}/accept` (`accept_chargeback`) to take the loss.
3. **(Sandbox) simulate a decision** — `POST /v1/chargebacks/{chargeback_id}/simulate` (`simulate_chargeback`) to advance status and fire chargeback webhooks.

Use `update_chargeback_metadata` (API key only) for internal reference data (< 8 KB). Files (evidence documents) are attached via the File Uploads API.
