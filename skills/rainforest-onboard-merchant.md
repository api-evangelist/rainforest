---
name: Onboard a merchant with Rainforest
description: Create a merchant, complete and submit its application, and get it to ACTIVE so it can process payments.
api: openapi/rainforest-merchants-openapi.yaml
operations: [create_merchant, get_merchant_application, update_merchant_application, submit_merchant_application, verify_merchant_application, simulate_merchant_application, get_merchant]
---

# Onboard a merchant

Use a server-side API key: `Authorization: Bearer {apikey}`. Sandbox keys are prefixed `sbx_apikey_`.

1. **Create the merchant** — `POST /v1/merchants` (`create_merchant`). A pending merchant application is created for it.
2. **Read the application** — `GET /v1/merchants/{merchant_id}/applications/{merchant_application_id}` (`get_merchant_application`) to see required fields and status.
3. **Fill it in** — `PATCH /v1/merchants/{merchant_id}/applications/{merchant_application_id}` (`update_merchant_application`). Only supplied fields are updated; set a field to `null` to remove it.
4. **Submit** — `POST .../submit` (`submit_merchant_application`). Rainforest runs KYC/KYB.
5. **(Sandbox) drive the outcome** — `POST .../simulate` (`simulate_merchant_application`) to move the application to APPROVED/DECLINED and fire merchant-application webhooks.
6. **Confirm ACTIVE** — poll `GET /v1/merchants/{merchant_id}` (`get_merchant`) or subscribe to merchant webhooks. A merchant must be `ACTIVE` before any payin, payment method, or device call.

Errors return `{ status: "ERROR", data: null, errors: [{ field, code, message }] }` — not RFC 9457.
