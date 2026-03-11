# Eloquent Model

This document defines guidelines for designing and implementing Eloquent models in Laravel applications.

## 1. Type Safety

- **[Required]** For values stored as specific strings in DB columns, such as `type` or `status`, create a dedicated Enum and configure a cast.

## 2. Query Builder

- **[Mandatory]** A Builder (and any scopes) must be limited to building queries only for its own table. If queries for related tables are needed, delegate to that table's Builder.
- **[Required]** Do not define scopes in model classes. Instead, create a dedicated Builder class and define them there.

## 3. Collections

- **[Required]** When implementing custom logic that involves multiple models, create a custom collection and implement the logic there. This is limited to logic that can be fully contained within the collection's models (delegation to other service classes is not allowed; delegation to related models or Eloquent collections is acceptable).

## 4. Responsibility & Design

- **[Mandatory]** Using service classes in models or custom collections is strictly prohibited. It violates the boundaries of the ORM layer.
- **[Mandatory]** Stateful values on a model must not be settable directly from outside. Define dedicated methods for state transitions.

## 5. Security

- **[Mandatory]** Do not use `$guarded = []`. Explicitly whitelist mass-assignable attributes using `$fillable`.
- **[Mandatory]** Do not use a sequential primary key as the route key. Exposing sequential IDs in URLs risks enumeration attacks. Use `getRouteKeyName()` to return a non-guessable value such as a UUID as the route key.
- **[Required]** Exclude attributes that must not be exposed during serialization (e.g., passwords) using `$hidden`.

## 6. Performance

- **[Required]** When accessing relations, use `with()` for eager loading to prevent N+1 query problems.
- **[Required]** Do not set default eager loading via `$with`. It causes a JOIN on every query and introduces unnecessary overhead.

## 7. Model Events

- **[Required]** Centralize side effects tied to model lifecycle events (e.g., sending notifications, logging) in an Observer. Do not write them directly in the model.

## 8. Configuration

- **[Mandatory]** When using non-incrementing primary keys such as UUIDs, explicitly set `$incrementing = false` and `$keyType = 'string'`.
- **[Required]** Use the `SoftDeletes` trait for models that require soft deletion.
