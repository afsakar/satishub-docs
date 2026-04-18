# 02 Installation

This package requires a valid license to use.
You can get your license here: [SatisHub](https://afsakar.lemonsqueezy.com/checkout/buy/f2fb0179-c8cd-4be6-bb0f-677381578fe8)

To install you'll need to add the repository to your composer.json file:

```bash
{
    "repositories": [
        {
            "type": "composer",
            "url": "https://satis.afsakar.com"
        }
    ]
}
```

Once the repository has been added to the composer.json file, you can install SatisHub like any other composer package using the composer require command:

```bash
composer require afsakar/satishub
```

You will be prompted to provide your username and password. The username will be the email address and the password will be equal to your license key.

```bash
Loading composer repositories with package information
Authentication required (satis.afsakar.com):
Username: [license-email]
Password: [license-key]
```
Next, add the plugin's views to your custom theme in your theme.css file:

```css
@source '../../../../vendor/afsakar/satishub/resources/views/**/*.blade.php';
@source '../../../../vendor/afsakar/satishub/src/**/*.php';
```
Afterward, run `npm run build` or `yarn build` to compile assets.

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
