# Exception Handling / Logging

This document defines guidelines for exception handling and logging in Laravel applications.

## Basic Policy

- Centralize exception responses, command output, and logging in `app/Exceptions/Handler.php` (HTTP via `render()`, console via `renderForConsole()`, logging via `report()`). Do not catch exceptions within application code for the purpose of handling responses or logging.
- The following uses of exception catching are permitted:
  - Rolling back transactions
  - Closing files or resources
  - Returning `false` or a fallback value for expected exceptions (business logic branching)
- If exception responses exist that are not delegated to the Handler, consider revisiting the responsibility boundaries and exception design.
- It is absolutely not permitted to catch and swallow exceptions solely for the purpose of changing UX. Response control must be handled in the Handler.

## Exception Class Design

- Create custom exception classes for domain-specific errors and express their meaning through the class name (e.g., `PaymentFailedException`, `ResourceNotFoundException`).
- Change exception log levels by mapping them in `$levels` in the Handler. Both class names and interface names are accepted as keys.

```php
protected $levels = [
    ValidationException::class  => LogLevel::INFO,
    WarningLoggable::class      => LogLevel::WARNING,  // interface names are also accepted
    ExternalApiException::class => LogLevel::ERROR,
];
```

## Log Level Guidelines

| Level | Intended Use |
|---|---|
| `INFO` | Expected errors caused by the user, such as validation errors |
| `WARNING` | Business rule violations, retryable external service errors, etc. |
| `ERROR` | System failures, data inconsistencies requiring immediate attention |
| `CRITICAL` | Failures that render the service inoperable |

## Response

- Centralize HTTP response conversion in `Handler`'s `render()` or `renderable()`.
- Unify the error response format (structure and field names) across the entire application.
- Do not include stack traces or internal information in production responses.
