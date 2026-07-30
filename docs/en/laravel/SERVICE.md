# Service

This document defines guidelines for designing the service layer in Laravel applications.

## Fetching Query Results

- **[Mandatory]** Whether to use `->get()` (fetch all) or `->paginate()` (fetch paginated) is decided by the caller (e.g., the controller). Do not provide separate "paginated" and "non-paginated" methods in the repository/service layer; instead, expose a single query-building method that returns a `Builder` (not a materialized `Collection` or `LengthAwarePaginator`), and let the caller call `->get()` or `->paginate()` on it. This rule applies equally to the [Repository](./REPOSITORY.md) layer.

```php
// ❌ Wrong: duplicating a get() version and a paginate() version as separate methods
interface FooRepositoryInterface
{
    public function findByParams(array $params): Collection;
    public function findByParamsWithPaginator(array $params, int $limit): LengthAwarePaginator;
    public function buildQueryByParams(array $params): Builder;
}

class FooRepository implements FooRepositoryInterface
{
    public function findByParams(array $params): Collection
    {
        return $this->buildQueryByParams($params)->get();
    }

    public function findByParamsWithPaginator(array $params, int $limit): LengthAwarePaginator
    {
        return $this->buildQueryByParams($params)->paginate($limit);
    }

    public function buildQueryByParams(array $params): Builder
    {
        // ...
    }
}

// ✅ Correct: only expose query building; let the caller decide get() vs. paginate()
interface FooRepositoryInterface
{
    public function buildQueryByParams(array $params): Builder;
}

class FooRepository implements FooRepositoryInterface
{
    public function buildQueryByParams(array $params): Builder
    {
        // ...
    }
}

// The caller (e.g., the controller) decides
$query = $repository->buildQueryByParams($params);
$items = $query->paginate($perPage); // or $query->get();
```

Why:

- A `->get()` version and a `->paginate()` version differ only in what is called after the query is built — the query-building logic itself is identical. Duplicating this as separate methods means every time a condition is added or changed, both methods must be updated in sync, which is a breeding ground for bugs where only one of them gets fixed.
- Whether pagination is needed, and at what page size (`per_page`), is inherently a concern of the request (the screen or API use case), not a concern of the repository/service layer, whose responsibility is data retrieval logic. To avoid mixing layer responsibilities, this decision is left to the caller.
