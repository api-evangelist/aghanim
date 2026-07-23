---
name: Run coupons and store promotions
description: Create coupons and store promotions to drive conversions in the Aghanim game hub.
api: openapi/aghanim-openapi-original.json
operations: [get_coupons, create_coupon, get_store_promotions, create_store_promotion]
---

# Run coupons and store promotions

LiveOps monetization levers via the Aghanim Server-to-Server API (`https://api.aghanim.com/s2s`).

## Auth
`Authorization: Bearer <SDK/API key>` (sandbox key while testing).

## Steps
1. **Review coupons** — `get_coupons` (`GET /v1/coupons`).
2. **Create a coupon** — `create_coupon` (`POST /v1/coupons`). Redemption surfaces as a `coupon.redeemed` webhook event.
3. **Review store promotions** — `get_store_promotions` (`GET /v1/store_promotions`).
4. **Create a store promotion** — `create_store_promotion` (`POST /v1/store_promotions`) scoped to a `store_id`; the resulting `applied_store_promotion_id` appears on affected store items.

## Conventions & errors
- Pagination `limit`/`offset`; bulk variants exist (`bulk_update_coupon`, `bulk_update_store_promotions`).
- Validation errors return `422` (`errors/aghanim-problem-types.yml`).
- Segment promotions to cohorts using the Segmentation resource.
