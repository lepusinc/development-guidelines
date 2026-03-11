# Migration

This document defines guidelines for creating migration files in Laravel applications.

## 1. Safety

- Always implement `down()` that fully rolls back the changes made in `up()`. Write `down()` to be symmetric with `up()`.
- When dropping columns or tables, include the corresponding model and relation changes in the same PR.
- When adding a NOT NULL column to an existing table, include a `default` value or a data backfill process.
- Changes that affect backward compatibility, such as adding or removing `nullable()`, should be split into staged migrations considering the deployment order.
- Never edit a migration file that has already been executed in production. Create a new migration file if changes are needed.
- Do not mix DDL (schema changes) and DML (inserts, updates, deletes) in migrations — separate data operations into Seeders.
- Use `dateTime()` instead of `timestamp()` for date/time columns. `timestamp()` in MySQL causes UTC conversion issues and the year-2038 problem, while `dateTime()` is safely mapped to the appropriate type for each DB by Laravel's schema builder.
- Set `default(false)` / `default(0)` on permission and status flag columns such as `is_admin` and `is_active`. Designing with no permissions by default prevents unintended privilege escalation.

## 2. Performance

- When modifying tables with large amounts of data, implement with lock time in mind.
- When adding a foreign key constraint, add an index to the target column.

## 3. Maintainability

- Avoid using the `ENUM` type; use `string` or integer columns with constants instead. `ENUM` requires an `ALTER TABLE` to add or remove values, which may involve table recreation in some databases and results in high change costs.

## 4. Naming Conventions

- Use snake_case consistently for table names and column names (e.g., `user_profiles`, `created_at`).
- Match the naming pattern of existing migration files.
- Follow the `YYYY_MM_DD_HHMMSS_<verb>_<table_name>_table.php` format for file names (e.g., `2024_01_15_093000_create_users_table.php`).
- Prefix with a verb that describes the action: `create_` / `add_` / `remove_` / `rename_` / `drop_`.
- Name pivot tables in singular form, placing the table name of the entity that is the subject of the relationship (the more domain-specific, lower-abstraction entity) first (e.g., the pivot table for `payments` and `http_communications` is `payment_http_communication`).

## 5. Idempotency

- Verify that you are not attempting to recreate columns or indexes that already exist.

## 6. Portability

- Avoid writing in a way that depends on a specific DB software.
