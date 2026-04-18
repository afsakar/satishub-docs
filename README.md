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

- [Introduction](/docs/01-introduction.md)
- [Installation](/docs/02-installation.md)
- [Configuration](/docs/03-configuration.md)
- [Architecture](/docs/04-architecture.md)
- [Data Model](/docs/05-data-model.md)
- [API Reference](/docs/06-api-reference.md)
- [Admin Panel](/docs/07-admin-panel.md)
- [Commands](/docs/08-commands.md)
- [Testing](/docs/09-testing.md)
- [Operations](/docs/10-operations.md)
- [Troubleshooting](/docs/11-troubleshooting.md)
- [AI Context Files](/docs/12-ai-context-files.md)
