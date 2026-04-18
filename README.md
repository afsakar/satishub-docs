# SatisHub Package

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

- `docs/01-introduction.md`
- `docs/02-installation.md`
- `docs/03-configuration.md`
- `docs/04-architecture.md`
- `docs/05-data-model.md`
- `docs/06-api-reference.md`
- `docs/07-admin-panel.md`
- `docs/08-commands.md`
- `docs/09-testing.md`
- `docs/10-operations.md`
- `docs/11-troubleshooting.md`
- `docs/12-ai-context-files.md`
