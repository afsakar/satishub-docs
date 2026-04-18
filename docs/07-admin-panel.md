# 07 Admin Panel

## Filament Integration

The package exposes Filament resources through `SatisPlugin`.

Host app responsibility:

- register plugin in panel provider
- ensure panel auth guard is configured (`admin` is common in this repo)

## Resource Groups

Under `satis/src/Filament/Resources`:

- Packages
- Products
- Licenses
- Orders
- SupportTickets
- Users
- BlockedDomains

## Typical Admin Workflow

1. Create a Package (`vendor`, `name`, repository URL, visibility).
2. Create one or more Product entries under the package.
3. Attach external provider IDs in mapping records.
4. Sync external catalog IDs using command if needed.
5. Monitor Orders and generated Licenses.

## Product/Package Cover Images

Models support a single-file `cover` media collection.

- forms use `SpatieMediaLibraryFileUpload`
- tables use `SpatieMediaLibraryImageColumn`

Cover images can also be used by storefront-like pages in the host app.

## Media Uploads

Product and Package forms use Spatie Media Library plugin components for cover images.

- upload component: `SpatieMediaLibraryFileUpload`
- table image column: `SpatieMediaLibraryImageColumn`

## Localization

Resource labels and UI text are under:

- `satis/resources/lang/en/resources.php`
- `satis/resources/lang/tr/resources.php`

## Visibility Enum

Repository visibility options are represented by:

- `satis/src/Enums/RepoVisibility.php`

This enum includes display labels and UI metadata (color and icon) for Filament usage.
