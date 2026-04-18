# 12 AI Context Files

## Purpose

This project includes two AI-oriented context files:

- `llms.txt`
- `llms-full.txt`

They provide compact and extended technical context for coding agents.

## File Roles

## `llms.txt` (quick context)

Use for fast orientation. It should include:

- package purpose and scope
- current route surface
- core invariants and safety constraints
- key commands and config anchors

## `llms-full.txt` (extended context)

Use for deeper implementation work. It should include:

- architecture snapshot
- provider model (Stripe, Polar, LemonSqueezy)
- detailed business rules
- operational and testing guidance

## When to Update

Update these files whenever one of the following changes:

- route contracts
- provider support/model
- command names/options
- security/access behavior
- core domain invariants
- important config keys

## Maintenance Rules

- Keep both files aligned with `docs/*` and `README.md`.
- Keep `llms.txt` concise; avoid implementation noise.
- Keep `llms-full.txt` detailed but practical.
- Remove outdated references immediately (for example old command or config names).

## Recommended Review Checklist

1. Confirm routes against `routes/*`.
2. Confirm commands with `php artisan list --raw` and `--help` output.
3. Confirm config keys from `config/satishub.php`.
4. Confirm provider and webhook behavior from current services.
5. Confirm docs index in `satis/README.md` includes this page.
