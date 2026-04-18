# 03 Configuration

## Main Config File

Primary package config lives in `config/satishub.php`.

Main sections:

- `user_model`
- `repository`
- `support.attachments`
- `licensing`
- `billing`

## Billing

Global keys:

- `BILLING_DEFAULT_PROVIDER`
- `BILLING_USE_DEFAULT_PROVIDER_GLOBALLY`
- `BILLING_WEBHOOK_PATH`

Provider keys are grouped under `satishub.billing.providers` for:

- Polar
- Stripe
- LemonSqueezy

Each provider includes API credentials and webhook signature options.

### Provider Config Example

`config/satishub.php` provider section pattern:

```php
'providers' => [
    'stripe' => [
        'label' => 'Stripe',
        'gateway' => \Afsakar\Satishub\Billing\Gateways\StripeCheckoutGateway::class,
        'api_base' => env('STRIPE_API_BASE', 'https://api.stripe.com'),
        'secret_key' => env('STRIPE_SECRET'),
        'customer_creation' => env('STRIPE_CUSTOMER_CREATION', 'always'),
        'webhook' => [
            'secret' => env('STRIPE_WEBHOOK_SECRET'),
            'signature_header' => env('STRIPE_WEBHOOK_SIGNATURE_HEADER', 'stripe-signature'),
            'signature_algorithm' => env('STRIPE_WEBHOOK_SIGNATURE_ALGORITHM', 'sha256'),
            'signature_style' => env('STRIPE_WEBHOOK_SIGNATURE_STYLE', 'stripe'),
        ],
    ],
],
```

### Webhook Processor Mapping Example

```php
'webhook_processors' => [
    'stripe' => \Afsakar\Satishub\Services\Billing\StripeWebhookService::class,
    'polar' => \Afsakar\Satishub\Services\Billing\PolarWebhookService::class,
    'lemon_squeezy' => \Afsakar\Satishub\Services\Billing\LemonSqueezyWebhookService::class,
],
```

### Catalog Sync Driver Mapping Example

```php
'catalog_sync_drivers' => [
    'stripe' => \Afsakar\Satishub\Billing\CatalogSync\Drivers\StripeCatalogSyncDriver::class,
    'polar' => \Afsakar\Satishub\Billing\CatalogSync\Drivers\PolarCatalogSyncDriver::class,
    'lemon_squeezy' => \Afsakar\Satishub\Billing\CatalogSync\Drivers\LemonSqueezyCatalogSyncDriver::class,
],
```

### Order Billing Link Resolver Mapping Example

Order invoice/receipt links are resolved per provider. You can plug custom logic with:

```php
'order_link_resolvers' => [
    'my_provider' => \App\Billing\MyProviderOrderLinkResolver::class,
],
```

Resolver class expectations:

- class is container-resolvable
- either callable (`__invoke(Order $order): array`) or has `resolve(Order $order): array`
- returns an array with optional keys:
  - `invoice_url`
  - `receipt_url`
  - `customer_portal_url`

## Adding a Custom Provider

To add `my_provider`, configure four layers:

1. `billing.providers.my_provider.gateway`
2. `billing.webhook_processors.my_provider`
3. `billing.catalog_sync_drivers.my_provider`
4. `billing.order_link_resolvers.my_provider` (recommended for invoice/receipt links)

You can keep core package code unchanged if your classes honor package contracts.

## Repository and Docs

- `REPOSITORY_GITHUB_TOKEN`
- `REPOSITORY_ARTIFACTS_DISK`
- `REPOSITORY_DIST_URL_TTL_MINUTES`
- `REPOSITORY_DOCS_MAX_FILE_BYTES`
- `REPOSITORY_DOCS_MAX_BUNDLE_BYTES`
- `REPOSITORY_DOCS_MAX_BUNDLE_DOCUMENTS`

## License Key Generation

License key generation is configurable via:

- `satishub.licensing.key_generator`

This value must be a container-resolvable class that implements:

- `Afsakar\Satishub\Services\Licensing\Contracts\LicenseKeyGenerator`

Default generator:

- `Afsakar\Satishub\Services\Licensing\DefaultLicenseKeyGenerator`

Example:

```php
'licensing' => [
    'key_generator' => \App\Licensing\MyLicenseKeyGenerator::class,
],
```

## Support Attachments

Attachment constraints are configurable with:

- `satishub.support.attachments.max_files`
- `satishub.support.attachments.max_file_kb`
- `satishub.support.attachments.allowed_mime_types`

Support admin recipients can be configured with:

- `satishub.support.admin_model`
- `satishub.support.admin_guard`

Fallback order used by the service:

1. `satishub.support.admin_model`
2. `satishub.user_model`

This allows installations without a dedicated `Admin` model to continue with the user model.

If `admin_model` is set to your `User` model, you can also set `admin_guard=web` to treat web-authenticated users as support admins in admin-only support actions.

## Minimum Environment Template

```dotenv
BILLING_DEFAULT_PROVIDER=stripe
BILLING_USE_DEFAULT_PROVIDER_GLOBALLY=false
BILLING_WEBHOOK_PATH=billing/webhook/{provider}

STRIPE_SECRET=
STRIPE_WEBHOOK_SECRET=

POLAR_ACCESS_TOKEN=
POLAR_WEBHOOK_SECRET=

LEMON_SQUEEZY_API_KEY=
LEMON_SQUEEZY_STORE=
LEMON_SQUEEZY_WEBHOOK_SECRET=

REPOSITORY_GITHUB_TOKEN=
REPOSITORY_ARTIFACTS_DISK=local
```
