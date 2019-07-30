# Webhooks

Crush API support following types of webhooks:

- `image#processing`
- `image#error`
- `image#optimized`
- `shop#subscription_updated`

Crush API use `POST` requests to send webhooks and treat webhook as successfully delivered if the webserver responds with status code `201 Created`.

If the webserver responds with any other HTTP status code - Crush API marks webhook as failed and try to redeliver it later. Max number of retries to deliver webhook - 5.

Crush API does not try to read response body.

## `image#processing`

> Example payload

```json
{
  "object_type": "image",
  "shop_identifier": "user@example.com",
  "compression_type": "balanced",
  "compression_level": null,
  "image_id": 111,
  "event": "processing",
  "link": "https://url.to.download/image"
}
```

## `image#error`

> Example payload

```json
{
  "object_type": "image",
  "shop_identifier": "user@example.com",
  "compression_type": "balanced",
  "compression_level": null,
  "image_id": 111,
  "event": "error",
  "error": "cant_compress",
  "quota_usage:": 1212121
}
```

## `image#optimized`

> Example payload

```json
{
  "object_type": "image",
  "shop_identifier": "user@example.com",
  "compression_type": "lossy",
  "compression_level": 65,
  "image_id": 111,
  "event": "optimized",
  "optimized_images": [
    {
      "style": "original",
      "size": 154356,
      "link": "https://url.to.download/image",
      "id": 112,
      "expire_at": "2019-07-30T11:22:56Z"
    }
  ],
  "quota_usage:": 1212121
}
```

## `shop#subscription_updated`

> Example payload

```json
{
  "object_type": "shop",
  "shop_identifier": "user@example.com",
  "event": "subscription_updated",
  "plan": "micro",
  "quota_usage": 1212121,
  "charged_at": "2019-07-30T11:22:56Z",
  "next_charge_at": "2019-08-30T11:22:56Z",
  "subscription_id": "JN677WHBDWH8762",
  "subscription_status": "active",
  "customer_id": "JN677WHBDWH8762",
}
```