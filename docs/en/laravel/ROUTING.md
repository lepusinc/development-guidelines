# Routing

This document defines guidelines for routing in Laravel applications.

## 1. Naming Conventions

- **[Required]** Use kebab-case for URLs (e.g., `/user-profiles`).
- **[Required]** Do not include verbs in URLs. Express operations through HTTP methods (`GET /users` ✓, `GET /getUsers` ✗).
- **[Required]** Assign a route name via `->name()` to every route. Avoid referencing raw URL strings; use the `route()` helper instead.

## 2. Design & Structure

- **[Mandatory]** Route files must contain only route definitions. Do not write business logic in closures. Routes with closures cannot be cached, which degrades production performance.
- **[Required]** Assign exactly one controller action per route (using an Invocable Controller or a Resource Controller action method).
- **[Required]** Group routes that share common middleware, prefixes, or namespaces.
- **[Recommended]** Use `Route::resource` / `Route::apiResource` for CRUD operations to enforce RESTful conventions.

## 3. Security

- **[Mandatory]** Apply the `auth` middleware to all routes that require authentication without exception. Do not rely solely on authentication checks inside controllers.
- **[Required]** Do not rely only on authorization (policies) inside controllers. Combine with route-level middleware for defense in depth.
