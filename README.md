# SatisHub Package

<center>
<img src="https://raw.githubusercontent.com/afsakar/satishub-docs/main/assets/satishub.jpeg" alt="SatisHub Logo" />
</center>

SatisHub is a Laravel package for selling private Composer packages with license-based access control.

It includes:

- multi-provider checkout (Stripe, Polar, LemonSqueezy)
- webhook processing and order/license lifecycle handling
- private Composer repository authentication
- license activation/validation APIs
- Filament resources for package operations

## Preview

[![Watch SatisHub preview](https://raw.githubusercontent.com/afsakar/satishub-docs/main/assets/satishub.jpeg)](https://raw.githubusercontent.com/afsakar/satishub-docs/main/assets/satishub-preview.MP4)

## Quick Start

```bash
composer require afsakar/satishub
php artisan vendor:publish --tag=satishub-config
php artisan migrate
```

Host app requirements:

- register `repo.auth` alias in `bootstrap/app.php`
- register `\Afsakar\Satishub\Filament\SatisPlugin::make()` in your panel provider

## Documentation

All detailed setup, configuration, provider/webhook examples, API usage, operations, and troubleshooting are in `docs`:

- [Introduction](https://github.com/afsakar/satishub-docs/blob/main/docs/01-introduction.md)
- [Installation](https://github.com/afsakar/satishub-docs/blob/main/docs/02-installation.md)
- [Configuration](https://github.com/afsakar/satishub-docs/blob/main/docs/03-configuration.md)
- [Architecture](https://github.com/afsakar/satishub-docs/blob/main/docs/04-architecture.md)
- [Data Model](https://github.com/afsakar/satishub-docs/blob/main/docs/05-data-model.md)
- [API Reference](https://github.com/afsakar/satishub-docs/blob/main/docs/06-api-reference.md)
- [Admin Panel](https://github.com/afsakar/satishub-docs/blob/main/docs/07-admin-panel.md)
- [Commands](https://github.com/afsakar/satishub-docs/blob/main/docs/08-commands.md)
- [Testing](https://github.com/afsakar/satishub-docs/blob/main/docs/09-testing.md)
- [Operations](https://github.com/afsakar/satishub-docs/blob/main/docs/10-operations.md)
- [Troubleshooting](https://github.com/afsakar/satishub-docs/blob/main/docs/11-troubleshooting.md)
- [AI Context Files](https://github.com/afsakar/satishub-docs/blob/main/docs/12-ai-context-files.md)
