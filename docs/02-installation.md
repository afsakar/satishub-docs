# 02 Installation

## Install via Composer

```bash
composer require afsakar/satishub
```

The package provider is auto-discovered.

## Publish Configuration

```bash
php artisan vendor:publish --tag=satishub-config
```

This publishes `config/satishub.php` to the host app.

## Run Migrations

```bash
php artisan migrate
```

## Register Required Middleware Alias

In `bootstrap/app.php`:

```php
$middleware->alias([
    'repo.auth' => \Afsakar\Satishub\Http\Middleware\AuthenticateComposerBasicAuth::class,
]);
```

Without this alias, private repository routes cannot authenticate Composer clients.

## Register Filament Plugin

In your panel provider:

```php
->plugin(\Afsakar\Satishub\Filament\SatisPlugin::make())
```

## First Boot Checklist

1. Create at least one Package and Product in admin.
2. Add provider mapping(s) for external price/product IDs.
3. Configure webhook endpoint in your provider dashboard.
4. Run initial catalog sync in dry-run mode.

## Customer Composer Setup Example

Your customer typically adds your repository URL and credentials (license key + buyer email).

`composer.json` example:

```json
{
  "repositories": [
    {
      "type": "composer",
      "url": "https://your-domain.test/repo"
    }
  ]
}
```

`auth.json` example:

```json
{
  "http-basic": {
    "your-domain.test": {
      "username": "LIC-XXXX-XXXX",
      "password": "buyer@example.com"
    }
  }
}
```

Order of license/email can be flexible in auth parsing, but this format is recommended.
