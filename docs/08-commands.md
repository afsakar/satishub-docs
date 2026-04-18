# 08 Commands

## satishub:sync-catalog

Purpose: sync local products and provider mappings from remote provider catalogs.

Usage:

```bash
php artisan satishub:sync-catalog [--provider=key] [--all] [--dry-run] [--deactivate-missing] [--report]
```

Important options:

- `--provider`: sync only one provider
- `--all`: sync all configured providers
- `--dry-run`: calculate changes without writing
- `--deactivate-missing`: deactivate local records missing in provider catalog
- `--report`: print combined provider summary

### Example 1: Safe Dry Run

```bash
php artisan satishub:sync-catalog --provider=stripe --dry-run --report
```

Use this to preview create/update/deactivate actions without database writes.

### Example 2: Full Multi-Provider Sync

```bash
php artisan satishub:sync-catalog --all --deactivate-missing --report
```

Run this after validating dry-run results and confirming provider credentials.

## packages:sync-tags

Purpose: sync package versions from GitHub tags.

Usage:

```bash
php artisan packages:sync-tags
```

This command synchronizes repository tags into `package_versions` and prepares distribution metadata.

## Recommended Workflow

```bash
php artisan satishub:sync-catalog --all --dry-run
php artisan satishub:sync-catalog --all --deactivate-missing --report
php artisan packages:sync-tags
```

## Exit Behavior Notes

- `dry-run` should complete with no writes.
- Any provider-level failure should be treated as a release blocker.
- Always run with `--report` in CI or staging for auditability.
