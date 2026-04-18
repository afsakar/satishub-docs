# 10 Operations

## Deployment Baseline

```bash
php artisan migrate --force
php artisan queue:work
php artisan storage:link
```

Recommended additional steps:

```bash
php artisan config:cache
php artisan route:cache
```

## Billing and Catalog Operations

```bash
php artisan satishub:sync-catalog --all --dry-run
php artisan satishub:sync-catalog --all --deactivate-missing --report
```

## Provider Webhook Operations Checklist

For each provider dashboard:

1. set endpoint to `POST /billing/webhook/{provider}`
2. set matching signing secret
3. confirm signature header and style in `satishub.php`
4. send test event and verify `webhook_events` row creation

## Package Version Sync

```bash
php artisan packages:sync-tags
```

## Media Maintenance

```bash
php artisan media-library:clean
```

## Operational Data Sources

- webhook idempotency and processing status: `webhook_events`
- license lifecycle: `licenses`, `license_checks`, `license_activations`
- support flow: `support_tickets`, `support_ticket_messages`

## Background Jobs

Keep queue workers running for async tasks (notifications, long-running operations).

```bash
php artisan queue:work --tries=1 --timeout=0
```
