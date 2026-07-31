# Feature Test Guidelines

## Purpose & Scope

- Targets the application's overall behavior through a path close to end-to-end, such as HTTP requests or the execution of CLI commands.
- Purpose: guarantee that the actual request-to-response flow — including routing, middleware, controllers, and the DB — or a command's run-to-exit-status behavior and side effects (DB updates, output content, etc.) behave as users expect.
- **[Required]** Delegate branch coverage and boundary-value verification of individual logic to unit tests; feature tests focus on the happy path and representative error cases (e.g., typical error responses or a command's abnormal exit).

## Topic Guidelines

- [Validation Testing](./VALIDATION.md)

(Other feature test guidelines will be described here.)
