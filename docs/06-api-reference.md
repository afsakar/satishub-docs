# 06 API Reference

## License API

Base routes are registered in `satis/routes/api.php`.

- `POST /licenses/activate`
- `POST /licenses/validate`

Rate limiter: `license-api`.

### Example: Activate License

Request:

```http
POST /licenses/activate
Content-Type: application/json

{
  "license_key": "LIC-ABCD-1234",
  "domain": "customer.example",
  "app_name": "Customer App",
  "package_name": "vendor/private-package",
  "ip": "203.0.113.10"
}
```

### Example: Validate License

Request:

```http
POST /licenses/validate
Content-Type: application/json

{
  "license_key": "LIC-ABCD-1234",
  "domain": "customer.example",
  "ip": "203.0.113.10"
}
```

## Package Documentation API

Routes:

- `GET /api/packages/{package}/docs`
- `GET /api/packages/{package}/docs/content?path=...`
- `GET /api/packages/{package}/docs/bundle?limit=...`

Rate limiter: `docs-api`.

## Billing Webhook API

Route:

- `POST /billing/webhook/{provider}`

Path is configurable with `BILLING_WEBHOOK_PATH`.

### Stripe Webhook Example

```http
POST /billing/webhook/stripe
stripe-signature: t=1710000000,v1=<signature>
Content-Type: application/json

{
  "id": "evt_123",
  "type": "checkout.session.completed",
  "data": {
    "object": {
      "customer": "cus_123",
      "amount_total": 4900,
      "currency": "usd"
    }
  }
}
```

### Polar Webhook Example

```http
POST /billing/webhook/polar
webhook-signature: sha256=<signature>
Content-Type: application/json

{
  "id": "evt_456",
  "type": "order.paid",
  "data": {
    "order_id": "ord_123",
    "product_id": "prod_abc"
  }
}
```

### LemonSqueezy Webhook Example

```http
POST /billing/webhook/lemon_squeezy
x-signature: <signature>
Content-Type: application/json

{
  "meta": {
    "event_name": "order_created"
  },
  "data": {
    "id": "1001",
    "attributes": {
      "status": "paid"
    }
  }
}
```

## Private Repository Endpoints

Routes in `satis/routes/repo.php`:

- `GET /repo/packages.json`
- `GET /repo/p2/{vendor}/{name}.json`
- `GET /repo/dist/{version}`

Middleware: `repo.auth`, `throttle:repository`.

### Composer Auth Example

`auth.json`:

```json
{
  "http-basic": {
    "your-domain.test": {
      "username": "LIC-ABCD-1234",
      "password": "buyer@example.com"
    }
  }
}
```

## Support Attachment Download

Route in `satis/routes/web.php`:

- `GET /satishub/support/attachments/{mediaUuid}`

Middleware: `web`, `signed`.

This endpoint is intended for generated temporary signed URLs, not direct public linking.
