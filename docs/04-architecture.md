# 04 Architecture

## Service Provider Responsibilities

`SatisServiceProvider` is the package runtime hub. It:

- merges package config
- loads package routes, migrations, views, and translations
- registers commands in console runtime
- configures rate limiters for API and repository access

## Core Modules

- Billing: `satis/src/Billing`
- HTTP APIs: `satis/src/Http/Controllers/Api`
- Repository endpoints: `satis/src/Http/Controllers/Repository`
- Domain models: `satis/src/Models`
- Filament resources: `satis/src/Filament/Resources`

## Billing Design

- Checkout is delegated by `CheckoutManager` and provider registries.
- Webhooks are validated and forwarded to provider-specific processors.
- Catalog sync is driver-based (`catalog_sync_drivers`) and provider-isolated.

### Checkout Flow

1. Product selected by customer.
2. `CheckoutManager` resolves provider key.
3. Gateway class for provider creates checkout URL/session.
4. Customer is redirected to provider checkout.

### Webhook Flow

1. Provider sends event to `/billing/webhook/{provider}`.
2. Signature verification is applied from provider config.
3. Processor class handles payload normalization.
4. Order/license state is updated.
5. Idempotency and audit metadata are tracked in `webhook_events`.

## Repository Access Model

Repository endpoints are not public package feeds. Access is entitlement-based.

Checks include:

- valid basic auth credentials (license + email)
- active license status
- support validity (when policy requires)
- product-package ownership relationship

## Support Attachment Security

- URLs are signed and temporary.
- Route key uses media UUID (`{mediaUuid}`).
- Download action enforces ticket-level ownership and role rules.

## Distribution Security

- Repository routes are protected by `repo.auth` middleware.
- Dist downloads are signed and time-limited.
- License and support validity control access.

## Domain Events

The package dispatches domain events for integration use-cases.

- `Afsakar\Satishub\Events\Billing\OrderCreated`
- `Afsakar\Satishub\Events\Billing\LicenseCreated`
- `Afsakar\Satishub\Events\Billing\WebhookEventProcessed`
- `Afsakar\Satishub\Events\Support\SupportTicketOpened`
- `Afsakar\Satishub\Events\Support\SupportTicketReplied`

Typical usage in host apps:

- analytics pipelines
- external billing/CRM sync
- custom notification channels
- provisioning or entitlement side effects
