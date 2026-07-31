---
name: Create a customer, place an order, and track it
description: Authenticate to the Mile Partner API, create or update a customer, create an order for that customer, then track the order by tracking number.
api: openapi/mile-partner-api-openapi-original.json
operations:
  - post_app_apiv1_trackingapi_index
  - post_app_apiv1_trackingapi_createcustomer
  - post_app_apiv1_trackingapi_createorderwithoutmerchant
  - get_app_apiv1_trackingapi_trackorder
---

# Create a customer, place an order, and track it

Use the Mile Partner API (base URL `https://lastmile.milenow.com/index.php`, paths under
`/api/v1/partners/`). All calls are token-authenticated.

## 1. Authenticate

Call `post_app_apiv1_trackingapi_index` — `POST /api/v1/partners/login` with `user` and
`password`. Capture the returned `access_token`. Supply it on every later call as the
`access_token` query parameter (or as an HTTP Bearer token).

## 2. Create or update the customer

Call `post_app_apiv1_trackingapi_createcustomer` — `POST /api/v1/partners/create/customer`.
Pass `is_update=0` to create a new customer (or `is_update=1` to update an existing one).
Keep the returned customer identifier.

## 3. Create the order

Call `post_app_apiv1_trackingapi_createorderwithoutmerchant` —
`POST /api/v1/partners/order/create` — to place the order for that customer (merchant id is
optional on this operation). Record the tracking number returned for the order.

## 4. Track the order

Call `get_app_apiv1_trackingapi_trackorder` — `GET /api/v1/partners/track/order` with
`tracking_no` — to retrieve the current order status.

## Rules

- Idempotency is NOT supported by this API (no idempotency key). Do not blindly retry a
  create call on a network error — first check whether the order exists.
- The published spec documents 200 responses only; treat any non-200 as an error and surface
  the raw response body, since no structured error envelope is defined.
- See `authentication/mile-authentication.yml` for the auth model and
  `conventions/mile-conventions.yml` for cross-cutting conventions.
