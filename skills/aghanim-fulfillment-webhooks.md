---
name: Fulfill purchases via webhooks
description: Register Aghanim webhooks and verify signatures to fulfill purchases (item.add / item.remove / order.paid) on your game server.
api: openapi/aghanim-openapi-original.json
operations: [create_webhook, get_webhooks, expire_webhook_secret]
---

# Fulfill purchases via webhooks

Aghanim drives fulfillment through server-to-server webhooks. Your game server implements the handlers; Aghanim calls them.

## Auth
Management calls use `Authorization: Bearer <SDK/API key>`. Inbound webhooks are authenticated by HMAC signature, not the API key.

## Steps
1. **Register the endpoint** — `create_webhook` (`POST /v1/webhooks`) with your HTTPS URL. Copy the generated secret.
2. **List / confirm** — `get_webhooks` (`GET /v1/webhooks`).
3. **Implement handlers** for the core fulfillment events (`asyncapi/aghanim-webhooks.yml`):
   - `player.verify` — confirm the player exists / is eligible.
   - `item.add` — grant the purchased item(s) to the player.
   - `item.remove` — revoke on refund/chargeback.
   - `order.paid` / `order.refunded` — reconcile order state.
4. **Verify every request** — compute `HMAC-SHA256` over the raw payload and compare to `X-Aghanim-Signature`; reject stale requests using `X-Aghanim-Signature-Timestamp`.
5. **Dedupe** — treat `idempotency_key` as the unique event id; ignore replays.
6. **Rotate secrets** — `expire_webhook_secret` (`POST /v1/webhooks/{webhook_id}/expire_secret`) when rotating.

## Notes
- Payloads carry a `sandbox` boolean — branch test vs live logic on it.
- Payload envelope: `event_id, game_id, event_type, event_time, event_data, idempotency_key, request_id, sandbox, trigger, transaction_id, context`.
