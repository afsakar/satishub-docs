# 05 Data Model

## Catalog

- `packages`
- `package_versions`
- `products`
- `product_checkout_mappings`

Key intent:

- `packages`: repository identity (`vendor`, `name`) and visibility
- `products`: purchasable variants/pricing and checkout provider defaults
- `product_checkout_mappings`: per-provider external IDs

## Sales and Billing

- `orders`
- `customer_accounts`
- `webhook_events`

Key intent:

- `orders`: provider-neutral order ledger
- `customer_accounts`: provider customer identity links
- `webhook_events`: event idempotency and processing trail

## Licensing

- `licenses`
- `license_activations`
- `license_checks`

Key intent:

- `licenses`: entitlement source of truth
- `license_activations`: domain/app installation records
- `license_checks`: validation request history

## Support

- `support_tickets`
- `support_ticket_messages`

Support tables let you tie customer communications to product/license/order context.

## Additional

- `blocked_domains`
- `media` (Spatie Media Library)

## Notes

- `Product` and `Package` support `cover` media collections.
- `RepoVisibility` enum is defined in `satis/src/Enums/RepoVisibility.php`.

## Practical Relationship Example

One common chain:

- `Package` -> has many `Product`
- `Product` -> has many `License`
- `License` -> belongs to `Order`

This lets repository and API checks validate both commercial ownership and support policy.
