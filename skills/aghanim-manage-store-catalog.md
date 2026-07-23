---
name: Build and manage a game store catalog
description: Create catalog items, a store, and store items in the Aghanim game hub via the Server-to-Server API.
api: openapi/aghanim-openapi-original.json
operations: [get_items, create_item, get_stores, create_store, create_store_item, get_store_items]
---

# Build and manage a game store catalog

Use the Aghanim Server-to-Server API (`https://api.aghanim.com/s2s`) to stand up a sellable catalog for a game hub.

## Auth
Send `Authorization: Bearer <SDK/API key>`. Use the **sandbox** key while testing (Dashboard → Integration → API keys); sandbox and live keys are distinct.

## Steps
1. **Inventory existing items** — `get_items` (`GET /v1/items`). Supports `limit`/`offset` and `sort_field`/`sort_order`.
2. **Create catalog items** — `create_item` (`POST /v1/items`). Attach a `price_template_id`; set `category_id` to file it under an item category.
3. **List / create the store** — `get_stores` (`GET /v1/stores`); if none fits, `create_store` (`POST /v1/stores`).
4. **Place items in the store** — `create_store_item` (`POST /v1/store_items/{store_id}`). Reference the `item_id` and optionally `bonus_item_id` / `fallback_item_id`.
5. **Verify** — `get_store_items` to confirm the store renders the intended offer set.

## Conventions & errors
- Pagination is `limit`/`offset`; sorting via `sort_field`/`sort_order`/`order_by` (see `conventions/aghanim-conventions.yml`).
- Validation failures return `422` with `{ "detail": [ { "loc", "msg", "type" } ] }` (see `errors/aghanim-problem-types.yml`).
- Test end-to-end in sandbox before switching the Dashboard toggle to live.
