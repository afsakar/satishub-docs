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

## Screen Gallery

### Dashboard

![Admin dashboard](https://raw.githubusercontent.com/afsakar/satishub-docs/main/assets/dashboard.png)

### Package and Product Management

![Package details](https://raw.githubusercontent.com/afsakar/satishub-docs/main/assets/package-details.png)
![Custom frontend packages](https://raw.githubusercontent.com/afsakar/satishub-docs/main/assets/custom-frontend-packages.png)
![Custom frontend docs](https://raw.githubusercontent.com/afsakar/satishub-docs/main/assets/custom-frontend-docs.png)

### Order and License Flow

![Order details](https://raw.githubusercontent.com/afsakar/satishub-docs/main/assets/order-details.png)
![License details](https://raw.githubusercontent.com/afsakar/satishub-docs/main/assets/license-details.png)

### User and Support Management

![User details](https://raw.githubusercontent.com/afsakar/satishub-docs/main/assets/user-details.png)
![Support ticket list/detail](https://raw.githubusercontent.com/afsakar/satishub-docs/main/assets/support-ticket-view.png)
![Support ticket reply](https://raw.githubusercontent.com/afsakar/satishub-docs/main/assets/support-ticket-reply.png)

### Product Preview Video

[Watch product preview video](https://github.com/user-attachments/assets/984715a4-7c03-45ca-b98c-0384d391ab5b)

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
