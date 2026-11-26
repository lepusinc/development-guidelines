# Eloquent Model

This document defines guidelines for designing and implementing Eloquent models in Laravel applications.

## 1. Type Safety

- **[Required]** For values stored as specific strings in DB columns, such as `type` or `status`, create a dedicated Enum and configure a cast.

## 2. Query Builder

- **[Mandatory]** A Builder (and any scopes) must be limited to building queries only for its own table. If queries for related tables are needed, delegate to that table's Builder.
- **[Required]** Do not define scopes in model classes. Instead, create a dedicated Builder class and define them there.
- **[Mandatory]** Put query building (conditions, aggregation, ordering, etc.) in a Builder, not in model methods. This applies not only to scopes: do not build queries inside ordinary model methods either.
- **[Mandatory]** Put a condition in the Builder of the model whose table the condition applies to. Do not write conditions on a child table in the parent model (e.g. conditions on the child `children` table belong in the Builder of the child `ChildModel`, not in the parent `ParentModel`).
- **[Required]** When narrowing down children from a parent, call the Builder methods through the relation with method chaining. Group combined conditions into a Builder method with a meaningful name.

```php
// Bad: a method of the parent model builds conditions on the child table
class ParentModel extends Model
{
    public function activeChildren(): HasMany
    {
        return $this->children()
            ->where('status', ChildStatus::Active)
            ->whereNull('archived_at');
    }
}

// Good: conditions on the child table are grouped in the child model's Builder
class ChildModelBuilder extends Builder
{
    public function active(): static
    {
        return $this->where('status', ChildStatus::Active)
            ->whereNull('archived_at');
    }
}

// The child model returns that Builder from newEloquentBuilder()
class ChildModel extends Model
{
    public function newEloquentBuilder($query): ChildModelBuilder
    {
        return new ChildModelBuilder($query);
    }
}

// Caller: call through the relation with method chaining
$children = $parent->children()->active()->get();
```

- **[Required]** As a rule, define a condition or an ordering as a method on the Builder of the model whose table it applies to, and use that method. However, if a scope with the same meaning is already defined on the model, use that scope (do not define the same condition again on the Builder). This is an exception to "Do not define scopes in model classes": it only allows using a scope that already exists, and does not allow adding a new condition to the model as a scope. In either case, do not rewrite the same condition with `where()` or `orderBy()`. The same applies inside `whereHas()` and similar closures: use the related model's Builder methods (or scopes).

```php
// Bad: conditions that already exist as a scope or a Builder method are rewritten with where()
ParentModel::query()
    ->where('status', ParentStatus::Active)
    ->whereHas('children', fn ($query) => $query
        ->where('status', ChildStatus::Active)
        ->whereNull('archived_at'));

// Good: use the scope already defined on ParentModel (scopeActive()) and
// the method on ChildModel's Builder (active())
ParentModel::query()
    ->active()
    ->whereHas('children', fn ($query) => $query->active());
```

## 3. Collections

- **[Required]** When implementing custom logic that involves multiple models, create a custom collection and implement the logic there. This is limited to logic that can be fully contained within the collection's models (delegation to other service classes is not allowed; delegation to related models or Eloquent collections is acceptable).

## 4. Responsibility & Design

- **[Mandatory]** Using service classes in models or custom collections is strictly prohibited. It violates the boundaries of the ORM layer.
- **[Mandatory]** Stateful values on a model must not be settable directly from outside. Define dedicated methods for state transitions. The general principle is in [Domain Model (Entities)](../DOMAIN_MODEL.md). In Eloquent, apply it as follows.
  - Do not include state-related attributes (statuses such as `status`, results, active flags, timestamps that accompany a state change, etc.) in `$fillable`. They must not be settable via `create()` / `fill()` / `update()`.
  - Removing an attribute from `$fillable` does not prevent assignments like `$model->status = ...`. Do not assign state-related attributes from outside the model (services, controllers, etc.).
  - Accept values that accompany a state change (e.g. `requested_at`) as arguments of the dedicated state-transition method.
  - When a query builder bulk update (`update()`) changes state-related columns, encapsulate it inside a dedicated method of the model, custom collection, or Builder. Bulk updates do not fire model events (`updating` / `updated`, etc.), so Observer side effects ([7. Model Events](#7-model-events)) and audit logs are not executed. For state changes that need them, do not use a bulk update; change each model through its dedicated method.

```php
// Bad: a service sets the state and its accompanying value directly
$batch->status = BatchStatus::InProgress;
$batch->requested_at = now();
$batch->save();

// Good: the model's dedicated method checks the transition and records the accompanying value
$batch->markRequested(now());
```

## 5. Security

- **[Mandatory]** Do not use `$guarded = []`. Explicitly whitelist mass-assignable attributes using `$fillable`.
- **[Mandatory]** Do not use a sequential primary key as the route key. Exposing sequential IDs in URLs risks enumeration attacks. Use `getRouteKeyName()` to return a non-guessable value such as a UUID as the route key.
- **[Required]** Exclude attributes that must not be exposed during serialization (e.g., passwords) using `$hidden`.

## 6. Performance

- **[Required]** When accessing relations, use `with()` for eager loading to prevent N+1 query problems.
- **[Required]** Do not set default eager loading via `$with`. Unlike `with()`, which is called explicitly per query, `$with` triggers implicit eager loading on every model retrieval — executing additional `WHERE IN` queries for each relation regardless of whether the relation data is needed, which adds unnecessary query overhead.

## 7. Model Events

- **[Required]** Centralize side effects tied to model lifecycle events (e.g., sending notifications, logging) in an Observer. Do not write them directly in the model.

## 8. Configuration

- **[Mandatory]** When using non-incrementing primary keys such as UUIDs, explicitly set `$incrementing = false` and `$keyType = 'string'`.
- **[Required]** Use the `SoftDeletes` trait for models that require soft deletion.

## 9. Relationships

- **[Mandatory]** When defining a `belongsToMany()` relationship whose pivot table follows a domain-specific naming convention (i.e., not alphabetical order), explicitly pass the pivot table name as the second argument. Laravel resolves pivot table names alphabetically by default, so omitting the argument will result in the wrong table being referenced.

```php
// Laravel defaults to alphabetical order: http_communication_payment
// Domain-specific order requires explicit declaration:
public function httpCommunications(): BelongsToMany
{
    return $this->belongsToMany(HttpCommunication::class, 'payment_http_communication');
}
```
