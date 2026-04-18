# 11 Troubleshooting

## Webhook Signature Failures

Symptom: webhook returns 401.

Check:

- provider webhook secret
- signature header name
- signature algorithm and style
- provider endpoint path matches `BILLING_WEBHOOK_PATH`

Quick verification command:

```bash
php artisan route:list --path=billing/webhook
```

## Catalog Sync Issues

Symptom: sync command fails or unexpected deactivations occur.

Check:

- provider exists in `catalog_sync_drivers`
- run with `--dry-run` first
- verify targeted provider (`--provider` vs `--all`)

Suggested recovery flow:

```bash
php artisan satishub:sync-catalog --provider=stripe --dry-run --report
php artisan satishub:sync-catalog --provider=stripe --deactivate-missing --report
```

## Repository Access Denied

401 usually means credentials are wrong.
403 usually means business constraints failed.

Check:

- license key + email pair
- license status and support validity
- requested package belongs to licensed product

Also check customer Composer configuration (`auth.json`) format.

## Missing Docs/README Content

Check:

- `REPOSITORY_GITHUB_TOKEN`
- package repository URL
- docs limits in `satishub.repository.docs`

If using private GitHub repositories, ensure token scope is sufficient for content access.

## Missing Cover Images

Check:

- media records exist in `media` table
- storage disk configuration
- public symlink (`php artisan storage:link`) where needed

## Support Attachment Download Problems

If attachment links fail:

- verify signed URL has not expired
- verify media UUID exists
- verify user is ticket owner or admin according to access rules
