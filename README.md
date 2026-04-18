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

- [Introduction](https://raw.githubusercontent.com/afsakar/satishub-docs/main/docs/01-introduction.md)
- [Installation](https://raw.githubusercontent.com/afsakar/satishub-docs/main/docs/02-installation.md)
- [Configuration](https://raw.githubusercontent.com/afsakar/satishub-docs/main/docs/03-configuration.md)
- [Architecture](https://raw.githubusercontent.com/afsakar/satishub-docs/main/docs/04-architecture.md)
- [Data Model](https://raw.githubusercontent.com/afsakar/satishub-docs/main/docs/05-data-model.md)
- [API Reference](https://raw.githubusercontent.com/afsakar/satishub-docs/main/docs/06-api-reference.md)
- [Admin Panel](https://raw.githubusercontent.com/afsakar/satishub-docs/main/docs/07-admin-panel.md)
- [Commands](https://raw.githubusercontent.com/afsakar/satishub-docs/main/docs/08-commands.md)
- [Testing](https://raw.githubusercontent.com/afsakar/satishub-docs/main/docs/09-testing.md)
- [Operations](https://raw.githubusercontent.com/afsakar/satishub-docs/main/docs/10-operations.md)
- [Troubleshooting](https://raw.githubusercontent.com/afsakar/satishub-docs/main/docs/11-troubleshooting.md)
- [AI Context Files](https://raw.githubusercontent.com/afsakar/satishub-docs/main/docs/12-ai-context-files.md)
