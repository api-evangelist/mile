---
name: Configure and verify order-status webhooks
description: Authenticate to the Mile Partner API, configure an order-status webhook endpoint, send a test event, and list configured webhooks to confirm registration.
api: openapi/mile-partner-api-openapi-original.json
operations:
  - post_app_apiv1_trackingapi_index
  - post_app_apiv1_trackingapi_configureorderstatuswebhook
  - post_app_apiv1_trackingapi_testwebhook
  - get_app_apiv1_trackingapi_listwebhooks
---

# Configure and verify order-status webhooks

Register an endpoint so Mile pushes order-status updates to your system.

## 1. Authenticate

Call `post_app_apiv1_trackingapi_index` — `POST /api/v1/partners/login` with `user` and
`password` — and capture the `access_token`. Pass it on every later call as the
`access_token` query parameter (or HTTP Bearer).

## 2. Configure the order-status webhook

Call `post_app_apiv1_trackingapi_configureorderstatuswebhook` —
`POST /api/v1/partners/webhook/order-status/configure` — with the receiving URL you want Mile
to POST order-status events to.

## 3. Send a test event

Call `post_app_apiv1_trackingapi_testwebhook` — `POST /api/v1/partners/webhook/test` — to
trigger a test order-status delivery and confirm your endpoint is reachable and returns 2xx.

## 4. Confirm registration

Call `get_app_apiv1_trackingapi_listwebhooks` — `GET /api/v1/partners/webhook/list` — and
verify your endpoint appears in the list of webhooks configured for the company. You can also
call `get_app_apiv1_trackingapi_getwebhookinfo` (`/api/v1/partners/webhook/info`) for the
current configuration.

## Rules

- Related webhook topics have their own configure/test operations: order-creation
  (`post_app_apiv1_trackingapi_configureordercreationwebhook`), settlement
  (`post_app_apiv1_trackingapi_configuresettlementwebhook` /
  `post_app_apiv1_trackingapi_testsettlementwebhook`), and inventory-transfer / loadout
  (`post_app_apiv1_trackingapi_configurewebhook` /
  `post_app_apiv1_trackingapi_testloadoutwebhook`). See `asyncapi/mile-webhooks.yml`.
- Payload schemas are not published; treat delivered fields defensively.
