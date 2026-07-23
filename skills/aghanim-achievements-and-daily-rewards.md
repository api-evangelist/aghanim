---
name: Configure achievements and daily rewards
description: Create achievements, unlock them for players, and configure daily rewards in the Aghanim game hub.
api: openapi/aghanim-openapi-original.json
operations: [get_achievements, create_achievement, unlock_achievement, get_daily_rewards, create_daily_reward]
---

# Configure achievements and daily rewards

Engagement mechanics for an Aghanim game hub via the Server-to-Server API (`https://api.aghanim.com/s2s`).

## Auth
`Authorization: Bearer <SDK/API key>` (sandbox key while testing).

## Steps
1. **Review achievements** — `get_achievements` (`GET /v1/achievements`).
2. **Create an achievement** — `create_achievement` (`POST /v1/achievements`). Give it a stable `achievement_type` you can reference from the game server.
3. **Unlock for a player** — `unlock_achievement` (`POST /v1/achievements/{achievement_type}/unlock`) when the player hits the milestone.
4. **Review daily rewards** — `get_daily_rewards` (`GET /v1/daily_rewards`).
5. **Create a daily reward ladder** — `create_daily_reward` (`POST /v1/daily_rewards`). Attach a `bonus_item_id` for grant-on-claim.

## Conventions & errors
- List endpoints paginate with `limit`/`offset`.
- Validation errors return `422` (`errors/aghanim-problem-types.yml`).
- Item grants surface to the game server as `item.add` webhook events (`asyncapi/aghanim-webhooks.yml`).
