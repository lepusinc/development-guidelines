# Repository

This document defines guidelines for designing the repository layer in Laravel applications.

## Fetching Query Results

- **[Mandatory]** Whether to use `->get()` (fetch all) or `->paginate()` (fetch paginated) is decided by the caller (e.g., the controller or the service layer). Do not provide separate "paginated" and "non-paginated" methods in the repository layer; instead, expose a single query-building method that returns a `Builder` (not a materialized `Collection` or `LengthAwarePaginator`), and let the caller call `->get()` or `->paginate()` on it.

  For implementation examples and rationale, see [Service](./SERVICE.md#fetching-query-results) — the same rule applies to both the repository and service layers.
