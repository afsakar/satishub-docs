# 09 Testing

## Test Framework

- Pest 4
- PHPUnit 12

## Package Test Location

- `satis/tests/Feature`

## What Is Covered

- webhook route and signature behavior
- provider webhook processors
- catalog sync command flows
- repository usage activation checks
- support ticket flow and attachment permissions
- docs API responses and readme loading
- media cover single-file behavior

## Run All Package Feature Tests

```bash
php artisan test --compact satis/tests/Feature
```

## Common Targeted Suites

```bash
php artisan test --compact satis/tests/Feature/SyncCatalogCommandTest.php
php artisan test --compact satis/tests/Feature/ProviderWebhookProcessorsTest.php
php artisan test --compact satis/tests/Feature/SupportTicketFlowTest.php
php artisan test --compact satis/tests/Feature/ProductPackageCoverMediaTest.php
```

Additional examples:

```bash
php artisan test --compact satis/tests/Feature/BillingWebhookRouteTest.php
php artisan test --compact satis/tests/Feature/LemonSqueezySyncCatalogCommandTest.php
php artisan test --compact satis/tests/Feature/StripeSyncCatalogCommandTest.php
```

## Formatting

When PHP files change, run:

```bash
vendor/bin/pint --dirty --format agent
```
